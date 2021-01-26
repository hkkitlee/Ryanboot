# Ryanboot - 小白「云端系统」
# 拒絕病毒！拒絕流氓！拒絕廣告！

* 主頁/index: https://hkkitlee.ddns.net:9000/
* 下載：https://hkkitlee.ddns.net:9000/ryan.zip
* 備用下載：http://hkkitlee.ddns.net:8999/ryan.zip
* 論壇/forum: http://bbs.wuyou.net/forum.php?mod=viewthread&tid=415206&extra=page%3D1
* 詳細圖文教學使用 https://hkkitlee.ddns.net:9000/setup.html
* Changlog:https://hkkitlee.ddns.net:9000/changelog.txt
* 從你的iPXE伺服器連接至小白雲端啟動系統。dhcp && chain --autofree http://hkkitlee.ddns.net:8999/chain.ipxe

# 請注意！請注意！請注意！
##### 本軟件的部分系統(Remote control by server)現時開放權限為自動執行遠程連線！（給我的小白朋友用）不是駭客入侵。機器前也是可以透明操作的
##### 主伺服器不是全日24小時啟動，副服務器提供24/7官方Linux安裝服務。
##### 使用與否隨閣下所喜，由於機主也是透明可操作的，因此使用便是同意並是自願性質，請緊慎操作「水能載舟 亦能覆舟」
##### 本人出於分享精神寫此文件
##### 除非是機主及我同意的有償服務，否則即使連線到伺服器亦不會有任何操作，特此聲明
##### 少寫文章,可能段落格式,表達方法都不太好.請多多見諒


# 軟件簡介:本軟件適用於
##### 1機器類型:實體機器,虛礙機(vmware [esxi/workstation]/ kvm / virtualbox等等)
##### 2BIOS/UEFI [Secureboot] 都可以
##### 3影像檔內並沒有操作系統,所以體積很小(約25MB)和需要伺服器供檔啟動
##### 4因軟件會不定時更新/除錯,請在使用前下載一次
##### 5賬密請仔細閱讀，未知賬號請不要強行進入，否則伺服器會禁止IP連線。另立賬密以供無懮論壇會員作測試，如刪除/更改恕不另行通知。

##### 名稱username：wuyou  密碼passwd：hkkitlee

```
可啟動項目:


Ryanboot ##小白云端系統啟動首頁
|---(A) Boot byYour Network MAC address ##使用網卡唯一識別碼自動啟動
|---(B) Login for Install or live LINUX/WINDOWS ##登入伺服器
|       |---(A) RyanTC mini PXE Server  ##文本終端pxe 伺服器
|       |---(B) Boot Clonezilla for Backup / Restore ##再生龍備份/還原。終端中使用partclone的備份軟件
|       |---(C) Boot REMOTE CONTROL system Menu ##遠程被控項目
|       |       |---(1) P2P BOOT Debian 10 Buster Installer or Live for Data Rescue  ##(P2P啟動RPBL；硬盤壞軌讀取，救援誤刪系統。)
|       |       |---(2) Tinycore v11 Desktop ##桌面、ipxe編譯環境、連 pxe 伺服器
|       |       |---(3) Boot Win10PE_x64 w/VNC  ##(Winpe不用多說了吧)
|       |       |---(4) Install Centos7 via VNC by Kit  ##(安裝Centos Linux)
|       |       |---(5) Install Fedora via VNC by Kit ##(安裝Fedora Linux)
|       |       |---(6) Boot Redo rescue for Backup / Restore ##桌面中使用partclone桌面的備份軟件
|       |       |---(Ctrl-R) Return to First Menu ##返回首頁
|       |
|       |---(D) P2P Boot Official Debian Buster Live Menu ##BT/p2p啟動Debian官方桌面版本項目
|       |       |---(0) Lxqt
|       |       |---(1) Gnome
|       |       |---(2) Kde
|       |       |---(3) Lxde
|       |       |---(4) Mate
|       |       |---(5) Standard
|       |       |---(6) Xfce
|       |       |---(7) Cinnamon
|       |       |---(Ctrl-R) Return to First Menu ##返回首頁
|       |
|       |---Testing Area (Restricted)
|       |---(Ctrl-R) Return to First Menu ##返回首頁
|
|---(C)Linux Installer or netboot.xyz (24/7)
|       |---(A) Install Ubuntu amd64/i386 @
|       |---(B) Install Fedora amd64 @
|       |---(C) Install Debian amd64/i386 @
|       |---(D) Install Parrot-Linux amd64/i386
|       |---(E) Install Kali-Linux amd64/i386
|       |---(F) Install Arch Linux amd64
|       |---(G) Install Arch Linux amd64 by script
|       |---(H) Install Oracle Linux amd64
|       |---netboot.xyz
|---Free Memtest ##記憶體測試
```

# https://github.com/hkkitlee/P2P-Boot-Linux

## Special thx:
* https://github.com/ValdikSS/Super-UEFIinSecureBoot-Disk
* https://ipxe.org/
