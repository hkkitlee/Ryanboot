# Ryanboot - 小白「云端系统」
# 拒絕病毒！拒絕流氓！拒絕廣告！

* 主頁/index: https://hkkitlee.ddns.net:9000/
* 下載：https://hkkitlee.ddns.net:9000/ryan.zip
* 備用下載：http://hkkitlee.ddns.net:8999/ryan.zip
* 論壇/forum: http://bbs.wuyou.net/forum.php?mod=viewthread&tid=415206&extra=page%3D1
* 詳細圖文教學使用 https://hkkitlee.ddns.net:9000/setup.html
* Changlog:https://hkkitlee.ddns.net:9000/changelog.txt
* 從你的iPXE伺服器連接至小白雲端啟動系統。
``` dhcp && chain --autofree http://hkkitlee.ddns.net:8999/chain.ipxe ```

本軟件的由來：出於幫朋友重灌，掃毒，恢復文件誤刪之類的操作。往往需要在機器面前才可以操作。因此本人「組合」成這個只有不足40MB的usb影像檔。用來遠程控制解決大部份的操作以節省交通時間。

本分享帖是以本軟件由編譯，組合，制作影像檔；伺服器軟件設定。


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

* 啟動總覽Map: https://github.com/hkkitlee/Ryanboot/blob/main/map.txt

# https://github.com/hkkitlee/P2P-Boot-Linux

## Special thx:
* https://github.com/ValdikSS/Super-UEFIinSecureBoot-Disk
* https://ipxe.org/
