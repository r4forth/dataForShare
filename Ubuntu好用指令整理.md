# 更新套件列表與套件

```
# 更新套件列表
sudo apt update

# 更新套件
sudo apt upgrade -y
```

# 設定網路對時

```
sudo apt install -y ntpdate
sudo ntpdate clock.stdtime.gov.tw
```
# 設定 OpenSSH

```
# 安裝 OpenSSH-server
sudo apt install -y openssh-server

# 修改設定值
sudo sed -i '32s/#//' /etc/ssh/sshd_config
sudo sed -i '32s/prohibit-password/no/' /etc/ssh/sshd_config

# 重啟服務
sudo systemctl restart ssh
```

# 建立使用者

```
sudo adduser daniel
```

# 將某個使用者加入到 sudo 群組

```
sudo usermod -G sudo daniel

# 測試是否能執行 sudo 權限的指令
su - daniel
sudo reboot
```

# 刪除使用者帳號

```
# 刪除使用者帳號，但不刪掉家目錄
sudo deluser daniel

# 刪除使用者帳號與家目錄
sudo deluser daniel --remove-home
```
# 設定固定IP

```
sudo vi /etc/netplan/01-netcfg.yaml
# ====================================================
network:
  version: 2
  renderer: networkd
  ethernets:
    ens3:
      dhcp4: no
      # IP address/subnet mask
      addresses: [172.16.2.30/24]
      # default gateway
      gateway4: 172.16.2.254
      nameservers:
        # name server this host refers
        addresses: [172.16.2.1,172.16.2.2]
      dhcp6: no
# ====================================================
# 讓命令生效
sudo netplan apply
# 檢視 IP 
ip a
# 不使用 IPV6
sudo echo "net.ipv6.conf.all.disable_ipv6 = 1" >> /etc/systel.conf
# 檢視是否關閉 IPv6
sudo sysctl -p
```

# 檢視目前執行服務

```
# 檢視正在執行的服務
sudo systemctl -t service

# 停止服務
sudo systemctl stop 服務名稱

# 關閉服務
sudo systemctl disable 服務名稱
```

# 檢視使用者所屬群組

* [查看user屬於那一個group ](https://go-linux.blogspot.com/2011/11/tips-usergroup.html)

```
sophia@ideapad:~$ groups sophia
sophia : sophia adm dialout cdrom sudo dip plugdev lpadmin sambashare
```

# 

# 登入樹梅派使用序列埠

* [從序列埠登入到 Raspberry Pi](https://www.raspberrypi.com.tw/1999/connect-to-raspberry-pi-via-serial/)

```
sophia@ideapad:~$ ls /dev/ttyUSB*
/dev/ttyUSB0
# 將使用者加入 dialout 群組
# 指令執行完後，登出系統再登入，使功能生效。
sophia@ideapad:~$ sudo usermod -G dialout -a sophia
# 確認使用者已可使用 serial port
sophia@ideapad:~$ groups sophia | grep dialout
sophia : sophia adm dialout cdrom sudo dip plugdev lpadmin sambashare
```

# 安裝 VirtualBox 

```
# VirtualBox 
# 安裝必需的軟體
LINUX_HEADERS=$(uname -r)
sudo apt -y install gcc make linux-headers-$LINUX_HEADERS dkms

# 安裝 VirtualBox 6
# 修改 /etc/apt/sources.list
sudo sed -i '$ a\# VirtualBox Source' /etc/apt/sources.list
sudo sed -i '$ a\deb https://download.virtualbox.org/virtualbox/debian bionic contrib' /etc/apt/sources.list
# 下載 VirtualBox 套件庫金鑰
wget https://www.virtualbox.org/download/oracle_vbox_2016.asc 
sudo apt-key add oracle_vbox_2016.asc 
# 更新 套件庫
sudo apt update 
# 安裝 VirtualBox 6.0
sudo apt -y install virtualbox-6.0
# 檢視安裝 VirtualBox 版本
VBoxManage -v 
# 安裝 extension packages
wget https://download.virtualbox.org/virtualbox/6.0.4/Oracle_VM_VirtualBox_Extension_Pack-6.0.4.vbox-extpack
echo y | sudo VBoxManage extpack install Oracle_VM_VirtualBox_Extension_Pack-6.0.4.vbox-extpack
# 檢視延伸套件版本
VBoxManage list extpacks 
```

# 安裝防毒軟體

```
sudo apt install -y clamav

# 修改設定擋
sudo sed -i -e "s/^NotifyClamd/#NotifyClamd/g" /etc/clamav/freshclam.conf 
sudo systemctl stop clamav-freshclam

# 手動更新病毒定義檔
sudo freshclam

# 啟動防毒服務
sudo systemctl start clamav-freshclam

```


# 安裝 R 環境

```
# 如果是用 mini.iso 安裝系統，需要額外安裝 gnupg 套件
sudo apt-get install -y gnupg
# 加入公鑰
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
# 加入套件來源
# 如果是用 mini.iso 安裝系統，需要額外安裝其他套件才能支援 add-apt-repository 
sudo apt -y install software-properties-common dirmngr apt-transport-https lsb-release ca-certificates
# 加入 R 套件庫，選用元智大學
sudo add-apt-repository 'deb https://ftp.yzu.edu.tw/CRAN/bin/linux/ubuntu bionic-cran35/'
# 更新系統與安裝 R 環境
sudo apt-get update
sudo apt-get install -y r-base libcurl4-openssl-dev libxml2-dev
sudo apt-get autoremove -y
```

# 安裝 RStudioServer 1.2 預覽版

```
wget https://s3.amazonaws.com/rstudio-ide-build/server/bionic/amd64/rstudio-server-1.2.1327-amd64.deb
sudo apt-get install -y gdebi-core
sudo gdebi -n rstudio-server-1.2.1327-amd64.deb
```

# 支援 Serial port 存取

```
sudo usermod -aG dialout $USER
```

# R 小問題處理

```
# 支援 TrueType 字型 freetype-config
sudo apt-get install -y libfreetype6-dev
``` 

# 安裝 Julia 環境

```
wget https://julialang-s3.julialang.org/bin/linux/x64/1.0/julia-1.0.3-linux-x86_64.tar.gz
tar zxvf julia-1.0.3-linux-x86_64.tar.gz
mv julia-1.0.3 julia
sed -i '$ a\export PATH="$HOME/julia/bin:$PATH"' ~/.bashrc
source ~/.bashrc
```


# 安裝處理 Jupyter Notebook 環境

使用套件管理系統: Miniconda

```
# 安裝 Miniconda
# 64 位元版本
wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash ~/Miniconda3-latest-Linux-x86_64.sh -b -p $HOME/miniconda
# 新增環境變數路徑
sed -i '$ a\export PATH="$HOME/miniconda/bin:$PATH"' ~/.bashrc
# 環境變數啟用
source ~/.bashrc
# 更新 pip
pip install --upgrade pip
# 安裝 jupyter notebook
conda install jupyter -y
# 修改設定檔，允許遠端登入
# 產生 Jupyter notebook 設定檔
jupyter notebook --generate-config
# 2019.02.12 修正
# 允許本機以外的 IP 連入
sed -i '204s/#//' ~/.jupyter/jupyter_notebook_config.py
sed -i '204s/localhost/0.0.0.0/' ~/.jupyter/jupyter_notebook_config.py
# 不直接開啟瀏覽器
sed -i '267s/#//' ~/.jupyter/jupyter_notebook_config.py
sed -i '267s/True/False/' ~/.jupyter/jupyter_notebook_config.py
# 不使用密碼，可直接使用
sed -i '340s/#//' ~/.jupyter/jupyter_notebook_config.py
sed -i '340s/<generated>//' ~/.jupyter/jupyter_notebook_config.py
```

# 安裝 GForth

```
sudo apt install -y gforth
```

# 安裝 Jupyter notebook 各種程式語言支援核心

```

```
