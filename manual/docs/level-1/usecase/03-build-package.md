---
title: その他(OS以外)のパッケージをビルドする
---

> **NOTE**
> このページを読む前に、[レベル1に必要な知識](../model.md)を確認してください  

> **NOTE**
> サンプルOSのビルドについては、[サンプルOSをビルドする](./02-build-sample-os.md)をご参照ください  

# その他(OS以外)のパッケージをビルドする
その他(OS以外)のパッケージをビルドしてみましょう。まだpokyリポジトリをcloneしていない場合、cloneします  
使用するブランチは[こちら](https://wiki.yoctoproject.org/wiki/Releases)から選んでください。ここでは{{YOCTO_BRANCH}}ブランチを選択しています  

```bash
$ git clone https://git.yoctoproject.org/git/poky -b {{YOCTO_BRANCH}}
$ cd poky
```

次に、その他(OS以外)のパッケージをビルドします。ここでは、`busybox`というソフトウェアをビルドします  

[レベル1に必要な知識](../model.md)の通り、以下の手順でビルドします  

1. 環境変数を設定する
1. bitbakeコマンドを実行する

```bash
# 環境変数を設定する
$ source oe-init-build-env

# ビルドコマンドを実行する
$ bitbake busybox
```

build/ディレクトリの中(build/tmp/deploy/rpm/core2_64)を確認すると、busyboxのインストーラが出力されていることが確認できます  

```bash
$ ll
total 3772
drwxr-xr-x 2 yosuke yosuke    4096 Sep 25 10:25 ./
drwxr-xr-x 3 yosuke yosuke    4096 Sep 25 10:25 ../
-rw-r--r-- 2 yosuke yosuke  389442 Sep 25 10:25 busybox-1.35.0-r0.core2_64.rpm
-rw-r--r-- 2 yosuke yosuke 1320159 Sep 25 10:25 busybox-dbg-1.35.0-r0.core2_64.rpm
-rw-r--r-- 2 yosuke yosuke    6561 Sep 25 10:25 busybox-dev-1.35.0-r0.core2_64.rpm
-rw-r--r-- 2 yosuke yosuke    8792 Sep 25 10:25 busybox-hwclock-1.35.0-r0.core2_64.rpm
-rw-r--r-- 2 yosuke yosuke  202015 Sep 25 10:25 busybox-ptest-1.35.0-r0.core2_64.rpm
-rw-r--r-- 2 yosuke yosuke 1064696 Sep 25 10:25 busybox-src-1.35.0-r0.core2_64.rpm
-rw-r--r-- 2 yosuke yosuke   10029 Sep 25 10:25 busybox-syslog-1.35.0-r0.core2_64.rpm
-rw-r--r-- 2 yosuke yosuke    8241 Sep 25 10:25 busybox-udhcpc-1.35.0-r0.core2_64.rpm
-rw-r--r-- 2 yosuke yosuke    8224 Sep 25 10:25 busybox-udhcpd-1.35.0-r0.core2_64.rpm
-rw-r--r-- 2 yosuke yosuke   15634 Sep 25 10:25 ptest-runner-2.4.2+git0+bcb82804da-r0.core2_64.rpm
-rw-r--r-- 2 yosuke yosuke   31537 Sep 25 10:25 ptest-runner-dbg-2.4.2+git0+bcb82804da-r0.core2_64.rpm
-rw-r--r-- 2 yosuke yosuke    6205 Sep 25 10:25 ptest-runner-dev-2.4.2+git0+bcb82804da-r0.core2_64.rpm
-rw-r--r-- 2 yosuke yosuke   14326 Sep 25 10:25 ptest-runner-src-2.4.2+git0+bcb82804da-r0.core2_64.rpm
-rw-r--r-- 2 yosuke yosuke  143713 Sep 25 10:23 zip-3.0-r2.core2_64.rpm
-rw-r--r-- 2 yosuke yosuke  321457 Sep 25 10:23 zip-dbg-3.0-r2.core2_64.rpm
-rw-r--r-- 2 yosuke yosuke    6089 Sep 25 10:23 zip-dev-3.0-r2.core2_64.rpm
-rw-r--r-- 2 yosuke yosuke   34729 Sep 25 10:23 zip-doc-3.0-r2.core2_64.rpm
-rw-r--r-- 2 yosuke yosuke  219198 Sep 25 10:23 zip-src-3.0-r2.core2_64.rpm
```