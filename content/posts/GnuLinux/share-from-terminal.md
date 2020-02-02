---
title: "اشتراک گذاری فایل ها در ترمینال"
date: 2020-01-31T20:13:37+03:30
description: "اشتراک گذاری فایل ها با transfer.sh"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocPosition: inner
author: victor
authorEmoji: 😎
tags: 
- transfer.sh
- اشتراک گذاری فایل ها
- لینوکس
- کامندلاین
categories:
- کاربردی
- Gnu/Linux
image: "images/post-image/2020/share.png"
meta_image: "images/post-image/2020/transfer.jpg"
---
![share files with transfer.sh](/images/post-image/2020/transfer.jpg)
شاید برای شما هم اتفاق افتاده باشه که وسط کار کردن در ترمینال خودتون به فایلی برخورد کنید و بخواهید اون رو با دیگران به اشتراک بذارید یک روش مرسوم این هست که مرورگر رو باز کنید و با استفاده از یک سایت اشتراک گذاری فایل یا سرویس های معروف مثل دراپ باکس یا گوگل درایو و... فایل خودتون رو آپلود کنید یا در کم دردسر ترین حالت با شبکه های اجتماعی اون فایل رو به اشتراک بگذارید اما یک روش ساده تر و سریع تر هم وجود داره که در ادامه اون رو بررسی میکنیم
## transfer.sh
این راه حل سریع و ساده برای اشتراک گذاری فایل ها از کامند لاین یا ترمینال استفاده از  [transfer.sh](https://transfer.sh) هست فقط کافیه فایلی که میخواهید رو با دستورات زیر آپلود کنید و در نتیجه لینک اشتراک گذاری فایل به عنوان خروجی چاپ میشه
### قابلیت ها
* دسترسی از خط فرمان
* رایگان بودن
* آپلود فایل تا حجم 10GB 
* ذخیره فایل ها تا 14 روز
* رمزنگاری فایل ها

### آپلود فایل با curl
یک ابزار معروف برای انتقال دیتا بین سرور و کلاینت با پروتکل های مختلف `curl` هست میتونیم برای آپلود فایل ازش استفاده کنیم
{{< alert theme="warning" >}}
قبل از هرچیز از نصب بودن **curl** در سیستم عامل خود مطمئن باشید
{{< /alert >}} 
```bash
> curl --upload-file ./hello.txt https://transfer.sh/hello.txt
```
```result
https://transfer.sh/66nb8/hello.txt
```
با اجرای دستور بالا در صورت موفقیت آمیز بودن لینک دانلود به عنوان خروجی دستور نمایش داده میشه

* پارامتر `upload-file--` برای آپلود فایل در curl استفاده میشه
* آرگومان `hello.txt/.` در واقع مسیر فایلی هست که قصد اشتراک گذاری اون رو داریم و باید حتما وجود داشته باشه
* آرگومان سوم لینک سروری هست که میخواهیم فایل رو داخلش آپلود کنیم که در مثال ما `https://transfer.sh` هست اما باید در ادامه اسم فایل رو که `hello.txt` هست اضافه کنیم
### تعریف alias
اما همینطور که واضحه تایپ کردن این هم خودش میتونه وقت گیر باشه و شاید موقع آپلود دستور دقیق برای آپلود فایل یادمون نیاد برای همین میتونیم برای این دستور یک `Alias` تعریف کنیم که کار مارو خیلی آسان خواهد کرد برای این کار تکه کد زیر رو کپی کنید و در انتهای فایل `zshrc.` یا `bashrc.` اضافه کنید
{{< alert theme="warning" >}}
این دو فایل در شاخه **/home/$USER** شما هستند و بسته به اینکه zsh دارید یا bash باید یکی از این فایل ها رو ویرایش کنید
{{< /alert >}} 

```
transfer() { if [ $# -eq 0 ]; then echo -e "No arguments specified. Usage:\necho transfer /tmp/test.md\ncat /tmp/test.md | transfer test.md"; return 1; fi
tmpfile=$( mktemp -t transferXXX ); if tty -s; then basefile=$(basename "$1" | sed -e 's/[^a-zA-Z0-9._-]/-/g'); curl --progress-bar --upload-file "$1" "https://transfer.sh/$basefile" >> $tmpfile; else curl --progress-bar --upload-file "-" "https://transfer.sh/$1" >> $tmpfile ; fi; cat $tmpfile; rm -f $tmpfile; }
```
بعد از اضافه کردن قطعه کد بالا می تونید با دستور `transfer` فایل های خودتون رو به سادگی آپلود کنید برای مثال:
```bash
transfer hello.txt
```
### دانلود فایل ها
برای دانلود فایل در ترمینال هم می تونید با دستور زیر از `curl` استفاده کنید :
```bash
curl https://transfer.sh/66nb8/hello.txt -o hello.txt
```
* آرگومان اول لینک اشتراک گذاری فایل آپلود شده هست
* پارامتر `o-`مختصر output است و باید اسم فایل خروجی رو برای دانلود و ذخیره به اون پاس بدیم که در مثال ما `hello.txt` هست

{{< box >}}
برای دیدن مثال ها و اطلاعات بیشتر به وبسایت <a href="https://transfer.sh">transfer.sh</a> مراجعه کنید
{{< /box >}}

