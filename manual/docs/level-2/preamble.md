---
title: レシピファイルとincファイル
---

## このページのスコープ

Level 1では、yoctoについて下図のように理解しました  
特に、bitbakeコマンドの 対象のパッケージ名 以外のinputを`設定ファイル`としてざっくり理解しました  


![](../level-1/images/os-build.drawio.svg)

![](../level-1/images/package-build.drawio.svg)

このページでは、この`設定ファイル`をもう少し詳細に分解し、その一部であればレシピファイルを学習します  

## 設定ファイル

#### 構成要素

設定ファイルは**主に**以下の要素で構成されます。現時点で各要素のリンク先を確認する必要はありません  

* レイヤ  
* レシピファイル  
* コンフィグファイル  

#### ディレクトリ構成

[yocto/poky/bibbakeのページ](../level-1/preamble.md)で勉強したのディレクトリ構成は以下の通りです  

![](../level-1/images/poky_directory.drawio.svg)

</br>

これに`レイヤ`と`レシピファイル`をLevel 1のディレクトリ構成にあてはめます  
(`コンフィグファイル`は今知る必要がないためこのページでは省略します)  

まず、先の図に`レイヤ`をあてはめます  

![](./images/poky_directory_rough.drawio.svg)

`other directories` として大雑把にくくっていたディレクトリがレイヤの集合であることがわかりました   
`レイヤ`の中に`レシピファイル`が存在するので、次はレイヤを分解してみましょう  

![](./images/poky_directory.drawio.svg)

図を見れば分かりますが、**レイヤはレシピファイルをまとめて管理しているディレクトリです**  
`レシピ以外のレイヤ内のファイル`のことは一旦無視してかまいません  

#### レシピファイル

{%
   include-markdown "../glossary/recipe.md"
   start="<!--outline-start-->"
   end="<!--outline-end-->"
%}

{%
   include-markdown "../glossary/recipe.md"
   start="<!--example-start-->"
   end="<!--example-end-->"
%}

#### incファイル

{%
   include-markdown "../glossary/inc.md"
   start="<!--outline-start-->"
   end="<!--outline-end-->"
%}

詳細はincファイルのリンク先をご参照ください  
