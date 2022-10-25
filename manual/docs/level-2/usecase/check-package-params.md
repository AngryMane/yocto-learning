---
title: パッケージのパラメータを確認する
---


!!! NOTE

    このページを読む前に、[レシピファイルとincファイル](../preamble.md)を確認してください  

# パッケージのパラメータを確認する

ここではパッケージの実際のパラメータを確認する方法を紹介します  

## bitbakeコマンドを使う方法

一番簡単なbitbakeコマンドを使う方法を紹介します  
まずは、以下のように`ビルド環境を設定するスクリプト`を実行して、bitbakeコマンドへのパスを通します  

~~~bash
$ source oe-init-build-env
~~~

次に、以下のように-eオプションをつけてbitbakeコマンドを実行します  

~~~bash
$ bitbake -e gcc
#
# $ABIEXTENSION:class-crosssdk
#   set /home/yosuke/work/git/yocto-learning/poky/meta/conf/machine-sdk/x86_64.conf:2
#     ""
ABIEXTENSION:class-crosssdk=""
#
# $ABIEXTENSION:class-nativesdk
#   set /home/yosuke/work/git/yocto-learning/poky/meta/conf/machine-sdk/x86_64.conf:3
#     ""
ABIEXTENSION:class-nativesdk=""
#
# $ACLOCALDIR
#   set /home/yosuke/work/git/yocto-learning/poky/meta/classes-recipe/autotools.bbclass:148
#     "${STAGING_DATADIR}/aclocal"
ACLOCALDIR="/home/yosuke/work/git/yocto-learning/poky/build/tmp/work/core2-64-poky-linux/gcc/12.2.0-r0/recipe-sysroot/usr/share/aclocal"

# 以下省略
~~~

このように、パッケージをビルドする際に参照する全ての変数の値と、それがどこで設定されているのかを出力できます  

## bitbake-getvarコマンドを使う方法

複数のパッケージについて、一つの変数の値を確認する場合、bitbake -eはあまりスマートな方法ではないかもしれません  
そのような場合、bitbake-getvarコマンドを使用します  

例によって、以下のように`ビルド環境を設定するスクリプト`を実行して、bitbake-getvarコマンドへのパスを通します  

~~~bash
$ source oe-init-build-env
~~~

次に、以下のようにbitbake-getvarコマンドを実行します  
-rオプションの引数がパッケージ名、最後のコマンド引数が対象の変数名です  

~~~bash
$ bitbake-getvar -r gcc HOMEPAGE
#
# $HOMEPAGE [3 operations]
#   set /home/yosuke/work/git/yocto-learning/poky/meta/conf/bitbake.conf:296
#     ""
#   set /home/yosuke/work/git/yocto-learning/poky/meta/conf/documentation.conf:195
#     [doc] "Website where more information about the software the recipe is building can be found."
#   set /home/yosuke/work/git/yocto-learning/poky/meta/recipes-devtools/gcc/gcc-common.inc:2
#     "http://www.gnu.org/software/gcc/"
# pre-expansion value:
#   "http://www.gnu.org/software/gcc/"
HOMEPAGE="http://www.gnu.org/software/gcc/"
~~~

変数名を--valueオプションの引数として渡すと、以下のように単純に変数の値だけを出力してくれます  

~~~bash
$ bitbake-getvar -r gcc --value HOMEPAGE
http://www.gnu.org/software/gcc/
~~~

また、以下のように各変数の説明文(docフラグ)を取得することもできます  

~~~bash
$ bitbake-getvar -r gcc -f doc --value HOMEPAGE
Website where more information about the software the recipe is building can be found.
$ bitbake-getvar -r gcc -f doc --value SRC_URI
The list of source files - local or remote. This variable tells the OpenEmbedded build system what bits to pull in for the build and how to pull them in.
~~~