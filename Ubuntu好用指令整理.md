# 檢視使用者所屬群組

* [查看user屬於那一個group ](https://go-linux.blogspot.com/2011/11/tips-usergroup.html)

```
sophia@ideapad:~$ groups sophia
sophia : sophia adm dialout cdrom sudo dip plugdev lpadmin sambashare
```
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
