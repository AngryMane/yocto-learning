---
title: サンプルOSをビルドする
---

## 先に読むべきページ
* [Yocto](../component/01-yocto.md)


## ビルド環境をセットアップする

pokyリポジトリをcloneしてください  
使用するブランチは[**こちら(リンク)**](https://wiki.yoctoproject.org/wiki/Releases)から選んでください。ここでは{{YOCTO_BRANCH}}ブランチを選択しています  

~~~bash
$ git clone https://git.yoctoproject.org/git/poky -b {{YOCTO_BRANCH}}
$ cd poky
~~~

次に、pokyリポジトリ直下のoe-init-build-envを読み込みます    
これはビルドに必要なコマンドへのパスを通すなど、セットアップに必要な処理を実行してくれます  

~~~bash
# ビルド環境を設定するスクリプトを実行する
$ source oe-init-build-env
~~~

## ビルドを実行する

ビルド環境をセットアップ後、サンプルOSをビルドします。ここでは `core-image-minimal` という名前のサンプルOSをビルドします  
セットアップでbitbakeのパスが通っているため、以下とおり実行できます  

~~~bash
# ビルド用のコマンド(bitbakeコマンド)を実行する
$ bitbake core-image-minimal 
~~~

!!! NOTE

    最初のビルドにはかなり時間がかかるため、時間があるときに実施してください  

buildディレクトリの中(`poky/build/tmp/deploy/images/qemux86-64/`)を確認すると、linuxカーネルやrootfsが出力されていることが確認できます  
(この表ではシンボリックリンク等不要なファイルを削除しています)  

~~~bash
$ cd build/tmp/deploy/images/qemux86-64
$ ll
bzImage--5.19.3+git0+5eb0fa93f8_4d93345670-r0-qemux86-64-20220909143216.bin   -> linuxカーネル
core-image-minimal-qemux86-64-20220909143216.qemuboot.conf                    -> qemuの設定ファイル
core-image-minimal-qemux86-64-20220909143216.rootfs.ext4                      -> rootfs
core-image-minimal-qemux86-64-20220909143216.rootfs.manifest                  -> カスタマイズしたlinux OSに何が入っているかをまとめた情報
core-image-minimal-qemux86-64-20220909143216.rootfs.tar.bz2                   -> rootfsのtar.bz2. docker importするとコンテナにできる
core-image-minimal-qemux86-64-20220909143216.testdata.json                    -> ビルド時の変数設定などのデータが入っている
modules--5.19.3+git0+5eb0fa93f8_4d93345670-r0-qemux86-64-20220909143216.tgz   -> カーネルモジュール
~~~
