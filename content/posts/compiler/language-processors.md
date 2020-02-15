---
title: "Language Processors پردازنده های زبان"
date: 2020-02-15T16:51:15+03:30
draft: false
description: "اصول طراحی کامپایلر فصل اول: بررسی پردازنده های زبان (Language Processors)"
hideToc: false
enableToc: true
enableTocContent: false
tocPosition: inner
author: victor
authorEmoji: 😎
tags: 
- طراحی کامپایلر
- کامپایلر
- پردازنده های زبان
- Language Processors
series:
- اصول طراحی کامپایلر
categories:
- طراحی کامپایلر فصل1

image: "images/post-image/compiler/compiler.png"
meta_image: "images/post-image/compiler/LanguageProcessors.jpg"
---
## کامپایلر (compiler)
به زبان ساده کامپایلر برنامه ای هست که می تونه یک برنامه که به یک زبان نوشته شده رو بخونه (زبان مبدا یا source language) و به یک برنامه معادل به زبان دیگه (زبان هدف یا target language) ترجمه کنه
به شکل زیر دقت کنید یک نقش مهم کامپایلر این هست که هر اروری که در زبان مبدا، طی فرایند ترجمه شناسایی می شود را گزارش کند

{{< img src="/images/post-image/compiler/languageprocessors1.png" width="300px" height="300px" >}}
اگر برنامه مقصد (target program) یک برنامه زبان ماشین (machine-language) با قابلیت اجرایی (executable) باشه در این صورت می تواند توسط کاربر برای پردازش ورودی ها و تولید خروجی مورد نظر فراخوانی بشه به دیاگرام زیر که اجرای یک target program را نشان می دهد توجه کنید
{{< img src="/images/post-image/compiler/languageprocessors2.png" width="500px" height="500px" >}}

## مفسر (interpreter)
یکی دیگه از انواع رایج پردازنده های زبان ( Language Processors ) مفسرها هستند.
مفسر به جای تولید target program به عنوان یک برنامه ترجمه شده، بر اساس source program و ورودی های کاربر(input) مستقیما عملیات های مشخص شده رو اجرا می کنه شکل زیر یک مفسر را نشان می دهد
{{< img src="/images/post-image/compiler/languageprocessors3.png" width="500px" height="500px" >}}

## معایب و مزایای compiler و interpreter
معمولا کد زبان ماشینی که از برنامه هدف (target program) توسط کامپایلر تولید شده، سریع تر از مفسری هست که ورودی ها رو به خروجی نگاشت میکنه در حالی که یک مفسر در تشخیص دادن ارور ها بهتر عمل میکنه چون source program رو خط به خط و دستور به دستور اجرا می کنه

## کامپایلر های هیبریدی (hybrid compiler) 
برای مثال زبان جاوا با ادغام دو پردازنده زبان مفسری و کامپایلری از یک ماشین مجازی بهره می برد یک برنامه مبدا (source program) جاوا ممکن است اول به یک زبان میانی به نام بایت کد (byte code) کامپایل شود سپس توسط ماشین مجازی (virtual machine) که همان مفسر جاوا هست خط به خط تفسیر می شود
فایده این روش این هست که بایت کد هایی که در یک ماشین کامپایل می شوند می توانند در یک ماشین دیگر تفسیر شوند یا شاید در سرار یک شبکه
البته برای دستیابی به پردازش سریع تر ورودی ها به خروجی، بعضی از انواع کامپایلر های جاوا به نام just-in-time compilers هستند که قبل از این که برنامه های میانی برای پردازش ورودی ها اجرا بشوند بایت کد را بلافاصله به زبان ماشین (صفر و یک) ترجمه می کنند 
{{< img src="/images/post-image/compiler/languageprocessors4.png" width="500px" height="500px" >}}
## پیش پردازشگر preprocessor
برای تولید یک target program علاوه بر کامپایلر ممکن است چندین برنامه دیگر نیاز باشد همچنین  یک source program ممکن است به ماژول هایی تقسیم شود که در فایل های جداگانه ذخیره شده اند. گاهی اوقات یک وظیفه (Task) از گردآوری کردن برنامه مبدا (source program) به برنامه مستقل دیگری واگزار می شود به این برنامه پیش پردازش گر یا preprocessor میگویند 
پیش پردازشگر همچنین ممکن است فایل های header یا macro ها را پردازش و در دستورالعمل های برنامه مبدا جایگزاری کند مثلا در تکه کد زیر از ماکرو `define#` استفاده شده که پیش پردازنده زبان c مورد پردازش قرار میدهد
```c
#include <stdio.h>
#define PI 3.1415
#define circleArea(r) (PI*r*r)
int main() {
    float radius, area;
    printf("Enter the radius: ");
    scanf("%f", &radius);
    area = circleArea(radius);
    printf("Area = %.2f", area);
    return 0;
}
```
## اسمبلر assembler
بعد از اون source program اصلاح شده (modified) به کامپایلر داده سپرده میشه کامپایلر ممکن است یک برنامه با زبان اسمبلی `assembly` به عنوان خروجی تولید کند چون برای کامپایلر تولید کردن کد اسمبلی به عنوان خروجی و دیباگ کردن آن آسان تر است و بعد از آن زبان اسمبلی توسط برنامه ای به نام اسمبلر `assembler` پردازش می شود. اسمبلر برنامه ای است که کد اسمبلی را به کد زبان ماشین یا همان صفر ویک با قابلیت جابجایی یا relocatable (کدی که میتواند در هر کجای حافظه بارگزاری شود) تبدیل میکند
## linker
برنامه های بزرگ اغلب در تکه های کوچکتر کامپایل می شوند بنابراین کد ماشین relocatable باید با object فایل های relocatable دیگر و فایل های کتابخانه ای link شود تا بتواند در ماشین اجرا شود برنامه ای که این کار را انجام می دهد لینکر `linker` نام دارد.
لینکر آدرس های حافظه خارجی را resolve می کند جایی که کد موجود در یک فایل ممکن است به مکانی در یک فایل دیگر ارجاع داشته باشد. سپس بارگزار یا `loader` همه ی object فایل های اجرایی (executable) را در حافظه اصلی  (Ram) برای اجرا شدن بارگزاری یا load می کند. تصوری زیر مراحلی که توضیح دادیم رو نشون میدهد

{{< img src="/images/post-image/compiler/languageprocessors5.png" width="500px" height="500px" >}}

