<font style="font-size:26px">




## <center> LPIC2 JADI ( Linux Professional Institute Certification ) </center>
## <center> **EXAM 201** </center>
</font>

<font style="font-size:20px">

# Table of Contents

1. [EPISODE 10](#EPISODE-10)
   
   1. [compile](#compile)

1. [EPISODE 11](#EPISODE-11)

   1. [system monitoring](#system-monitoring)

1. [EPISODE 12](#EPISODE-12)      

   1. [kernel](#kernel)

1. [EPISODE 13](#EPISODE-13)

   1. [kernel compile E1](#kernel-compile-E1)

1. [EPISODE 14](#EPISODE-14)

   1. [kernel compile E2](#kernel-compile-E2)

1. [EPISODE 15](#EPISODE-15) 

    1. [kernel maintain](#kernel-maintain)

2. [EPISODE 16](#EPISODE-16) 

    1. [filesystem](#filesystem)

3. [EPISODE 17](#EPISODE-17) 

    1. [mounting filesystem](#mounting-filesystem)

4. [EPISODE 18](#EPISODE-18) 

    1. [btrfs](#btrfs)

5. [EPISODE 19](#EPISODE-19) 
   1. [other filesystem](#other-filesystem)

5. [EPISODE 20](#EPISODE-20) 
   1. [check and tuning file system](#check-and-tuning-file-system)

1. [COMMAND LIST](#COMMAND-LIST)

</font>

---
<font style="font-size:24px">

<div id="EPISODE-10"/>

## <font color=red>EPISODE 10 : </font> 
---
<div id="compile"/>

### <center> compile </center>
---
برای کامپایل کردن یک برنامه از سورس از دستورات زیر به ترتیب استفاده می کنیم  

 این دستور چک می کند آیا همه ی پکیج ها در سیستم وجود دارد یا نه  
 
> ./configure  
 
 این دستور کامپایل کردن را شروع می کند  
 
> make  
 
 فایل های اجرایی را کپی می کند جایی که نیاز باشد
 
> sudo make install  

 
---
<div id="EPISODE-11"/>

## <font color=red>EPISODE 11 : </font> 
---
<div id="system-monitoring"/>

### <center> system monitoring </center>
---
دستورات لازم برای هر سخت افزار هم به صورت جدول هم به صورت عادی نوشته می شود:


|   MEMORY   |    CPU   |   I/O  |   NETWORK     |  process  |
| :--------: | :------: | :----: | :-----------: | :-------: |
|   htop     |  htop    |  iotop | iftop         |htop
|   top      |  top     |  lsof  |iptraf/iptraf-ng|top
|   sar      |  sar     |    sar | netstat       |ps 
|   free     |  iostat  |  iostat|     tcpdump   |pmap 
|   vmstat   |  mpstat  |        | ifconfig      | pstree
|            |       w  |        | ip            |w
|            |  iostat  |        | ntop
|            |          |        | ss
|            |          |        | lsof
|            |          |        |


> htop  
> top  
> free  
> sar  
> vmstat  
> ps  
> ps -ef  
> mpstat  
> iostat  
> w  
> iftop -i \[wlpname کارت شبکه]  
> -i= show interface کارت شبکه  
> iptraf  
> netstat  
> tcpdump  
> ip  
> ifconfig  
> ss  
> lsof  
> pmap  
> pstree  
> pmap  

سرویس هایی برای مانیتور سیستم وجود دارند که محیط وب هم به همراه دارند که نام چند مورد را مشاهده می کنید:

<font color=gree>

1. Cacti
2. MRTG
3. Nagios
4. RRDTool
5. munin
6. collectd

</font>

---
<div id="EPISODE-12"/>

## <font color=red>EPISODE 12 : </font> 
---
<div id="kernel"/>

### <center> kernel </center>
---
 دستوری برای نشان دادن مموری تقسیم شده 
 
> ipcs  


در مسیر زیر میتوان لیست کرنل های سیستم را بدست آورد.  

      ls /boot 

انواع اسم های کرنل ها:

<font color=gree>

1. vmlinuz
1. vmlinux
2. bzImage
3. kernel
4. zImage

</font>

حرف z  در کلمه ی  vmlinuz  به معنای زیپ شدن و فشرده شدن است. vmlinux به معنای نسخه فشرده نشده می باشد.

---
<div id="EPISODE-13"/>

## <font color=red>EPISODE 13 : </font> 
---
<div id="kernel-compile-E1"/>

### <center> kernel compile E1 </center>
---
سورس های مختلف لینوکس در مسیر زیر قرار دارد:  

      /usr/src

برای کامپایل کرنل از دستورات زیر استفاده می کنیم:
 نکته : دستورات زیر بر مبنای باز کردن فایل و انجام دستورات در مسیر دایرکتوری کرنل می باشد پس هیچ گونه آدرسی جلوی دستورات نوشته نشده است .  
  دستور زیر تمام اطلاعات کانفیگ را می پرسد و معمولا از دستور زیر به جای این دستور استفاده می کنیم که یک ضرب فایل کانفیگی بر اساس دیفالت های خود کرنل  
 
> make config  
 
 این دستور فایل کانفیگ بر اساس فایل کرنل لینوکس می سازد  
 
> make defconfif 
 
 بعد ساخت فایل کانفیگ دستور زیر کرنل را کامل کامپایل می کند.  
 
> make 

---
<div id="EPISODE-14"/>

## <font color=red>EPISODE 14 : </font> 
---
<div id="kernel-compile-E2"/>

### <center> kernel compile E2 </center>
---
برای درست کردن ماژول ها و نصب آن ها از دستورات زیر به ترتیب استفاده می کنیم:

 ساخت ماژول های کرنل  
 
> make modules  
 
 نصب ماژول های کرنل
 
> make modules_install

 برای نصب کردن کرنل کامپایل شده از دستور زیر استفاده می کنیم:   
  دستور زیر کرنل را نصب می کند و به صورت خودکار  فایل initrd را می سازد
 
> make install  
 
 بعد از تمام شدن نصب از دستور زیر برای آپدیت گراب استفاده می کنیم و بعد سیستم را بوت می کنیم  
 
> update-grub2

---
<div id="EPISODE-15"/>

## <font color=red>EPISODE 15 : </font> 
---
<div id="kernel-maintain"/>

### <center> kernel maintain </center>
---

فایل های ماژول های کرنل در ادرس زیر می باشد:

      /lib/modules
 تنظیمات فایل های ماژول های کرنل در ادرس زیر می باشد:

      /lib/modules-load.d/

در مسیر زیر هم میتوان فایل های مربوط به ماژول هارا مشاهده کرد:

      /etc/modules-load.d/

دستور زیر تمام ماژول ها را نشان می دهد:

> lsmod

برای دیدن اطلاعات ماژول ها از دستور زیر می توان استفاده کرد :  

> modinfo ""module-name""

 از دستور زیر برای قطع و وصل کردن و حذف کردن ماژول ها می توان استفاده کرداگر از سوویچ استفاده نکنیم این دستور ماژول را فقط لود می کند

> modprobe ""module-name""  
> -r = remove  
> -n = dry run فقط تست می کنه که لود میشه یا نه  
> -v = verbos
> -c = نشان دادن کانفیگ فعلی

دستور زیر تمام pci هارا نشان می دهد:

> lspci

برای نشان دادن تمام دیوایس های usb  از دستور زیر استفاده می کنیم:

> lsusb  

---
<div id="EPISODE-16"/>

## <font color=red>EPISODE 16 : </font> 
---
<div id="filesystem"/>

### <center> filesystem </center>
---

شماره های ۱ تا ۴ برای پارتیشن های پرایمری هستش و از شماره پنج به بعد برای پارتیشن های اکستندد هست.

انواع فایل سیستم ها نیتیو لینوکس و قابلیت های آنان:

<font color=gree>

1. ext2
2. ext3 (journaling)*
3. ext4 (journaling)*
4. reiserfs
5. btrfs (RAID and COW)*

</font>

انواع فایل سیستم ها غیر نیتیو لینوکس و قابلیت های آنان:

<font color=gree>

1. ntfs
2. vfat
3. xfs
4. zfs

</font>

---
<div id="EPISODE-17"/>

## <font color=red>EPISODE 17 : </font> 
---
<div id="mounting-filesystem"/>

### <center> mounting filesystem </center>
---
برای پارتیشن بندی دیسک از دستور زیر استفاده می کنیم:

> fdisk ""/dev/sdb""  

برای فورمت کردن دیسک از دستور زیر استفاده می کنیم:

> mkfs.ext4 ""/dev/sdb""

ادامه دستورات را برای بقیه فرمت ها مشاهده می کنیم:

      mkfs.bfs     mkfs.ext2    mkfs.ext4    mkfs.minix   mkfs.ntfs    
      mkfs.cramfs  mkfs.ext3    mkfs.fat     mkfs.msdos   mkfs.vfat

برای مانت کردن یک پارتیشن از دیسک به یک دایرکتوری از دستور زیر استفاده می کنیم:

> mount ""/dev/sdb1"" "/mnt"  
> -a = هرچیزی که در فایل اف اس تب نوشته شده باشه را مانت می کند  
> -r = read only

برای غیر فعال کردن مانت از دستور زیر استفاده می کنیم:

> umount ""/mnt""

یا می توانیم به جای مسیر دایرکتوری از مسیر دیسک استفاده کنیم:

> umount ""/dev/sdb1""

بعد از مانت کردن دیسک به یک دایرکتوری باید در فایل زیر مراحل را یادداشت کنیم تا در زمان بوت شدن سیستم دیسک به صورت اتوماتیک مانت شود

      /etc/fstab

---
<div id="EPISODE-18"/>

## <font color=red>EPISODE 18 : </font> 
---
<div id="btrfs"/>

### <center> btrfs </center>
---
فیچر های مربوط به فایل سیستم بی تی ار اف اس نگاه می کنیم.

1. COW
2. snapshot
3. subvolume
4. raid

برای ساخت فایل سیستم بی تی ار اف اس از دستور زیر استفاده می کنیم

> mkfs.btrfs ""/dev/sdb""



### <center> raid </center>

یک نوع ساختار دیسک هست که دو هارد با یک دیگر سینک می شوند و یک دیتا بر روی دو هارد نوشته می شود. تعداد دیسک ها می تواند با توجه به شماره رید متفاوت باشد

---
برا دیدن اطلاعات فایل سیستم بی تی ار اف اس می توانیم از دستور زیر استفاده کنیم:

> btrfs filesystem show

### <center> subvolume </center>

ساب والیوم ها دایرکتوری هایی هستند که می توانند در جاهای مختلفی مانت شود.

برای ساخت ساب والیوم از دستور زیر استفاده می کنیم: 

> btrfs subvalume creat ""new_sub""

برای لیست کردن ساب والیوم از دستور زیر استفاده می کنیم

> btrfs subvolume list ""/mnt""

هر ساب والییوم دارای یک ایدی می باشد که با دستور زیر مشخص می شود.

> btrfs subvalume get-default ""/mnt/subvalume-path""

برای مانت کردن هر ساب والیوم در جایی دیگری به صورت جدا می توانیم از دستور زیر استفاده کنیم:

> mount -o subvol=""/mnt/ubvalume-path"" /dev/sdb /tmp/distination

---
### <center> snapshot </center>

برای گرفتن اسنپ شات از ساب والیوم از دستور زیر استفاده می کنیم.

> btrfs subvalume snapshot ""/mnt/subvalume-path"" ""/mnt/new-snapshot-name""

---
<div id="EPISODE-19"/>

## <font color=red>EPISODE 19 : </font> 
---
<div id="other-filesystem"/>

### <center> other filesystem </center>
---
#### <center> optical file system </center>

فایل سیستم های اوپتیکالی از مانت شدن dev/cdrom/ به یک فایل به وجود می آید.  

      mount /dev/cdrom /mnt
---
#### <center> swap file system </center>

پارتیشنی از دیسک که مموری از آن استفاده می کند.

برای مانت کردن آن از مسیر زیر استفاده می کنیم.

      /etc/fstab

برای فعال وغیر فعال کردن سواپ فایل از دستور زیر استفاده می کنیم:

> swapon  
> -a= از روی کانفیگ فایل اف اس تب می خونه  
> swapoff

#### <center> encrypt data in file system</center>

اگر بخواهیم دیتا هایی که بر روی دیسک می نشیند را انکریپت کنیم می توانیم از دستور زیر زیر بعد از مانت کردن دیسک بر روی پارتیشن استفاده کنیم :

> mount -t ext4 ""/dev/sdb1"" ""/mnt""
  
> mount -t ecryptfs ""/mnt"" ""/mnt""

---
<div id="EPISODE-20"/>

## <font color=red>EPISODE 20 : </font> 
---
<div id="check-and-tuning-file-system"/>

### <center> check and tuning file system </center>
---

برای دیدن لیست بلاک ها از دستور زیر استفاده می کنیم.  

> lsblk

دستورات زیر برای چک کردن فایل سیستم های ext استفاده می شود معمولا.

برای چک کردن و دیباگ کردن فایل سیستم ها از دستور زیر استفاده می کنیم

> debugfs /dev/sdb1  

برای تغییر لی بل فایل سیستم های ext2/ext3/ext4 از دستور زیر استفاده می کنیم:  

> e2label ""/dev/sda1"" ""hani-disk""  

از دستور زیر برای دیدن ایدی های بلاک های سیستم استفاده می شود

> blkid

با استفاده از دستور زیر می توانید uuid و label درست کنید .  

> tune2fs ""/dev/sdb""

---

برای تغییر لی بل فایل سیستم های btrfs از دستور زیر استفاده می کنیم:  

دستور زیر ری لوکیت می کند دیوایس ها را.

> btrfs balance
 
دستور زیر می تواند فایل سیستم را از ext به btrfs تغییر دهد و بر عکس.

> btrfs-convert

دستور زیر برای تیون کردن استفاده می شود

> btrfstune

#### <center> چک کردن دیسک ها </center>

برای چک کردن فایل سیستم های روی دیسک از دستور زیر استفاده می کنیم. 

> fsck.extf ""/dev/sdb1""

دستورات زیر برای فایل سیستم های دیگر می باشد

      fsck.btrfs   fsck.cramfs  fsck.ext2    fsck.ext3    
      fsck.ext4    fsck.fat     fsck.minix   fsck.msdos   
      fsck.vfat

دستورات زیر برای چک کردن فایل سیستم بی تی ار اف اس می باشد.

> btrfs check

<div id="COMMAND-LIST"/>

# COMMAND LIST

> ./configure    
> make    
> sudo make install    
> htop    
> top    
> free    
> sar    
> vmstat    
> ps    
> ps -ef    
> mpstat    
> iostat    
> w    
> iftop -i \[wlpname کارت شبکه]    
> -i= show interface کارت شبکه    
> iptraf    
> netstat    
> tcpdump    
> ip    
> ifconfig    
> ss    
> lsof    
> pmap    
> pstree    
> pmap    
> ipcs    
> make config    
> make defconfif   
> make   
> make modules    
> make modules_install  
> make install    
> update-grub2  
> lsmod  
> modinfo ""module-name""  
> modprobe ""module-name""    
> -r = remove    
> -n = dry run فقط تست می کنه که لود میشه یا نه    
> -v = verbos  
> -c = نشان دادن کانفیگ فعلی  
> lspci  
> lsusb    
> fdisk ""/dev/sdb""    
> mkfs.ext4 ""/dev/sdb""  
> mount ""/dev/sdb1"" "/mnt"    
> -a = هرچیزی که در فایل اف اس تب نوشته شده باشه را مانت می کند    
> -r = read only  
> umount ""/mnt""  
> umount ""/dev/sdb1""  
> mkfs.btrfs ""/dev/sdb""  
> btrfs filesystem show  
> btrfs subvalume creat ""new_sub""  
> btrfs subvolume list ""/mnt""  
> btrfs subvalume get-default ""/mnt/subvalume-path""  
> mount -o subvol=""/mnt/ubvalume-path"" /dev/sdb /tmp/distination  
> btrfs subvalume snapshot ""/mnt/subvalume-path"" ""/mnt/new-snapshot-name""  
> swapon    
> -a= از روی کانفیگ فایل اف اس تب می خونه    
> swapoff  
> mount -t ext4 ""/dev/sdb1"" ""/mnt""  
> mount -t ecryptfs ""/mnt"" ""/mnt""  
> lsblk  
> debugfs /dev/sdb1    
> e2label ""/dev/sda1"" ""hani-disk""    
> blkid  
> tune2fs ""/dev/sdb""  
> btrfs balance  
> btrfs-convert  
> btrfstune  
> fsck.extf ""/dev/sdb1""  
> btrfs check  

</font>
