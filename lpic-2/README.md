<font style="font-size:20px">

<h1> Table of Contents </h1>

1. [Introduction](#Introduction)

2. [Linux Boot](#Linux-Boot)

3. [Boot Loaders](#Boot-Loaders)

4. [Init Concept](#Init-Concept)

5. [SysV](#SysV)      

6. [Systemd](#Systemd)

7. [Upstart and System Rescue](#Upstart-and-System-Rescue)

8. [Informing](#Informing)

9. []()

10. []()

11. []()

</font>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>

<font style="font-size:26px">

<h2> <center> LPIC2 JADI ( Linux Professional Institute Certification ) </center> </h2>

</font>

<p>&nbsp;</p>

<font style="font-size:24px">



<font color=red> <h2 id=EPISODE-1> EPISODE 1 : </h2> </font>

----------------------------------------------------------------------------------------------------------

<h3 id=Introduction> <center>Introduction</center> </h3>

course instructor : [JADI](https://jadi.ir/).

source courses : [GO TO CLASS LPIC2](https://gotoclass.ir/courses/lpic-2/).

Source Book is linux professional institute certification : [LPIC2](https://gotoclass.ir/courses/lpic-2/).

<a href="https://www.markdownguide.org" target="_blank">Learn Markdown!</a>


جادی میرمیرانی
جادی میرمیرانی برنامه‌نویس و دانشمند داده‌ای با بیش از بیست سال سابقه برنامه‌نویسی حرفه‌ای است که تجربه زیادی در زمینه تدریس در حوزه آی تی دارد. از مجموعه آموزش های پرطرفدار او میتوان به آموزش لینوکس اشاره کرد که در آن سرفصلهای lpic-1 را پوشش می‌دهد این دوره در وب‌سایت‌های مختلف با عنوان lpic جادی یا لینوکس جادی شناخته می‌شود. اما تدریسهای او به همین مورد محدود نمیشود. جادی علاوه بر این در وبسایت شخصی خود مجموعه‌ای از پادکستها و ویدیو کستها را برای علاقمندان به برنامه نویسی منتشر می‌کند و گاهگاهی دست به ترجمه کتابهای تخصصی می‌زند.

----------------------------------------------------------------------------------------------------------

<font color=red> <h2 id=EPISODE-2> EPISODE 2 : </h2> </font>

----------------------------------------------------------------------------------------------------------

<h3 id=Linux-Boot> <center>Linux Boot</center> </h3>

دستور زیر نشان دهنده ی مراحل بوت کرنل می باشد :

> dmesg

انواع معماری های عمومی بوت :

 <ol> <font color=gree >
 <li> BIOS </li>
 <li> UEFI </li>
 </font>
 </ol>

 در BIOS بعد از عمل POST از یک سکتور هارد انتظار می رود سخت افزار را بوت کند (اولین سکتور از هارد را MBR یا Master Boot Recorde می گویند) بعد به وسیله برنامه کوچکی به نام  Boot Loader کرنل  اجرا می شود .

در UEFI نیاز به بوت لودر برای بوت کرنل نیست و میتوان از هر قسمت از هارد کرنل را اجرا و سیستم را بوت کرد همچنین می توان از بوت لودر هم در این معماری برای بوت سیستم استفاده کرد.  

----------------------------------------------------------------------------------------------------------

<font color=red > <h2 id=EPISODE-3> EPISODE 3 : </h2> </font>

----------------------------------------------------------------------------------------------------------

<h3 id=Boot-Loaders> <center> Boot Loaders </center> </h3>

 انواع  بوت لودر :

<font color=gree >
<ol> 
<li> LILO (Linux Loader)</li>
<li> GRUB (Grand Unifide Boot Loader)</li>
<ol>
<li> GRUB Legacy</li>
<li> GRUB 2</li>
</ol>
</ol>
 </font>

در حال حاضر LILO استفاده نمی شود .  
در بوت لودر های GRUB از UEFI  ساپورت می شود. فایل های کانفیگ گراب دو در مسیر زیر قرار دارد.  

    /boot/grub/grub.cfg

در فایل بالا میتوان تنظیمات مربوط به گراب دو را مشاهده کرد .  
<ins>لازم به ذکر است برای تغییر کانفیگ گراب نباید فایل بالا را ویرایش کنید.</ins>  
از طریق فایل های زیر می توان به صورت جداگانه تغییرات لازم را انجام داد. 

    /boot/grub/grub.cfg
    /etc/grub.d/*

از دستورات زیر می توان برای نصب گراب بر روی قسمت MBR  دیسک انجام داد.

> grub-install  

    grub-install /dev/sda


----------------------------------------------------------------------------------------------------------

<font color=red> <h2 id=EPISODE-4> EPISODE 4 : </h2> </font>

----------------------------------------------------------------------------------------------------------
<center> <h3 id=Init-Concept> Init Concept </mark> </h3> </center>


انواع سیستم ها برای init کردن پرسس ها عبارت اند از :

<font color=gree>
<ol> 
<li>SysV</li>
<li>Systemd</li>
<li>Upstart</li>
</ol>
</font>

انواع Run Level ها : (ران لول ها می تواند شخصی سازی یا برای هر سیستم عامل متفاوت باشد و یا تعدادی از آن ها متفاوت نباشد و شبیه یک دیگر باشند)

<font color=gree>
<ol>
<li> LVL 0 (Shoutdown)</li>
<li> LVL 1 (Single User)</li>
<li> LVL 2 (GUI Multi User)</li>
<li> LVL 3 </li>
<li> LVL 4 </li>
<li> LVL 5 (GUI)</li>
<li> LVL 6 (Reboot)</li>
</ol>
</font>

برای دیدن ران لولی که در آن وجود داریم از دستور زیر استفاده می کنیم :

>runlevel

برای تغییر ران لول از دستور زیر استفاده می کنیم.

>init 5

دستوراتی که از ران لول ها استفاده می کنند :

> reboot  
> shoutdown  

hult mode 
> shoutdown -h 

power off
> shoutdown now 

----------------------------------------------------------------------------------------------------------

<font color=red><h2 id=EPISODE-5> EPISODE 5 :</h2></font>

----------------------------------------------------------------------------------------------------------
<center><h3 id=SysV> SysV </h3></center>

در سیستم های قدیمی که از Sys V استفاده میکنند فایل های کانفیگ مربوطه در ادرس های زیر موجود می باشد.  

    /etc/inittab
    /etc/init.d

در مسیر etc/init.d/ اسکریپت هایی برای stop, start, restart کردن سرویس ها وجود دارد. برای مثال :

    user@PC:~$ ls -l /etc/init.d/
    -rwxr-xr-x 1 root root 2269 Nov 28  2019 acpid
    -rwxr-xr-x 1 root root 5574 Nov  5  2019 alsa-utils
    -rwxr-xr-x 1 root root 2055 Jul 16  2019 anacron
    -rwxr-xr-x 1 root root 3740 Apr  1  2020 apparmor

----------------------------------------------------------------------------------------------------------

<font color=red><h2 id=EPISODE-6> EPISODE 6 :</h2></font>

----------------------------------------------------------------------------------------------------------
<center><h3 id=Systemd> Systemd </h3></center>

سیستم دی با یک مفهوم بزرگ کار می کنه : 
<font color=gree>
<ol>
<li>UNIT</li> 
</ol>
</font>

هر unit از چه چیز هایی تشکیل می شود؟ name + type+config

هر type می تواند شامل : 

<font color=gree>
<ol>
<li>target</li>
<li>service</li>
<li>socket</li>
<li>path</li>
<li>mount</li>
<li>and .....</li>
</ol>
</font>

برای دیدن تمامی Unit ها از دستور زیر استفاده می کنیم :
> systemctl  

یا

> systemctl list-units

تمامی  UNIT ها در مسیر زیر تعریف شده اند :

    /lib/systemd/system

بخشی دیگری از تنظیمات در مسیر زیر قرار دارد : 

    /etc/systemd/system/

دستوراتی که می توان برای تغییر وضعیت unit  ها استفاده کرد :

> systemctl \[options] [unit name] 

نشان دادن وضعیت یونیت
> systemctl start forticlient-scheduler.service

استارت کردن یونیت
> systemctl start forticlient-scheduler.service  

استوپ کردن یونیت
> systemctl stop forticlient-scheduler.service  

قطع و وصل کردن یونیت
> systemctl restart forticlient-scheduler.service  

اگر سرویسی قابلیت reload شدن داشته باشه فایل کانفیگ را از اول می خواند اما اگر این قابلیت را نداشته باشد همانند restart سرویس را قطع و وصل می کند.

> systemctl reload forticlient-scheduler.service  

فعال کردن upstart
> systemctl enable forticlient-scheduler.service

غیر فعال کردن upstart
> systemctl disable forticlient-scheduler.service

قطع کردن تمام یونیت ها و وصل کردن یک یونیت خاص (برای ترابل شوت استفاده مب شود)
> systemctl isolate forticlient-scheduler.service

نشان دادن لاگ های یونیت ها
> journalctl

----------------------------------------------------------------------------------------------------------
<font color=red><h2 id=EPISODE-7>EPISODE 7 :</h2></font>

----------------------------------------------------------------------------------------------------------
<center> <h3 id=Upstart-and-System-Rescue> Upstart and System Rescue</h3> </center>

اپ استارت یک سیستم منسوخ شده همانند SysV می باشد که تنها فرق متمایز و جذاب آن وابسته نبودن به ران لول ها بود.

در زمانی که کرنل لینوکس ما آسیب دیده باشد در هنگام بوت می توانیم در گراب کرنل را تغییر دهیم و مراحل بوت را از  طریق کرنل دیگری بگذرانیم.

----------------------------------------------------------------------------------------------------------

<font color=red><h2 id =EPISODE-8 >EPISODE 8 :</h2></font>

----------------------------------------------------------------------------------------------------------
<center> <h3 id=Informing> Informing </h3> </center>







</font>
