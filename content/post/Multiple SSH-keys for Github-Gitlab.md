+++
title = "Multiple SSH-keys for Github/Gitlab"
description = ""
author = ""
date = 2018-08-29T18:23:47+04:30
tags = []
draft = false
+++

با سلام خدمت همه دوستان

در این پست قرار هست نحوه  استفاده از چند ssh-key برای اکانت های مختلف گیت‌هاب یا گیت‌لب را با هم بررسی کنیم.

خب ممکنه شما چندتا اکانت گیت‌هاب یا گیت‌لب داشته باشید و یا حتی هم اکانت گیت هاب و هم اکانت گیت‌لب دارید و مدام برای پروژه هاتون کدنویسی میکنید.
برای اینکه بتونید از چندتا ssh-key مختلف استفاده کنید کافیه ادامه این پست را همراه من باشید :)

## Github

ابتدا میریم سراغ گیت‌هاب. برای اینکار باید اول از همه ssh برای خودمون بسازیم. و برای این کار کافی هست از دستور زیر در ترمینال استفاده کنید:

{{< highlight shell >}}

$ ssh-keygen -t rsa -C "your_name@home_email.com"

{{< / highlight >}}

 در اینجا با استفاده از ssh-kegen ما یک کلید جدید generate می کنیم که نوع اون rsa هست و قسمت آخر هم your_name@home_email.com آدرس ایمیل تون را باید وارد کنید.

بعد از اجرای دستور بالا خروجی شبیه زیر را مشاهده خواهید کرد:

{{< highlight shell >}}

Generating public/private rsa key pair. 
Enter file in which to save the key (/home/user_name/.ssh/id_rsa):

{{< / highlight >}}

در این مرحله باید یک اسم یکتا وارد کنید. مثلا فرض کنید  من برای پروژه های مربوط به خودم اسم زیر را وارد میکنم:

{{< highlight shell >}}

id_rsa_personal

{{< / highlight >}}

توجه داشته باشید که این نام دلخواه هست و هر اسمی که بخواهید میتونید براش قرار بدید.

بعد از نوشتن اسم کلید Enter را فشار بدید تا وارد مرحله بعدی بشیم. در مرحله جدید از شما میخواد که یک پسورد وارد کنید و این پسورد را دو مرتبه باید وارد و Enter بزنید.

تا اینجا شما برای اکانت خودتون یک SSH-Key درست کردید.

حالا فرض کنید میخواید یک ssh-key هم برای پروژه های مربوط به شرکتی که درش کار میکنید درست کنید. مراحل دقیقا مثل مراحل بالا هست. به ترتیب

{{< highlight shell >}}

$ ssh-keygen -t rsa -C "your_name@company_email.com"

{{< / highlight >}}

وارد کردن یک نام یکتا:

{{< highlight shell >}}

id_rsa_company

{{< / highlight >}}

و بعد هم وارد کردن پسورد.

برای اینکه مطمئن بشید کلیدهای عمومی و خصوصی ایجاد شدند کافیه لیست فایل های داخل پوشه .ssh را نگاه کنید:

{{< highlight shell >}}

$ ls ~/.ssh

id_rsa_personal  id_rsa_company  id_rsa_personal.pub  id_rsa_company.pub

{{< / highlight >}}

اما حالا وقتش رسیده تا فایل کانفیگ مربوط به مدیریت کلیدها را درست کنیم. با یک ویرایشگر یک فایل به نام config در پوشه .ssh ایجاد کنید. در اینجا من از ویرایشگر vim‌استفاده می کنم .

{{< highlight shell >}}

$ cd ~/.ssh/
$ vim config

{{< / highlight >}}

داخل فایل کانفیگ کد زیر را قرار بدید:

{{< highlight shell >}}

# personal account
Host personal.github.com
  HostName github.com
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/id_rsa_personal

{{< / highlight >}}


{{< highlight shell >}}

# Company account
Host company.github.com
  HostName github.com
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/id_rsa_company

{{< / highlight >}}


  مرحله بعدی هم پاک کردن کش کلیدها هست که با دستور زیر میتونید این کار را انجام بدید:

{{< highlight shell >}}

$ ssh-add -D

{{< / highlight >}}

اگر در این مرحله با خطای زیر مواجه شدید

{{< highlight shell >}}

Could not open a connection to your authentication agent.

{{< / highlight >}}

دستور زیر را اجرا کنید :

{{< highlight shell >}}

eval `ssh-agent -s`

{{< / highlight >}}

و دوباره کش را پاک کنید:

{{< highlight shell >}}

$ ssh-add -D

{{< / highlight >}}

خب حالا باید چک کنیم که آیا کلیدها درست اضافه شدند یا نه. برای اینکار دستور زیر را اجرا کنید:

{{< highlight shell >}}

$ ssh-add -l

{{< / highlight >}}


خروجی چیزی شبیه این خواهد بود:

{{< highlight shell >}}

2048 d4:e0:39:e1:bf:6f:e3:26:14:6b:26:73:4e:b4:53:83 /home/hossein52hz/.ssh/id_rsa_personal (RSA)
2048 7a:32:06:3f:3d:6c:f4:a1:d4:65:13:64:a4:ed:1d:63 /home/hossein52hz/.ssh/id_rsa_company (RSA)

{{< / highlight >}}

اما اگر خروجی شما خالی بود باید کلیدها را دستی اضافه کنید. برای اینکار دستورات زیر را اجرا کنید:

{{< highlight shell >}}

ssh-add ~/.ssh/id_rsa_company
ssh-add ~/.ssh/id_rsa_personal

{{< / highlight >}}


توی این مرحله باید کلید عمومی را کپی کنیم. برای اینکار میتونید از دستور زیر استفاده کنید

{{< highlight shell >}}

xclip -sel clip < ~/.ssh/id_rsa_personal.pub

{{< / highlight >}}


با این دستور کلید عمومی id_rsa_personal در clipboard کپی شده و فقط کافیه داخل اکانت گیت‌هاب خودتون برید و اونجا کلید را paste کنید.

برای این کار وارد آدرس زیر بشید

{{< highlight shell >}}

https://github.com/settings/keys

{{< / highlight >}}



و روی دکمه New ssh key کلیک کنید.  توی پنجره باز شده کلید خودتون را کپی کنید. و یک نام هم براش انتخاب کنید و ذخیره.

این کار را برای کلید id_rsa_company.pub هم تکرار کنید.

حالا برای تست ارتباط میتونید دستور زیر را اجرا کنید:


{{< highlight shell >}}

$ ssh -T git@home.github.com
Hi Hossein52hz! You've successfully authenticated, but GitHub does not provide shell access.

{{< / highlight >}}


{{< highlight shell >}}

$ ssh -T git@work.github.com
Hi Hossein52hz! You've successfully authenticated, but GitHub does not provide shell access.

{{< / highlight >}}


تبریک میگم شما موفق شدید :)




## Gitlab

اما در مورد گیت‌لب تمام مراحل شبیه هم هستند به جز فایل config که برای کلیدهایی که ساختید کافیه وارد فایل config توی پوشه .ssh بشید
و کد زیر را اضافه کنید

 {{< highlight shell >}}

Host gitlab.company_url.com
   HostName gitlab.company_url.com
   PreferredAuthentications publickey
   IdentityFile ~/.ssh/id_rsa_gitlab_company

{{< / highlight >}}
# GITLAB


همونطور که متوجه شدید من یک کلید به اسم id_rsagitlab_company ایجاد کردم.
 و بقیه مراحل هم مشابه مراحل بالا است. برای اضافه کردن کلید به گیت‌لب هم میتونید از آدرس زیر استفاده کنید:
 
 {{< highlight shell >}}

 https://gitlab.com/profile/keys

{{< / highlight >}}

 و برای تست اینکه همه چیز درست است یا نه از دستور زیر استفاده کنید:


 {{< highlight shell >}}

$ ssh -T git@gitlab.company_url.com
    
 Welcome to GitLab, Hossein52hz!

{{< / highlight >}}

در پایان هم اگر نکته ای هست که بتونه در بهتر شدن این نوشته کمک کنه خوشحال میشم در قسمت نظرات بیان کنید.

موفق باشید