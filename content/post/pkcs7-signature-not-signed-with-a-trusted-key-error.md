+++
title = "Pkcs#7 Signature Not Signed With a Trusted Key Error"
description = ""
author = ""
date = 2018-09-08T00:10:02+04:30
tags = []
draft = false
+++


## داستان upgrade Linux Mint 18.3 به نسخه 19 و دردسرهای آن

خب بدون مقدمه میرم سر اصل مطلب. من از توزیع Mint 18.3 استفاده میکردم. و امروز زد به سرم که upgrade کنم به نسخه جدید یعنی ۱۹. و این شد که دردسرها شروع شد.

سیستم من از قبل دارای ویندوز ۱۰ بود و نوع پارتیشن تیبل GPT و از همه مهمتر UEFI فعال بود.

بعد از upgrade کردن که از طریق [این لینک](https://www.tecmint.com/upgrade-to-linux-mint-19/) انجام دادم به مشکل عجیبی برخوردم.

بعد از انجام این کار که البته حجم حدود ۱ گیگ اپدیت داره نسخه جدید، سیستم را ریست کردم و سیستم بالا اومد.

بعد رفتم سراغ update manager و دیدم ۱ گیگ آپدیت نرم افزارها و فایل ها را لیست کرده. من هم زدم نصب کنه آپدیت ها را.

چشمتون روز بد نبینه سیستم را به حال خودش گذاشته بودم چون زیاد بود فایل هایی که باید نصب میشدن. برگشتم دیدم سیستم هنگ کرده. ctrl+f1 هم جواب نمیداد.
نه موس نه کیبورد نه هیچی. و من مجبور شدم سیستم را دستی خاموش روشن کنم. و اینجا بود که بعد از منو بوت صفحه جدیدی ظاهر میشد و این خط خطا توش تکرار شده بود

{{< highlight shell >}}

Pkcs#7 Signature Not Signed With a Trusted Key

{{< / highlight >}}

که تصویرش را میتونید ببینید:

![uefi error](/images/UEFI-Error.jpeg "uefi error")

سرچ هام شروع شد و هرچی گوگل کردیم بی فایده بود خیلی های دیگه با این خطا روبرو شده بودند ولی اکثرا مربوط به درایور nvidia بود. 

گفتم شاید گراب خراب شده و برای همین لینوکس مینت را بوتیبل کردم روی فلش و بصورت live اومدم بالا و مراحل [این لینک](https://howtoubuntu.org/how-to-repair-restore-reinstall-grub-2-with-a-ubuntu-live-cd) را دنبال کردم.


بعد هم ریست کردم دیدم باز هم همونطور میشه.

باز شروع کردم سرچ زدن و خوندن فروم ابونتو و ... که بی فایده بود.

یک مدت بود دوستان زیادی پیشنهاد میدادن آرچ استفاده کنم و برای همین گفتم چه بهونه ای بهتر از این D:
برای همین تصمیم گرفتم که کلا برم سراغ ارچ :) آرچ لینوکس را دانلود و روی فلش بوتیبل کردم و خواستم نصب کنم دیدم موقع بوت از روی فلش پیغام خطای زیر را میده

{{< highlight shell >}}

Invalid signature detected check secure boot policy

{{< / highlight >}}

دوباره سرچ. و متوجه شدم مربوط به uefi هست. برای همین رفتم توی قسمت setup سیستم و uefi را غیر فعال کردم. و بصورت اتفاقی فلش داخل پورت نبود تا از روی فلش بوت بشه. و دیدم که لینوکس مینت بالا اومد. و دیگه از اون خطا خبری نبود.

کلی خوشحال شدم :)))) و دوباره رفتم سراغ update manager و زدم لیست را Refresh کردم تا اگر آپدیت جدیدی هست نمایش بده. دیدم پیغام خطا میده که نمیتونه لیست را اپدیت کنه و باید دستور زیر را اجرا کنید:


{{< highlight shell >}}

sudo dpkg --configure -a

{{< / highlight >}}

من هم این دستور را اجرا کردم و دوباره تعداد خیلی زیادی پکیج و نرم افزار شروع به آپدیت شدن کردن. چندین چندبار هم پیغام میداد که میخوای همین لوکال را استفاده کنی یا آپدیت جدید میخوای؟ منم همه را میزدم آپدیت جدید.
مخصوصا در مورد uefi چندین بار پکیج نصب کرد. و هربار میزدم جدیدها را نصب کن.

خلاصه بعد از حدود نیم ساعت نصب و بروزرسانی ها تمام شد. و سیستم را ریست کردم و دیدم همه چیز اوکی هست.

بعد از اینکه همه چی اوکی بود دوباره رفتم توی setup سیستم و uefi را فعال کردم. و دیدم هیچ اتفاق خاصی نیافتاد و همه چیز مثل روز اول .

انصافا ui این لینوکس مینت ۱۹ خیلی خوشگل شده :)) 

## نکته جدید

این نکته را بعد از کار کردن با سیستم متوجه شدم و اون اینکه بعضی از کانفیگ هاتون مثل کانفیگ های مربوط به Tor یا appache2 را باید مجددا انجام بدید.

مثلا من مسیر /var/www را به home منتقل کردم که نیازمند این هست که فایل کانفیگ آپاچی را تغییر بدید. بعد از upgrade و داستان های مربوطه این تنظیمات به حالت پیش فرض برگشته بود که کافیه مجددا تنظیمات را انجام بدید.