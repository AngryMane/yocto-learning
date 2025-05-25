---
title: 新しくレシピを作成する
---

## 先に読むべきページ

* [Yocto](../component/01-yocto.md)
* [レシピファイルとincファイル](../study.md)

## 追加先のレイヤを決める

レシピはレイヤに所属する必要があります  
レシピを追加するときは、まずどのレイヤにレシピを追加するかを決定してください  

## レシピを作成する 

レシピを0から新しく作成するのは手間だと

```
$ devtool add bbc https://github.com/AngryMane/bbclient -B main
```