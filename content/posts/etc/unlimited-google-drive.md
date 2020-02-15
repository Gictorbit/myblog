---
title: "دورزدن محدودیت حجم google drive"
date: 2020-01-26T19:45:41+03:30
description: "ذخیره نامحدود فایل ها در حساب گوگل"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocPosition: outer
author: victor
authorEmoji: 😎
tags: 
- گوگل درایو
- python
categories:
- کاربردی
series:

image: "images/post-image/2020/unlimited-storage.png"
meta_image: "images/post-image/2020/google-drive.jpg"
---
## مقدمه
گوگل درایو یکی دیگر از سرویس های پر کاربرد گوگل هست که با داشتن حساب گوگل می تونید از اون برای ذخیره فایل های خود در اینترنت استفاده کنید اما به صورت پیشفرض هر حساب فقط 15 گیگابایت فضای ذخیره سازی رایگان داره و برای حجم بیشتر باید هزینه پرداخت کنید اما با یک روش جالب میشه این محدودیت رو دور زد در ادامه این روش رو توضیح میدم
#### UDS (Unlimited Drive Storage)
این روش با استفاده از یک اسکریپت پایتون به نام uds انجام میشه این اسکریپت در واقع فایل های شما رو در قالب داکیومنت یا فایل متنی در سرویس google doc ذخیره میکنه. چیزی که این پست رو جالب میکنه طرز کار این اسکریپت هست که در ادامه میبینیم 
#### امکانات uds
* آپلود فایل ها در گوگل درایو بدون استفاده از 15‍ گیگابایت فضای حساب
* دانلود فایل های آپلود شده از حساب
## منطق 
* سرویس گوگل داکیومنت برای فایل های متنی که شما ذخیره میکنید فضایی از گوگل درایو کم نمی کند
* تجزیه فایل های باینری به فایل های متنی با روش کد گذاری [base 64](https://en.wikipedia.org/wiki/Base64)
* حجم فایل های کد گذاری شده همیشه بیشتر از حجم فایل اصلی هست
* سرویس گوگل داکیومنت میتونه برای هر فایل متنی میلیون ها کاراکتر ذخیره کنه که حدودا معادل 710 کیلو بایت داده کدگذاری شده با `base 64‍` هست
## راه اندازی و احراز هویت
{{< alert theme="warning" >}}
قبل از اجرای دستورات از نصب بودن **git** و **python** در سیستم عامل مطمئن بشید
{{< /alert >}}

خب برای استفاده از این اسکریپت باید این ریپوزیتوری رو با استفاده از دستور زیر از گیت هاب clone کنیم.

```bash
git clone https://github.com/stewartmcgown/uds.git
```

بعد از اون باید کتابخانه های مورد نیاز رو به کمک فایل `requirements.txt` موجود در فولدر برنامه نصب کنیم

```bash
pip3 install -r requirements.txt
```
#### فعال کردن api حساب گوگل 
برای اینکه برنامه بتونه به گوگل درایو ما دسترسی داشته باشه باید api حساب ما رو داشته باشه برای این کار باید [ Google's API page](https://developers.google.com/drive/api/v3/quickstart/python) رو باز کنیم
و روی `Enable the Drive api` کلیک کنید
بعد از اون ، باید فایل کانفیگ رو که با فرمت `json` هست دانلود کنیم و در فولدر برنامه با نام `client_secret.json` ذخیره کنیم
{{< alert theme="warning" >}}
چون ما در **ایران** هستیم و سرویس های گوگل تحریم هستن باید از **vpn‍** استفاده کنیم
{{< /alert >}}

#### راه اندازی اولیه
حالا با یکی از دستورات زیر باید برنامه رو برای راه اندازی اولیه اجرا کنیم
{{< codes bash bash >}}
  {{< code >}}
  ```bash
  ./uds.py 
  ```
  {{< /code >}}
  {{< code >}}

  ```bash
  python3 uds.py
  ```
  {{< /code >}}

{{< /codes >}}

## دستورات UDS
برای کار با برنامه و ذخیره و دانلود فایل ها باید از آرگومان ها استفاده کنیم 
### آپلود فایل 
برای آپلود فایل باید از دستور `push--` استفاده کنیم برای مثال:
```bash
./uds.py --push Ubuntu.Desktop.16.04.iso
```
```result
Ubuntu.Desktop.16.04.iso will required 543 Docs to store.
Created parent folder with ID 1fc6JGpX6vUWiwflL1jBxM1YpuMHFAms8
Successfully Uploaded Ubuntu.Desktop.16.04.iso: [██████████████████████████████] 100%
```
### لیست فایل ها
برای دیدن لیست فایل های آپلود شده و موجود در درایو باید از دستور `list--` استفاده کنیم برای مثال:
```bash
./uds.py --list
```
```result
Name                      Size   Encoded    ID
------------------------  -----  ---------  ---------------------------------  
Ubuntu.Desktop.16.04.iso  810 MB  1.1 GB    1fc6JGpX6vUWiwflL1jBxM1YpuMHFAms8
Ubuntu.Desktop.18.10.iso  1.1 GB  1.3 GB    1RzzVfN9goHMTkM1Hf1FUWUVS_2R3GK7D
```
و برای جستجو در نام فایل ها باید کلمه مورد نظر رو به دستور لیست پاس بدیم:
```bash
./uds.py --list "18"
```
```result
Name                      Size   Encoded    ID
------------------------  -----  ---------  ---------------------------------  
Ubuntu.Desktop.18.10.iso  1.1 GB  1.3 GB    1RzzVfN9goHMTkM1Hf1FUWUVS_2R3GK7D
```
### دانلود فایل
برای دانلود فایل باید از دستور `pull--` استفاده کنیم برای مثال:
```bash
./uds.py --pull 1fc6JGpX6vUWiwflL1jBxM1YpuMHFAms8
```
```result
Downloaded Ubuntu.Desktop.16.04.iso: [██████████████████████████████] 100%
```

به طور پیشفرض فایل های دانلود شده در فولدر برنامه و دایرکتوری `downloads` ذخیره میشن

### حذف فایل
برای حذف فایل باید از دستور `delete--` استفاده کنیم برای مثال:
```bash
./uds.py --delete 1fc6JGpX6vUWiwflL1jBxM1YpuMHFAms8
```
```result
Deleted 1fc6JGpX6vUWiwflL1jBxM1YpuMHFAms8
```

{{< box >}}
منبع:
<a href="https://github.com/stewartmcgown/uds">UDS : Unlimited Drive Storage </a>
{{< /box >}}
