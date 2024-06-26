---
title: その他(OS以外)のパッケージをビルドする
---

## 先に読むべきページ
* [Yocto](../component/01-yocto.md)

## その他(OS以外)のパッケージをビルドする
その他(OS以外)のパッケージをビルドしてみましょう。まだpokyリポジトリをcloneしていない場合、cloneします  
使用するブランチは[こちら](https://wiki.yoctoproject.org/wiki/Releases)から選んでください。ここでは{{YOCTO_BRANCH}}ブランチを選択しています  

~~~bash
$ git clone https://git.yoctoproject.org/git/poky -b {{YOCTO_BRANCH}}
$ cd poky
~~~

次に、その他(OS以外)のパッケージをビルドします。ここでは、`busybox`というソフトウェアをビルドします  
[yocto/bitbake/pokyとは](../study.md)の通り、以下の手順でビルドします  

1. `ビルド環境を設定するスクリプト` を実行する
1. `bitbakeコマンド` を実行する

~~~bash
# ビルド環境を設定するスクリプト を実行する
$ source oe-init-build-env

# bitbakeコマンド を実行する
$ bitbake busybox
~~~

build/ディレクトリの中(build/tmp/deploy/rpm/core2_64)を確認すると、busyboxのインストーラが出力されていることが確認できます  

~~~bash
$ ll
total 3772
drwxr-xr-x 2    4096 Sep 25 10:25 ./
drwxr-xr-x 3    4096 Sep 25 10:25 ../
-rw-r--r-- 2  389442 Sep 25 10:25 busybox-1.35.0-r0.core2_64.rpm
-rw-r--r-- 2 1320159 Sep 25 10:25 busybox-dbg-1.35.0-r0.core2_64.rpm
-rw-r--r-- 2    6561 Sep 25 10:25 busybox-dev-1.35.0-r0.core2_64.rpm
-rw-r--r-- 2    8792 Sep 25 10:25 busybox-hwclock-1.35.0-r0.core2_64.rpm
-rw-r--r-- 2  202015 Sep 25 10:25 busybox-ptest-1.35.0-r0.core2_64.rpm
-rw-r--r-- 2 1064696 Sep 25 10:25 busybox-src-1.35.0-r0.core2_64.rpm
-rw-r--r-- 2   10029 Sep 25 10:25 busybox-syslog-1.35.0-r0.core2_64.rpm
-rw-r--r-- 2    8241 Sep 25 10:25 busybox-udhcpc-1.35.0-r0.core2_64.rpm
-rw-r--r-- 2    8224 Sep 25 10:25 busybox-udhcpd-1.35.0-r0.core2_64.rpm
-rw-r--r-- 2   15634 Sep 25 10:25 ptest-runner-2.4.2+git0+bcb82804da-r0.core2_64.rpm
-rw-r--r-- 2   31537 Sep 25 10:25 ptest-runner-dbg-2.4.2+git0+bcb82804da-r0.core2_64.rpm
-rw-r--r-- 2    6205 Sep 25 10:25 ptest-runner-dev-2.4.2+git0+bcb82804da-r0.core2_64.rpm
-rw-r--r-- 2   14326 Sep 25 10:25 ptest-runner-src-2.4.2+git0+bcb82804da-r0.core2_64.rpm
-rw-r--r-- 2  143713 Sep 25 10:23 zip-3.0-r2.core2_64.rpm
-rw-r--r-- 2  321457 Sep 25 10:23 zip-dbg-3.0-r2.core2_64.rpm
-rw-r--r-- 2    6089 Sep 25 10:23 zip-dev-3.0-r2.core2_64.rpm
-rw-r--r-- 2   34729 Sep 25 10:23 zip-doc-3.0-r2.core2_64.rpm
-rw-r--r-- 2  219198 Sep 25 10:23 zip-src-3.0-r2.core2_64.rpm
~~~