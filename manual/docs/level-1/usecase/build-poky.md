---
title: pokyをビルドする
---

# pokyをビルドする
タイトルの通り、pokyをビルドしてみましょう。まずはpokyリポジトリをcloneします  
使用するブランチは[こちら](https://wiki.yoctoproject.org/wiki/Releases)から選んでください。ここでは{{YOCTO_BRANCH}}ブランチを選択しています  

```bash
$ git clone https://git.yoctoproject.org/git/poky -b {{YOCTO_BRANCH}}
$ cd poky
```

ディレクトリの中身を確認してみましょう。以下のようになっているはずです  

```bash
$ ll
total 108
drwxr-xr-x 12 ./
drwxr-xr-x  5 ../
-rw-r--r--  1 .git
-rw-r--r--  1 .gitignore
-rw-r--r--  1 .templateconf
-rw-r--r--  1 LICENSE
-rw-r--r--  1 LICENSE.GPL-2.0-only
-rw-r--r--  1 LICENSE.MIT
-rw-r--r--  1 MAINTAINERS.md
-rw-r--r--  1 MEMORIAM
-rw-r--r--  1 Makefile
-rw-r--r--  1 README.OE-Core.md
lrwxrwxrwx  1 README.hardware.md -> meta-yocto-bsp/README.hardware.md
lrwxrwxrwx  1 README.md -> README.poky.md
lrwxrwxrwx  1 README.poky.md -> meta-poky/README.poky.md
-rw-r--r--  1 README.qemu.md
drwxr-xr-x  6 bitbake/
drwxr-xr-x  7 build/
drwxr-xr-x  4 contrib/
drwxr-xr-x 19 documentation/
drwxr-xr-x 22 meta/
drwxr-xr-x  5 meta-poky/
drwxr-xr-x  9 meta-selftest/
drwxr-xr-x  8 meta-skeleton/
drwxr-xr-x  8 meta-yocto-bsp/
-rwxr-xr-x  1 oe-init-build-env*
drwxr-xr-x 10 scripts/
```

シンボリックリンクや.git等不要ファイルを削除し、[Yoctoのモデル Level-1](../model.md) の粒度に整理します  

```bash
drwxr-xr-x  7 build/                  -> linuxカーネルとrootfsが出力されるディレクトリ
drwxr-xr-x  6 bitbake/                -> ビルド用のコマンド(のスクリプトファイルをまとめたディレクトリ)
-rwxr-xr-x  1 oe-init-build-env*      -> 初期化用スクリプト
-rw-r--r--  1 LICENSE                 ┐
-rw-r--r--  1 LICENSE.GPL-2.0-only    │
-rw-r--r--  1 LICENSE.MIT             │
-rw-r--r--  1 MAINTAINERS.md          │
-rw-r--r--  1 MEMORIAM                │
-rw-r--r--  1 Makefile                │
-rw-r--r--  1 README.OE-Core.md       ├> yocto設定ファイル
-rw-r--r--  1 README.qemu.md          │
drwxr-xr-x  4 contrib/                │
drwxr-xr-x 19 documentation/          │
drwxr-xr-x 22 meta/                   │
drwxr-xr-x  5 meta-poky/              │
drwxr-xr-x  9 meta-selftest/          │
drwxr-xr-x  8 meta-skeleton/          │
drwxr-xr-x  8 meta-yocto-bsp/         │
drwxr-xr-x 10 scripts/                ┘
```

次に、pokyをビルドします。[Yoctoのモデル Level-1](../model.md)の通り、以下の手順でビルドします  
(最初のビルドにはかなり時間がかかるため、時間があるときに実施してください)  

1. 環境変数を設定する
1. bitbakeコマンドを実行する

```bash
# 環境変数を設定する
$ source oe-init-build-env

# ビルド用のコマンドを実行する
$ bitbake core-image-minimal 
```

`core-image-minimal`については、今は気にしなくて良いです   
build/ディレクトリの中(build/tmp/deploy/images/qemux86-64/)を確認すると、linuxカーネルやrootfsが出力されていることが確認できます  

```bash
yosuke@DESKTOP-Q0SLK3M:~/work/git/yocto-learning/poky/build/tmp/deploy/images/qemux86-64$ ll
total 52388
drwxr-xr-x 2 ./
drwxr-xr-x 3 ../
lrwxrwxrwx 2 bzImage -> bzImage--5.19.3+git0+5eb0fa93f8_4d93345670-r0-qemux86-64-20220909143216.bin                       -> linuxカーネル
-rw-r--r-- 2 bzImage--5.19.3+git0+5eb0fa93f8_4d93345670-r0-qemux86-64-20220909143216.bin
lrwxrwxrwx 2 bzImage-qemux86-64.bin -> bzImage--5.19.3+git0+5eb0fa93f8_4d93345670-r0-qemux86-64-20220909143216.bin
-rw-r--r-- 2 core-image-minimal-qemux86-64-20220909143216.qemuboot.conf
-rw-r--r-- 2 core-image-minimal-qemux86-64-20220909143216.rootfs.ext4                                                      -> rootfs
-rw-r--r-- 2 core-image-minimal-qemux86-64-20220909143216.rootfs.manifest
-rw-r--r-- 2 core-image-minimal-qemux86-64-20220909143216.rootfs.tar.bz2
-rw-r--r-- 2 core-image-minimal-qemux86-64-20220909143216.testdata.json
lrwxrwxrwx 2 core-image-minimal-qemux86-64.ext4 -> core-image-minimal-qemux86-64-20220909143216.rootfs.ext4
lrwxrwxrwx 2 core-image-minimal-qemux86-64.manifest -> core-image-minimal-qemux86-64-20220909143216.rootfs.manifest
lrwxrwxrwx 2 core-image-minimal-qemux86-64.qemuboot.conf -> core-image-minimal-qemux86-64-20220909143216.qemuboot.conf
lrwxrwxrwx 2 core-image-minimal-qemux86-64.tar.bz2 -> core-image-minimal-qemux86-64-20220909143216.rootfs.tar.bz2
lrwxrwxrwx 2 core-image-minimal-qemux86-64.testdata.json -> core-image-minimal-qemux86-64-20220909143216.testdata.json
-rw-r--r-- 2 modules--5.19.3+git0+5eb0fa93f8_4d93345670-r0-qemux86-64-20220909143216.tgz
lrwxrwxrwx 2 modules-qemux86-64.tgz -> modules--5.19.3+git0+5eb0fa93f8_4d93345670-r0-qemux86-64-20220909143216.tgz
```