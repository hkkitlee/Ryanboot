# Howto/說明:

## First: Download
## 1: 下載：
[主機下載](https://hkkitlee.ddns.net:9000/ryan.zip) [備用下載](http://hkkitlee.ddns.net:8999/ryan.zip) [Github](https://github.com/hkkitlee/Ryanboot/raw/main/ryan.zip) [Gitee](https://gitee.com/hkkitlee/Ryanboot/raw/main/ryan.zip)
Download and unzip ryan.zip,3 files: ryantc.usb, ryantc.iso, version.
請下載ryan.zip,並解壓縮得到3個檔案：ryantc.usb, ryantc.iso, version.

### ryantc.usb:

rufus-3.5p.exe [Github](https://github.com/hkkitlee/Ryanboot/raw/main/rufus-3.5p.exe) [Gitee](https://gitee.com/hkkitlee/Ryanboot/raw/main/rufus-3.5p.exe)

Use Rufus to write the ryantc.usb into your usb disk.
現在我們把影像檔ryantc.usb用Rufus寫進usb盤去.

Caution!!!Please backup your usb key before write, otherwise all data will lost!!!
溫馨提示!!!請先備份好USB內的內容, 否則是會導至USB內的文件隕失!!!

<img src="https://rufus.ie/pics/rufus_en.png" />

Linux: dd if=ryantc.usb of=/dev/[s,v]da


### ryantc.iso:

虛擬機中掛載或燒錄到實體光碟即可。


-------------------------------------------


## Second:Config Computer boot priority
## 2:電腦啟動裝置設定

Switch on the computer and press "DEL" key on your keyboard continuously.
開啟電腦電源並不停按下鍵盤上的 "DEL"鍵

Now, please set your computer first boot priority from usb disk/cd-rom,not your harddisk.
請設定電腦第一啟動設備為U盤/光碟機,而不是硬盤

<img src="https://www.pcbuyerbeware.co.uk/wp-content/uploads/2014/05/AwardBIOSMenu.gif" />
Photo from : https://www.pcbuyerbeware.co.uk/wp-content/uploads/2014/05/AwardBIOSMenu.gif

<img src="https://www.billionwallet.com/windows10/images5/ueif-bios1.png" />
Photo from : https://www.billionwallet.com/windows10/images5/ueif-bios1.png


-------------------------------------------


## Third: Please plug your ethernet cable (cat 5/6) between your PC and router, ethernet port like below.
## 3: 請插入網線並連接你家的路由/光貓
<img src="https://www.lifewire.com/thmb/ZOZ-5GKiprkKtsDUAq3W0VAjnVI=/768x0/filters:no_upscale():max_bytes(150000):strip_icc()/149238278-56a1ad765f9b58b7d0c19fe8.jpg" />
Photo from : https://www.lifewire.com/thmb/ZOZ-5GKiprkKtsDUAq3W0VAjnVI=/768x0/filters:no_upscale():max_bytes(150000):strip_icc()/149238278-56a1ad765f9b58b7d0c19fe8.jpg
