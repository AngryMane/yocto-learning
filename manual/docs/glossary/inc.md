---
title: incファイル
glossary: inc
glossary: incファイル
---

# 概要
<!--outline-start-->

レシピファイルが参照するファイルです    
レシピファイルは `include` あるいは `require` を用いてincファイルを読み込み、incファイルの内容をその場所に挿入します  

レシピファイルが巨大すぎるときなどに記述内容を整理するのに使います  

<!--outline-end-->

# 具体例

<!--example-start-->

acpidパッケージがちょうどよさそうなので、このパッケージを具体例として見てみましょう  
まずはacpidパッケージのファイルパスと、そこにどのようなファイルが存在するのかをみてみます  

~~~bash
$ find ./* -name acpid*.bb
./meta/recipes-bsp/acpid/acpid_2.0.33.bb
$ ll ./meta/recipes-bsp/acpid
合計 20
drwxrwxr-x  3 4096  8月 14 12:19 ./
drwxrwxr-x 21 4096  8月 14 12:19 ../
drwxrwxr-x  2 4096  8月 14 12:19 acpid/
-rw-rw-r--  1 1369  8月 14 12:19 acpid.inc
-rw-rw-r--  1 266   8月 14 12:19 acpid_2.0.33.bb
~~~

次にacpidパッケージのレシピファイルの中身を確認しましょう  

~~~bash
$ cat ./meta/recipes-bsp/acpid/acpid_2.0.33.bb
require acpid.inc

LIC_FILES_CHKSUM = "file://COPYING;md5=8ca43cbc842c2336e835926c2166c28b \
                    file://acpid.h;endline=24;md5=324a9cf225ae69ddaad1bf9d942115b5"

SRC_URI[sha256sum] = "0856f71b3eb34a1b663d0a8e6363dfcbc519e63d847330498898658e2972dbe8"
~~~

ファイル先頭で`require`句を使ってincファイルを参照していますね。 では次に、incファイルの中身を確認してみます  

~~~bash
$ cat ./meta/recipes-bsp/acpid/acpid.inc 
SUMMARY = "A daemon for delivering ACPI events"
DESCRIPTION = "ACPID is a completely flexible, totally extensible daemon for \
delivering ACPI events. It listens on netlink interface (or on the \
deprecated file /proc/acpi/event), and when an event occurs, executes programs \
to handle the event. The programs it executes are configured through a set of \
configuration files, which can be dropped into place by packages or by the \
admin."
HOMEPAGE = "http://sourceforge.net/projects/acpid2"
BUGTRACKER = "http://sourceforge.net/p/acpid2/tickets/?source=navbar"
SECTION = "base"
LICENSE = "GPL-2.0-or-later"

// 以下incファイルの中身を省略
~~~

このファイルがacpidパッケージのレシピファイルの先頭でrequireされているため、acpidパッケージのレシピは以下と同等になります  

~~~bash
SUMMARY = "A daemon for delivering ACPI events"
DESCRIPTION = "ACPID is a completely flexible, totally extensible daemon for \
delivering ACPI events. It listens on netlink interface (or on the \
deprecated file /proc/acpi/event), and when an event occurs, executes programs \
to handle the event. The programs it executes are configured through a set of \
configuration files, which can be dropped into place by packages or by the \
admin."
HOMEPAGE = "http://sourceforge.net/projects/acpid2"
BUGTRACKER = "http://sourceforge.net/p/acpid2/tickets/?source=navbar"
SECTION = "base"
LICENSE = "GPL-2.0-or-later"

// 以下incファイルの中身を省略

LIC_FILES_CHKSUM = "file://COPYING;md5=8ca43cbc842c2336e835926c2166c28b \
                    file://acpid.h;endline=24;md5=324a9cf225ae69ddaad1bf9d942115b5"

SRC_URI[sha256sum] = "0856f71b3eb34a1b663d0a8e6363dfcbc519e63d847330498898658e2972dbe8"
~~~


<!--example-end-->