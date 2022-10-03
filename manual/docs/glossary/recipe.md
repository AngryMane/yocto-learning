---
title: レシピファイル
glossary: レシピファイル
glossary: レシピ
---

# 概要

**bitbakeのパッケージのパラメータ(リポジトリのURL等)を定義したファイルです**  
レシピファイルだとちょっと長いので、単にレシピと呼ぶこともあります  

# 具体例

alsa-libパッケージのレシピを見てみましょう  
レシピを探す方法には`bitbake-getvar`や`bitbake -e`などツールを使う方法がありますが、今回はfindコマンドで探します   
特定のパッケージのレシピファイルをfindコマンドで探す場合、以下のルールを手がかりにすると良いかもしません   

* 拡張子は.bb
* ファイル名はパッケージ名を含むことが多い。例えば、`パッケージ名_バージョン.bb`(**多数の例外あり**)
* 多くのレシピファイルは`meta`という接頭辞がついたディレクトリの中に存在する(**多数の例外あり**)

~~~bash
$ find ./meta* -name "alsa-lib*"
./meta/recipes-multimedia/alsa/alsa-lib_1.2.7.2.bb
~~~

見つけたレシピファイルをcatコマンドで確認してみます  

~~~bash
$ cat ./meta/recipes-multimedia/alsa/alsa-lib_1.2.7.2.bb
SUMMARY = "ALSA sound library"
DESCRIPTION = "(Occasionally a.k.a. libasound) is a userspace library that \
provides a level of abstraction over the /dev interfaces provided by the kernel modules."
HOMEPAGE = "http://www.alsa-project.org"
BUGTRACKER = "http://alsa-project.org/main/index.php/Bug_Tracking"
SECTION = "libs/multimedia"
LICENSE = "LGPL-2.1-only & GPL-2.0-or-later"
LIC_FILES_CHKSUM = "file://COPYING;md5=a916467b91076e631dd8edb7424769c7 \
                    file://src/socket.c;md5=285675b45e83f571c6a957fe4ab79c93;beginline=9;endline=24 \
                    "

SRC_URI = "https://www.alsa-project.org/files/pub/lib/${BP}.tar.bz2"
SRC_URI[sha256sum] = "8a35b7218e50f2a2c79342d0de98ded81439ce19e12809385ec9be9596de7c2f"
// 以下略
~~~

`パッケージのサマリ`や`ライセンスの情報`、`リポジトリのURI`など、パッケージ固有の設定を定義していることが分かります  
各変数の明確な定義はここでは紹介しません  