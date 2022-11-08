---
title: レシピのバージョンを変更する
---

## 先に読むべきページ
* [レシピファイルとincファイル](../study.md)


## レシピのバージョンを変更する

レシピのバージョン情報はPE/PV/PRという変数で管理されています。 公式ドキュメントからそれぞれの説明を以下に引用します   
詳細は [こちら](https://docs.yoctoproject.org/ref-manual/variables.html?highlight=pn#term-P) をご参照ください  

##### PE
PVが後方互換性なく変更された場合に、アップグレードを可能にするために使用されます。デフォルトではこの変数は設定されていません  

##### PV
レシピのバージョン。通常レシピのファイル名から抽出されます  
例えば、レシピの名前が expat_2.0.1.bb であれば、PV のデフォルト値は "2.0.1" となります  
PV は、ソースコードリポジトリから不安定な(すなわち開発)バージョンを構築していない限り、一般的にレシピ内で上書きされることはないでしょう  

##### PR
レシピのリビジョン。この変数のデフォルト値は "r0" です  
レシピの後続のリビジョンは慣習的に "r1"、"r2 "などの値を持っています。PVが増加すると、PRは慣習的に "r0 "にリセットされます。  

</br>

### ファイル名を変更することでPVを変更する

PV変数が定義されていない場合、レシピファイル名を変更してPV(レシピのバージョン)を変更することができます  
例えば、alsa-libパッケージを提供しているレシピファイルの名前を変更してみます  

~~~bash
$ mv ./meta/recipes-multimedia/alsa/alsa-lib_1.2.7.2.bb ./meta/recipes-multimedia/alsa/alsa-lib_2.0.0.0.bb
~~~

変更したレシピファイルはalsa-libパッケージのバージョン2.0.0.0を提供します  

~~~bash
$ source oe-init-build-env
$ bitbake-getvar -r alsa-lib --value PV
2.0.0.0
~~~

!!! WARNING

    ただし、alsa-libパッケージはパッケージ名を変更するとビルドに失敗します  
    SRC_URIなども合わせて修正する必要があるためですが、alsa-libパッケージ特有の事象なのでここでは詳しく説明しません  

</br>

### レシピで定義しているPE/PV/PR変数を変更する

通常のレシピの変数を変更するのと同様に変更すればOKです  
例えば、alsa-libパッケージのレシピファイルでPE/PV/PR変数を定義してみます  

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
PE = "2"        # 追加した行
PV = "2.0.0.0"  # 追加した行
PR = "r2"       # 追加した行
# 以下省略
~~~

変更したレシピファイルはalsa-libパッケージはPE=2, PV(バージョン)=2.0.0.0, PR(リビジョン)=r2を提供します  

~~~bash
$ source oe-init-build-env
$ bitbake-getvar -r alsa-lib --valu PE
2
$ bitbake-getvar -r alsa-lib --valu PV
2.0.0.0
$ bitbake-getvar -r alsa-lib --valu PR
r2
~~~