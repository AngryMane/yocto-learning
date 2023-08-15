# yoctoの概要

!!! note

    このドキュメントはコードブロックのコマンドを実行しながら読んでください  
    後述するレイヤやレシピ、プロバイダ、パッケージを理解するには自分で操作することが確実だからです  

## Yoctoとは
Yoctoプロジェクトは 

* <span style="color:red">**特定の実行環境(開発用ボードなど)**</span> 向けに
* <span style="color:red">**カスタマイズしたlinuxディストリビューション**</span> を

ビルドするための開発環境です。例えば以下のようなことができます

* Raspberry pi向けに`カスタマイズしたlinuxディストリビューション`をビルドし、Raspberry piボード上で動かす
* QEMU向けに`カスタマイズしたlinuxディストリビューション` をビルドし、QEMUで動かす
* Docker向けに`カスタマイズしたlinuxディストリビューション`をビルドし、Dockerコンテナとして動かす
* pythonを実行できる必要最小限のソフトウェアのみインストールしたlinuxディストリビューションをビルドする  
* ↑のOS上で動かすソフトウェアをビルドするためのSDKをビルドする  
* カスタマイズしたlinuxディストリビューションやそのSDKをビルドする環境を配布する

</br>

## pokyとは
pokyはyoctoの実装リポジトリです。 pokyの主な構成要素はレイヤとレシピです  
pokyとレイヤ、レシピは以下のような関係になっています  

![](./images/poky-layer-recipe.drawio.svg)

レイヤとレシピはこのページの後半で説明します  

</br>

## pokyのディレクトリ構成
pokyとレイヤ、レシピの関係を念頭に、実際にpokyディレクトリに何が入っているのかを観察します  
使用するブランチは[こちら](https://wiki.yoctoproject.org/wiki/Releases)から選んでください。ここでは{{YOCTO_BRANCH}}ブランチを選択しています  

~~~bash
$ git clone https://git.yoctoproject.org/git/poky -b {{YOCTO_BRANCH}}
$ tree -L 1
.
├── LICENSE                                                  ┐
├── LICENSE.GPL-2.0-only                                     |
├── LICENSE.MIT                                              |
├── MAINTAINERS.md                                           |
├── MEMORIAM                                                 ├  ライセンスファイルなど
├── Makefile                                                 |
├── README.OE-Core.md                                        |
├── README.hardware.md -> meta-yocto-bsp/README.hardware.md  |
├── README.md -> README.poky.md                              |
├── README.poky.md -> meta-poky/README.poky.md               |
├── contrib                                                  |
├── README.qemu.md                                           ┘
├── documentation                                            <- ドキュメント。基本オンラインドキュメントと同じだが、+αの記載もある
├── bitbake                                                  <- bitbakeというビルドコマンドを実装しているディレクトリ
├── meta                                                     ┐
├── meta-poky                                                |
├── meta-selftest                                            ├  レイヤ
├── meta-skeleton                                            │
├── meta-yocto-bsp                                           ┘
├── oe-init-build-env                                        <- ビルド環境を設定するスクリプト
└── scripts                                                  <- その他の便利に使えるスクリプト

10 directories, 12 files
~~~

不要なファイルを削除して整理すると以下の3種類しかないことが分かります  

~~~bash
$ tree -L 1
.
├── bitbake            <- bitbakeというビルドコマンドを実装しているディレクトリ
├── meta               ┐
├── meta-poky          |
├── meta-selftest      ├  レイヤ
├── meta-skeleton      │
├── meta-yocto-bsp     ┘
└── oe-init-build-env  <- ビルド環境を設定するスクリプト
~~~

それぞれ概要を見ていきましょう

</br>

## oe-init-build-env
yoctoをビルドする環境を設定するスクリプトです。主に以下の処理を実施します  

* 必要な環境変数を設定する
* ビルドディレクトリを作成する
* カレントディレクトリをビルドディレクトリに移動する
* bitbakeコマンドやその他便利なスクリプトにパスを通す

</br>

## bitbake
yocto固有のビルドコマンド(bitbake)を実装しているディレクトリです  
bitbakeコマンドはcmakeやninjaとよく似た機能を提供しています。例えば以下のように使用します  

```bash
$ source oe-init-build-env  # oe-init-build-envでビルド環境をセットアップする
$ bitbake python3           # bitbakeコマンドでビルドする
```

</br>

## レイヤ
まずはレイヤの中にどのようなファイルが含まれるのかを観察します  

```
$ cd meta-poky
$ tree 
.
├── README.poky.md         <- ただのREADME
├── classes                <- 今は無視してください
├── conf                   <- 今は無視してください
└── recipes-core
    ├── busybox            ┐
    ├── psplash            ├  ソフトウェアの名前のディレクトリ。レシピファイルが入っている
    └── tiny-init          ┘
        ├── files          <- 今は無視してください
        └── tiny-init.bb   <- レシピファイル。ビルドのパラメータ(リポジトリのURIなど)を定義している

12 directories, 23 files
```


`*.bb` というファイルがレシピと呼ばれるファイルです  
現時点では `レイヤ=レシピを格納するディレクトリ` と思っておいてください  

</br>

## レシピ
実際にレシピファイルの中身を見てみましょう。 別のページで説明するため、ここでは文法を気にしなくてもよいです  
(理解しやすくするため、一部ファイルを編集しています)  

```
$ cd meta-skeleton/recipes-skeleton/hello-single
$ cat hello_1.0.bb
DESCRIPTION = "Simple helloworld application"
LICENSE = "MIT"

PN = "hello"
PACKAGES:${PN} = "hello-package hello-package-doc"

SRC_URI = " \
    git://github.com/ab/cd.git;branch=main \
    file://helloworld.c \
"

do_compile() {
        ${CC} ${LDFLAGS} helloworld.c -o helloworld
}

do_install() {
        install -d ${D}${bindir}
        install -m 0755 helloworld ${D}${bindir}
}
```

ここでは `PN` 変数と`PACKAGES` 変数が重要です。 
 
#### PN 

`PN` 変数は `プロバイダ` を宣言しています。この `プロバイダ` がbitbakeのビルド対象です  
同じレシピ内の他の記述はこのプロバイダのパラメータを宣言しています  

#### PACKAGES 

`PACKAGES` 変数はパッケージを宣言しています  
パッケージはbitbakeがインストーラ(\*.deb,\*.rpmなど)を作成する単位です  
このパッケージはプロバイダ毎に指定します。1つのプロバイダが複数のパッケージを持つこともあります  

</br>

以上のことを踏まえてレシピとプロバイダ、パッケージの関係をまとめると以下のようになります  

![](./images/recipe-package-bitbake.drawio.svg)

!!! warning

    プロバイダという言葉を定義していますが、yoctoのコミュニティではこれもパッケージと呼びます  
    しかしながら、別の意味(PACKAGE変数)でパッケージという言葉を使用しているため、区別のためにプロバイダという言葉を使用します  
    複雑なことをしない限り、実務上区別が必要になることが少ないためこのような状態になっているものと思われます    

!!! warning

    PN変数は省略することができます  
    省略した場合、レシピファイル名から一定のルールにしたがってPN変数が設定されます  

!!! note

    1つのレシピは複数のプロバイダを宣言することが可能ですが、混乱を招くためここでは説明しません  




</br>

## まとめ

最後にpoky、レイヤ、レシピとパッケージの関係を一枚の絵にまとめてみます  
(これはディレクトリ構成ではないことに注意してください)  

![](./images/poky-layer-recipe-package.drawio.svg)

