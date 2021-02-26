# Ryan TinyCore PXE server

## A基礎教學

基於
* 1本帖是對多個發行版進行設定；
* 2圖形視窗有多個版本（Gnome,KDE等等），也不是必須的；而且大部分伺服器取消圖形視窗為搾取性能；所以會是以「命令行」的方式進行；當然你也可以在圖形介面下開一個terminal做設定。

出於個人感想：
多年前，由於機器壓力過大，工作不勝負荷。被迫從M$ 的Windows轉往Linux當中，轉到Linux就是經常聽說可以把性能搾出油來；還有就是Raid了。沒錢買Raid卡的怎辦！Linux用軟件就可以做Raid了！卡也不用買（不要問我什麼是硬盤沉餘陣列......）。心想兩個本來就是操作系統，分別為何如此巨大！
因此會多套用「這個就類似M$的XXX」之類來具體化表達。

現在的Win10怪事頻生，可以越更新越出問題，無語了...反正學懂Linux是自己的。

全文簡稱(大寫英文)：
| TC | TinyCore |
|---|---|
| RH | RedHat 系列包括Centos / Fedora |
| DB | Debian 系列包括Ubuntu |
| M$ | Windows |

關於安裝Linux
* TC只需下載影像檔並選擇安裝在U盤即可。
* RH/DB安裝以minimal install即可。詳細硬盤劃分之類怕與本論壇目的相違背，所以請自行參考、虛擬機、或用TC u盤。




## B安裝軟件並設定（俗稱軟件包管理器）
現在的M$ Win10 當中，有個叫［Microsoft Store]的軟件，其作用就類似我們智能電話中Android的Google Play或IOS Iphone中的App Store（國行版閹割了的）。事實上Linux很早期已有此功能實現。

各發行版因應自身設計，發展出不同的［線上管理器］：
| Linux	| 尋找 | 查詢詳情 | 安裝 | 卸載移除 |
| --- | --- | --- | --- | --- |
| RH | yum search XXX | yum info YYY | yum install YYY | yum remove YYY |
| DB | apt search XXX | apt info YYY | apt install YYY | apt remove YYY |
| TC | tce-ab (Enter後選"S"earch填入) | tce-ab ('S'earch完選取即可見) | tce-load -wi YYY | tce-remove YYY |


* XXX為你查詢關鍵字,查詢後得到軟件包的全名 
* YYY就是包全名稱用以安裝/移除 

這次我們需要dnsmasq(因為可連tftp一起啟動),所以:
| Linux | command |
| --- | --- |
| RH | sudo yum install dnsmasq -y |
| DB | sudo apt install dnsmasq -y |
| TC | sudo tce-load -wi dnsmasq |

***-y 為預先確認安裝***


在M$我們編輯文檔，沒有Office也的要個notepad；看看下面Linux中的一個cat命令已建立完畢。
其實還有其他方法，不過是因為後續3樓「懶人大法」的關係吧了。
現在編輯dnsmasq的設定檔，其位置在/etc/dnsmasq.conf：

RH/DB/TC:
```  sudo cat << EOF > /etc/dnsmasq.conf:
#Don't function as a DNS server:
port=0 #不以DNS server作啟動

#Log lots of extra information about DHCP transactions.
log-dhcp #以log在伺服器作記錄

enable-tftp #這裡dnsmasq兼任tftp。
tftp-root=/var/lib/tftpboot #tftp的設定路徑

#Disable re-use of the DHCP servername and filename fields as extra
#option space. That's to avoid confusing some old or broken DHCP clients.
dhcp-no-override #簡單而言，就是避免與太老舊的網卡相撞。

pxe-service=X86PC, "Install Legacy BIOS", bios/core/pxelinux.0
pxe-service=IA32_EFI, "Boot BC_EFI", efi64/efi/syslinux.efi
pxe-service=BC_EFI, "Boot BC_EFI", efi64/efi/syslinux.efi
pxe-service=X86-64_EFI, "Boot X86-64", efi64/efi/syslinux.efi

dhcp-range=192.168.2.5,proxy#*******這個就是設定為dhcp-proxy啟動，留意ip值是否適用,並沒有設子掩碼以盡量回應。
EOF
``` 

上面dnsmasq.conf中，在/var/lib/tftpboot下創建兩個新的資料夾，分別名為bios及uefi；用來儲存不同架構用的菜單。


sudo mkdir -p /var/lib/tftpboot/syslinux


## C防火牆/selinux/apparmor

20190729:
| command | 說明 |
| --- | --- |
| RH : |
| sudo systemctl stop firewalld | #關閉系統防火牆 |
| sudo systemctl start firewalld | #啟動系統防火牆 |
| sudo firewall-cmd —add-port=4011/udp | #臨時防火牆開放連線：udp的端口4011 |
| sudo firewall-cmd —add-port=4011/udp -permanent | #永久防火牆開放連線：udp的端口4011 |
| --- | 其他port自己照著辦吧！ |
| sudo firewall-cmd —remove-port=4011/udp | #取消喇 |
| sudo firewall-cmd —remove-port=4011/udp —permanent | #永久的取消 |
|   |
| DB: |
| sudo ufw disable | #關閉防火牆 |
| sudo ufw enable | #啟動防火牆 |
| sudo ufw default deny incoming | #設定防火牆預設禁止外來連線 |
| sudo ufw allow 4011/udp | #防火牆開放連線：udp的端口4011 |
| --- | 其他port自己照著辦吧！ |
| sudo ufw deny 4011/udp | #取消開放連線 |
| | |
| TC: | 沒有防火牆 |

* RH的selinux:sudo setenforce 0
* DB的apparmor:sudo systemctl stop apparmor


***暫時關閉防火牆,之後再追加啟動開放連線***




## D啟動dnsmasq（daemon mode)
平常我們在windows啟動軟件,就時雙按圖標，出現程式介面或出現小圖示在時間旁邊。以Tiny Pxe server作例：
  
相信大家也不陌生了。但有否留意HTTPd/DNSd最後為何有個d字？
沒錯，這就是我上面的標題—daemon mode。意思是類似背境長駐程式。

在Linux中deamon是—「程式/命令（在背境）執行/啟動伺服的方式」。
在M$中就類似在時間邊的最小化圖示。

還是實際點不扯太遠，看看如何啟動dnsmasq作一個pxe伺服「環境」。
因應預設定檔是/etc/dnsmasq.conf，所以會馬上根據預設定檔來啟動。
```  RH/DB: sudo systemctl start dnsmasq
TC: sudo dnsmasq -9 -d ``` 

PXE環境「硬件」至此已經完成，只欠給客戶機的「軟件」，就是E和F了




## E.  Syslinux
原包下載：https://mirrors.edge.kernel.org/ ... x/syslinux-6.03.zip

簡介：Syslinux 提供現成的［pxelinux.0給bios］［syslinux.efi給uefi]的網啟軟件，我們只需要在下載，解壓縮至dnsmasq指定的/var/lib/tftpboot/內即可。

```  RH/DB/TC: 
sudo wget https://mirrors.edge.kernel.org/ ... x/syslinux-6.03.zip -O /var/lib/tftpboot/syslinux-6.03.zip #wget=下載至/var/lib/tftpboot/
sudo unzip /var/lib/tftpboot/syslinux-6.03.zip -d /var/lib/tftpboot/ #把syslinux-6.03.zip解壓縮至/var/lib/tftpboot/
``` 


放好軟件syslinux了，是時候告訴syslinux的要做什麼工作，就是靠［菜單］；告知要chainload 我們的主角ipxe了。

#BIOS版菜單

```  sudo cat << EOF > /var/lib/tftpboot/bios/core/pxelinux.cfg
DEFAULT menu.c32
LABEL bios
KERNEL undionly.kpxe dhcp && chain http://10.10.10.10/NFW.ipxe
EOF ``` 


#UEFI版菜單

```  sudo cat << EOF > /var/lib/tftpboot/efi64/efi/syslinux.cfg
DEFAULT menu.c32
LABEL uefi
KERNEL ipxe.efi dhcp && chain http://10.10.10.10/NFW.ipxe
EOF ``` 


「軟件」的一半—Syslinux 已完成了




## F. IPXE
下載bios版ipxe:http://boot.ipxe.org/undionly.kpxe
下載uefi版ipxe:http://boot.ipxe.org/ipxe.efi

又是如法炮製：
RH/DB/TC:
```  sudo wget http://boot.ipxe.org/undionly.kpxe -O /var/lib/tftpboot/bios/core/undionly.kpxe #下載bios版ipxe至/var/lib/tftpboot/bios/core/
sudo wget http://boot.ipxe.org/ipxe.efi -O /var/lib/tftpboot/efi64/efi/ipxe.efi #下載uefi版ipxe至/var/lib/tftpboot/efi64/efi/
``` 

根據上面syslinux賦予ipxe的腳本是http://10.10.10.10/NFW.ipxe。那往後只需修改網頁伺服器內的這個腳本即可。不需要再行編譯了。
除非網頁伺服的ip或script改名改位置，才需要更改TC內syslinux的設定檔了。

「軟件」部份已全部完成。是時候在IPXE的script中發揮你們天賦了。 
溫馨提示：如需向「公網」對外開放，請做足防禦。大屁孩小屁孩滿天飛！




G*特別*關於TC儲存回u盤
RH/DB正常都屬於安裝在硬盤的操作系統（Live等版本除外），正常關機下是會回寫數據待下次啟動。
但TC因為本身設定是RamOS，內存特點就是沒電消數據，半點不留人。所以我們需要手工多一步來寫回U盤。寫回「設定、需啟動命令」留待下一次開機自動讀取執行。

TC:sudo filetool.sh -b


是否開始發覺現在用的操作系統完來在背後有很多工作？
這個操作系統彈性十足吧？這其實只是九牛一毛...


即將會是「超級懶人自動執行易改包」....

懶人思路：
雖然我們已經可以用命令來做pxe伺服器了。也已經可以回寫儲存在u盤了。
但我的本意是做一個臨時的pxe；既然是臨時的：［dnsmasq網絡參數］、［httpd ip］等等都可能需要因環境情況而改變喇！

M$當中有種命令檔叫「批次檔」*.bat。Linux也有一種用來存放命令的檔案叫「bash file」 *.sh；
我們大可以把所有命令放在bash檔，一次過執行。

我是一個非常懶、貪方便的人。每次需要改變bash就需要坐在「臨時拉夫」的機器面前：看看看，打打打。
生產環境的惡劣，曾經試過要鍵盤壞幾個鍵，要屏幕不是插頭不對就是色偏又或驅動不對［Linux常見事］沒畫面；但網絡沒問題。既然我只需要Pxe服務......

不如我們*只*設定TC啟動時，*wget下載*我們預先設定好的bash檔再*執行*。那我們就不用再坐在「臨時機」前再「操勞一番」。反正我們都有httpd來存放操作系統文件，加多個bash檔好了！

優點：
1可以避免鍵盤屏幕之類問題。
2連Syslinux的菜單都可以遙距改變！「最主要的是httpd伺服ip啊」！
3避免對u盤有不可逆回的改變！即使誤操作，從新啟動又一條好漢啦！
4更可以多寫幾個不同功能的bash檔，變成不同的多功能伺服器！


綜合以上思路，我們以後只需要更改bash檔及ipxe的script就可以完整控制整個臨時的TC伺服器了！！！

現在首先需要知道如何令TC自動執行第一個bash，翻查官網文件原來位置是 /opt/bootlocal.sh，當系統核心完成啟動並加載好其他模組後即以root身份執行，太好了。

那我們只需修改 /opt/bootlocal.sh令其*wget下載*、*執行*我們自已的bash檔就完成了！

在TC開個terminal:
sudo cat << EOF >  /opt/bootlocal.sh
#繼續假設你httpd地址是http://10.10.10.10/並把bash檔命名為TCbash.sh，放到網頁的根目錄可供下載。
sudo wget http://10.10.10.10/TCbash.sh -O /TCbash.sh #下載並儲存在TC的根目錄。
sudo bash /TCbash.sh #執行就是bash檔前加命令bash。
EOF

sudo filetool.sh -b #馬上寫回u盤了

那bash如何寫！！！2樓綠色的全部就是了！
原文:趕緊把需要的複製粘上吧！
20190727:實測下因為bash檔由{系統}並以{root}執行,而TC是禁止root安裝的。所以安裝dnsmasq時以{tc}執行,而啟動則需要(全路徑)。請看最下文的bash分享。

既然已經「線上引導啟動」那2樓的G—sudo filetool.sh -b還有需要加到bash檔嗎？所以就是紅色了。
又來溫馨提示：bash是有「先後順序」的，即不會「未安裝，就執行」的道理。理解不？


我的成品則會「嘗試」再深一點的改造，並放在編譯/設定ipxe usb UEFI Secure boot/Bios 遠程安裝 / 救援 Linux/Winpe10 +ValdikSS中，用來網啟出另一台Pxe伺服出來。
到時大家可以一個u盤，網啟你們網絡的全部電腦了。

移動pxe完成了，哈哈。有興趣的朋友自己動動手吧。





20190728我的改造如下：
TC的啟動腳本改為在/etc/init.d/dhcp.sh，原因如下：
1我是直接修改iso(Live)的corepure64.gz，啟動時不執行/opt/bootlocal.sh
2啟動過快，有時網卡未初始化完成就執行bash，導致所需的工作失敗(如安裝dnsmasq)。



#由於root禁止安裝，所以用root命令用戶名tc來安裝dnsmasq，如下：
/bin/su tc -c '/usr/bin/tce-load -wi dnsmasq'; 


sudo /bin/cat << EOF > /etc/dnsmasq.conf

#Don't function as a DNS server:
port=0

#Log lots of extra information about DHCP transactions.
log-dhcp

enable-tftp
tftp-root=/var/lib/tftpboot

#Disable re-use of the DHCP servername and filename fields as extra
#option space. That's to avoid confusing some old or broken DHCP clients.
dhcp-no-override

pxe-prompt="Press F8 for NBP (Net Boot Program) menu.Default kkpxe.", 10

#0
pxe-service=X86PC, "kkpxe for Legacy BIOS", undionly.kkpxe
pxe-service=X86PC, "kpxe for Legacy BIOS", undionly.kpxe
pxe-service=X86PC, "pxe for Legacy BIOS", undionly.pxe

#2
pxe-service=IA64_EFI, "Boot IA64_EFI", uefi/ipxe64.efi

#6
pxe-service=IA32_EFI, "Boot IA32_EFI", uefi/ipxe32.efi

#7
pxe-service=X86-64_EFI, "Boot X86-64_EFI", uefi/ipxe64.efi

#8
pxe-service=Xscale_EFI, "Boot BC_EFI", uefi/ipxe64.efi

#9
pxe-service=BC_EFI, "Boot BC_EFI", uefi/ipxe64.efi

EOF

ip=$(/sbin/ifconfig |grep -v 127 | grep 'inet ' | sed 's/^.*inet addr://g'    | sed 's/ *Bcast.*$//g') 

echo "dhcp-range=$ip,proxy" >> /etc/dnsmasq.conf


/usr/local/sbin/dnsmasq -9 &
