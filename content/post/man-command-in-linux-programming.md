+++
title = "نکته ای جالب در مورد دستور man در لینوکس"
description = ""
author = ""
date = 2019-03-18T23:37:29+03:30
tags = []
draft = true
+++

اگر از کاربرهای سیستم عامل های لینوکسی باشید احتمال خیلی زیاد با دستور man برخوردید و یا همیشه از این دستور استفاده می کنید.

معمول ترین استفاده از این دستور برای مشاهده راهنمای یک دستور دیگه و همچنین مشاهده سوئیچ هایی که میشه با اون دستور بکار برد و خلاصه یک راهنمایی کامل در مورد نحوه استفاده از اون دستور است.

اما شاید جالب باشه که بدونید این دستور میتونه همراه با ۹ تا عدد دیگه هم استفاده بشه که در زیر لیست این اعداد و کاربردشون نمایش داده شده:

{{< highlight shell >}}

       1   Executable programs or shell commands
       2   System calls (functions provided by the kernel)
       3   Library calls (functions within program libraries)
       4   Special files (usually found in /dev)
       5   File formats and conventions eg /etc/passwd
       6   Games
       7   Miscellaneous  (including  macro  packages  and  conventions), e.g.
           man(7), groff(7)
       8   System administration commands (usually only for root)
       9   Kernel routines [Non standard]

{{< /highlight >}}

اگر از مقدار عددی در دستور man استفاده نکنید بصورت پیش فرض کوچک ترین عدد در نظر گرفته و اجرا میشه.

به عنوان مثال اگر در مورد نحوه استفاده از دستور chmod بخواهید اطلاعاتی بدست بیارید کافیه دستور زیر را داخل ترمینال اجرا کنید:

{{< highlight shell >}}

man chmod
{{< /highlight >}}

این دستور معادل دستور زیر میشه:

{{< highlight shell >}}
man 1 chmod
{{< /highlight >}}

اما اگر بخواهید در مورد فراخوانی های سیستمی یا همون system callsها اطلاعات بیشتری در مورد این دستور پیدا کنید کافیه از عدد ۲ همراه با این دستور استفاده کنید تا راهنمای استفاده از این دستور به عنوان یک system call توی برنامه نویسی تون اطلاعات بیشتری بدست بیارید.
به عنوان مثال 

{{< highlight shell >}}

man 2 chmod
{{< /highlight >}}

خروجی دستور بالا بصورت زیر است:

{{< highlight shell >}}

NAME
       chmod, fchmod, fchmodat - change permissions of a file

SYNOPSIS
       #include <sys/stat.h>

       int chmod(const char *pathname, mode_t mode);
       int fchmod(int fd, mode_t mode);

       #include <fcntl.h>           /* Definition of AT_* constants */
       #include <sys/stat.h>

       int fchmodat(int dirfd, const char *pathname, mode_t mode, int flags);

   Feature Test Macro Requirements for glibc (see feature_test_macros(7)):

       fchmod():
           Since glibc 2.24:
               _POSIX_C_SOURCE >= 199309L
           Glibc 2.19 to 2.23
               _POSIX_C_SOURCE
           Glibc 2.16 to 2.19:
               _BSD_SOURCE || _POSIX_C_SOURCE
           Glibc 2.12 to 2.16:
               _BSD_SOURCE || _XOPEN_SOURCE >= 500 ||
                   _POSIX_C_SOURCE >= 200809L
           Glibc 2.11 and earlier:
               _BSD_SOURCE || _XOPEN_SOURCE >= 500

       fchmodat():
           Since glibc 2.10:
               _POSIX_C_SOURCE >= 200809L
           Before glibc 2.10:
               _ATFILE_SOURCE
...

{{< /highlight >}}