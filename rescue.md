# 整個過程沒有掛載"受傷"的硬盤,同時此Linux是網絡啟動運行,所以不存在進一步的對硬盤「誤操作」
Linux / testdisk / ddrescue都是沒有版權問題的開源軟件。
http://www.gnu.org/software/ddrescue/ddrescue.html
http://www.cgsecurity.org/wiki/PhotoRec

## DDrescue支援介面:
* ATA
* SATA
* SCSI
* old MFM drives
* floppy discs
* SD卡

## Photorec檔案系統支援:
* FAT
* NTFS
* exFAT
* ext2/ext3/ext4 filesystem
* HFS+

建議備份，再備份。檔案救援是件很消耗時間和人品的事情。別無他選、萬不得已才執行吧。
當然，此方法比起硬盤要進*無塵實驗室*或更換母板來得便宜。但要有心理準備的是，可能需要幾天時間不關電腦下才完成「掃描」，救不救得回還要看你的「人品」，所以備份吧！

## ddrescue
"硬盤分區block device"級別的救援：會「強行」讀取即使有壞軌的磁盤（前題是Bios能正確偵測到硬盤），並儲存成影像檔。供檔案救援軟件PhotoRec用。


* 1.準備額外的空間(硬盤),作為儲存恢復的檔案
先用ssh/samba掛載你們自己的遠端硬盤
或是掛載外置硬盤


* 2.打開terminal,輸入lsblk查看客戶機硬盤掛載點.
出於示範使用虛礙機,所以掛載點是/dev/vda1
真實機用sata/scsi多是/dev/sd{a-z}
ide就是/dev/hd{a-z}我想應該不多人用"博物館級別的電腦"吧


* 3.ddrescue /dev/vda1 /儲存掛載點/vda1.ddrimg [中間空格]





## testdisk-photorec
"檔案file"級別的救援


* 1.準備額外的空間(硬盤),作為儲存恢復的檔案
先用ssh/samba掛載你們自己的遠端硬盤
或是掛載外置硬盤


* 2.打開terminal,輸入lsblk查看客戶機硬盤掛載點.
出於示範使用虛礙機,所以掛載點是/dev/vda1
真實機用sata/scsi多是/dev/sd{a-z}
ide就是/dev/hd{a-z}我想應該不多人用"博物館級別的電腦"吧


* 3.在terminal中再輸入photorec /dev/vda1[中間空格]
選對「檔案系統」xfs,ext,fat,ntfs等，就可開始掃描硬盤


* 4.選擇將"掃描已刪的檔案"重新儲存在1所準備好的空間


* 5.查看儲存的是否你想要的檔案(檔名隋機,不是原來檔名),如否重覆4
