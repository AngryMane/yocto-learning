---
title: パッケージ名を変更する
---


!!! NOTE

    このページを読む前に、[レシピファイルとincファイル](../preamble.md)を確認してください  

!!! NOTE

    実はパッケージとレシピは1:1の関係ではありませんが、ここでは説明しません  

# パッケージ名を変更する

あるレシピが提供するパッケージの名前はPN変数として管理されます  
PN変数は以下のように決定されます。  詳細は [リンク1](https://git.yoctoproject.org/poky/tree/bitbake/lib/bb/cache.py?h=dunfell-23.0.20#n88) と [リンク2](https://git.yoctoproject.org/poky/tree/bitbake/lib/bb/parse/\__init__.py?h=dunfell-23.0.20#n137) をご参照ください  

1. レシピがPN変数を定義している場合、定義している値をそのまま使う
1. レシピがPN変数を定義していない場合、レシピファイルの名前の`_`より前の部分をPN変数とする
    * 例えばレシピファイルの名前が`sample_1.0.0.bb`の場合、パッケージ名は`sample`となる

したがって、レシピが提供するパッケージ名は以下の2つの方法で変更できます  

* レシピファイル名を変更する
* レシピで定義しているPN変数を変更する

#### レシピファイル名を変更する

PN変数が定義されていない場合、レシピファイル名を変更してパッケージ名を変更することができます  
例えば、alsa-libパッケージを提供しているレシピファイルの名前を変更してみます  

~~~bash
$ mv ./meta/recipes-multimedia/alsa/alsa-lib_1.2.7.2.bb ./meta/recipes-multimedia/alsa/alsa-lib-renamed_1.2.7.2.bb
~~~

変更したレシピファイルはalsa-lib-renamedパッケージを提供するため、以下のコマンドでalsa-lib-renamedパッケージをビルドできます  

~~~bash
$ source oe-init-build-env
$ bitbake alsa-lib-renamed
# 以下省略
~~~

!!! WARNING

    ただし、alsa-libパッケージはパッケージ名を変更するとビルドに失敗します  
    SRC_URIなども合わせて修正する必要があるためですが、alsa-libパッケージ特有の事象なのでここでは詳しく説明しません  

#### レシピで定義しているPN変数を変更する

レシピで定義しているPN変数を新規に追加するかあるいは変更することでパッケージ名を変更することができます  
例えば、alsa-libパッケージのレシピファイルにPN変数を追加してみます  

~~~bash
$ vi ./meta/recipes-multimedia/alsa/alsa-lib_1.2.7.2.bb
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
PN = "alsa-lib-renamed"  # 追加した行
# 以下省略
~~~

変更したレシピファイルはalsa-lib-renamedパッケージを提供するため、以下のコマンドでalsa-lib-renamedパッケージをビルドできます

~~~bash
$ source oe-init-build-env
$ bitbake alsa-lib-renamed
# 以下省略
~~~

!!! WARNING

    ただし、alsa-libパッケージはパッケージ名を変更するとビルドに失敗します  
    SRC_URIなども合わせて修正する必要があるためですが、alsa-libパッケージ特有の事象なのでここでは詳しく説明しません  