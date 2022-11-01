---
title: yocto/bitbake/poky
---

# yocto/bitbake/poky

## Yoctoとは
Yoctoプロジェクトは、`カスタマイズしたlinuxOSをビルドする`ための開発環境です  
`カスタマイズしたlinuxOSをビルドする`作業を少し細かく分解すると、以下の図のようになります   
(あくまで簡単なイメージです)  

![](images/os-build.drawio.svg)  

上図の通り、基本的にはpokyリポジトリに存在する`bitbake`というコマンドを使用することになります  
このページでは上図の中の以下の2つの概要を説明します  

* パッケージ名  
* pokyリポジトリ(リファレンス実装)  


## パッケージ名

#### パッケージとは
上図のとおり、bitbakeコマンドは引数で指定された対象をビルドします。↓のようなイメージですね   

~~~bash
$ bitbake python3
~~~

このbitbakeコマンドがビルド対象とするものを**パッケージ**と呼びます  
例えば、core-image-minimal(yoctoのサンプルOSの一つ)、python3やbusyboxはパッケージです  
したがって、**ビルド環境の設定をすれば**以下のコマンドでビルドできます   

~~~bash
$ bitbake core-image-minimal # サンプルOS(core-image-minimal)をビルドする
$ bitbake python3            # python3をビルドする
$ bitbake busybox            # busyboxをビルドする
~~~

<!--
ただし、OSイメージのパッケージをビルドした場合と、普通のソフトウェアのパッケージをビルドした場合ではbitbakeコマンドの出力が異なります  
OSイメージをビルドする場合、先に紹介した図になります  

![](images/os-build.drawio.svg)  

普通のソフトウェアのパッケージをビルドした場合、以下のようにインストーラが出力されます  

![](images/package-build.drawio.svg)  

-->

#### パッケージ間の依存関係

あるパッケージをビルドする際、別のソフトウェアを必要とすることがあります  
例えば、python3(cPython3)をビルドする場合を考えてみます。この場合、必要なソフトウェアは概ね以下の2種類に分類できます  

* ビルドするために必要なソフトウェア  
    python3はCで開発されているためビルド(とくにコンパイル)にgccなどのCコンパイラが必要です  
* ビルドする際に外部のライブラリ等として必要なソフトウェア  
    python3はsqlite3などの外部ライブラリを静的リンクしている(と思う)ので、ビルドにはこれらのライブラリが必要です  

これらはhostにinstall済みのものを使用することもできますが、ビルドする環境によって結果が異なってしまいます  
この問題を解決するために、**あるパッケージをビルドする時、ビルドに必要なソフトウェアを提供するパッケージをbitbakeが検出してビルドしてくれます**  

つまり、以下の図のようにあるパッケージをbitbakeでビルドすると必要なパッケージが芋づる式にビルドされます  

![](./images/build-depends.drawio.svg)

!!! WARNING

    依存関係にはビルド時の依存関係と実行時の依存関係があるのですが、ここではあまり詳しく説明しません  


## pokyリポジトリ(リファレンス実装)
#### pokyリポジトリとは
pokyは以下の3つをまとめたリポジトリです  

* `設定ファイル`のリファレンス実装
* `ビルド環境を設定するスクリプト`
* `bitbakeコマンド`

特に `設定ファイル` は複雑なため、大抵はpokyリポジトリをカスタマイズして`カスタマイズしたlinuxOS`をビルドします  

![](images/inout_poky.drawio.svg)  

#### pokyのディレクトリ構成
現時点で必要な粒度でpokyのディレクトリ構成を確認します。  

![](images/poky_directory.drawio.svg)

!!! NOTE

    linuxカーネルやrootfsのファイル名は設定ファイルによってかなり変化します  

実際にpokyのディレクトリ構成を確認してみましょう  
使用するブランチは[こちら](https://wiki.yoctoproject.org/wiki/Releases)から選んでください。ここでは{{YOCTO_BRANCH}}ブランチを選択しています  

~~~bash
$ git clone https://git.yoctoproject.org/git/poky -b {{YOCTO_BRANCH}}
$ tree -L 1
.
├── LICENSE
├── LICENSE.GPL-2.0-only
├── LICENSE.MIT
├── MAINTAINERS.md
├── MEMORIAM
├── Makefile
├── README.OE-Core.md
├── README.hardware.md -> meta-yocto-bsp/README.hardware.md
├── README.md -> README.poky.md
├── README.poky.md -> meta-poky/README.poky.md
├── README.qemu.md
├── bitbake                                                  <- bitbakeコマンド(を提供しているディレクトリ)
├── build                                                    <- この中にlinuxカーネルとrootfsがある
├── contrib                                                  ┐
├── documentation                                            │
├── meta                                                     │
├── meta-poky                                                ├  設定ファイル
├── meta-selftest                                            │
├── meta-skeleton                                            │
├── meta-yocto-bsp                                           ┘
├── oe-init-build-env                                        <- ビルド環境を設定するスクリプト
└── scripts

10 directories, 12 files
~~~

ライセンスファイルやシンボリックリンク、.git等不要ファイルを削除して整理します  

~~~bash
$ tree -L 1
.
├── bitbake                                                  <- bitbakeコマンド(を提供しているディレクトリ)
├── build                                                    <- この中にlinuxカーネルとrootfsがある
├── contrib                                                  ┐
├── meta                                                     │
├── meta-poky                                                ├  設定ファイル
├── meta-selftest                                            │
├── meta-skeleton                                            │
├── meta-yocto-bsp                                           ┘
├── oe-init-build-env                                        <- ビルド環境を設定するスクリプト
└── scripts
~~~

先に示した図の通りのディレクトリ構造になっていることが分かります  
