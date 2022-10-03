---
title: 存在するパッケージ名のリストを取得する
---

!!! NOTE

    このページを読む前に、[yocto/bitbake/pokyとは](../preamble.md)を確認してください  

# 存在するパッケージのリストを取得する
bitbakeコマンドの入力にはパッケージ名がありますが、そもそもどんな名前のパッケージがあるか知らないとパッケージ名を指定できません  
そのため、このページでは実際にどのようなパッケージが存在しているのかを確認する方法を紹介します  

まずはpokyリポジトリをcloneします  
使用するブランチは[こちら](https://wiki.yoctoproject.org/wiki/Releases)から選んでください。ここでは{{YOCTO_BRANCH}}ブランチを選択しています  

~~~bash
$ git clone https://git.yoctoproject.org/git/poky -b {{YOCTO_BRANCH}}
$ cd poky
~~~


次に、`ビルド環境を設定するスクリプト`を実行します。これをやらないとbitbakeなどのコマンドが使用できないためです  

~~~bash
$ source oe-init-build-env
~~~

`ビルド環境を設定するスクリプト`を実行することで、様々なユーティリティコマンドが使用可能になります  
今回はbitbake-layersコマンドを使用します。 以下の通りコマンドを実行してください  

~~~bash
$ bitbake-layers show-recipes
NOTE: Starting bitbake server...
Loading cache: 100% |#############################################################################################################################################################################| Time: 0:00:00
Loaded 1658 entries from dependency cache.
=== Available recipes: ===
acl:
  meta                 2.3.1
acpica:
  meta                 20220331
acpid:
  meta                 2.0.33
adwaita-icon-theme:
  meta                 42.0
alsa-lib:
  meta                 1.2.7.2
alsa-plugins:
  meta                 1.2.7.1
alsa-state:
  meta                 0.2.0
alsa-tools:
# 以下省略
~~~

この出力の`acl`や`acpica`などがパッケージ名に相当します  

なお、OSイメージをビルドするパッケージの多くは、core-image...という名前です(例外あり)  
したがって、以下のようにgrepを組み合わせるとOSイメージをビルドするパッケージの大雑把なリストが得られます  

~~~bash
$ bitbake-layers show-recipes | grep core-image
core-image-base:
core-image-full-cmdline:
core-image-kernel-dev:
core-image-minimal:
core-image-minimal-dev:
core-image-minimal-initramfs:
core-image-minimal-mtdutils:
core-image-ptest-all:
core-image-ptest-fast:
core-image-rt:
core-image-rt-sdk:
core-image-sato:
core-image-sato-dev:
core-image-sato-sdk:
core-image-testcontroller:
core-image-testcontroller-initramfs:
core-image-tiny-initramfs:
core-image-weston:
core-image-weston-sdk:
core-image-x11:
~~~