---
title: サンプルOSをビルドする
---

!!! NOTE

    このページを読む前に、[yocto/bitbake/pokyとは](../preamble.md)を確認してください  

# サンプルOSをビルドする
pokyに含まれるサンプルOSをビルドしてみましょう。まずはpokyリポジトリをcloneします  
使用するブランチは[こちら](https://wiki.yoctoproject.org/wiki/Releases)から選んでください。ここでは{{YOCTO_BRANCH}}ブランチを選択しています  

~~~bash
$ git clone https://git.yoctoproject.org/git/poky -b {{YOCTO_BRANCH}}
$ cd poky
~~~

次に、サンプルOSをビルドします。ここでは、`core-image-minimal`という名前のサンプルOSをビルドします  
[yocto/bitbake/pokyとは](../preamble.md) の通り、以下の手順でビルドします  

1. `ビルド環境を設定するスクリプト` を実行する
1. `bitbakeコマンド` を実行する

!!! NOTE

    最初のビルドにはかなり時間がかかるため、時間があるときに実施してください  

~~~bash
# ビルド環境を設定するスクリプト を実行する
$ source oe-init-build-env

# bitbakeコマンド を実行する
$ bitbake core-image-minimal 
~~~

build/ディレクトリの中(build/tmp/deploy/images/qemux86-64/)を確認すると、linuxカーネルやrootfsが出力されていることが確認できます  

~~~bash
$ ll
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
~~~