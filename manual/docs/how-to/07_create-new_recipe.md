---
title: 新しくレシピを作成する
---

## 先に読むべきページ

* [Yocto](../component/01-yocto.md)
* [レシピファイルとincファイル](../study.md)

## レシピを作成する 

レシピを0から新しく作成するのはなれないと面倒です  
yoctoのpokyリポジトリにはdevtoolというコマンドが存在し、以下の方法でレシピのテンプレートを生成できます  

```
$ source oe-init-build-env

# devtool add ${レシピ名}       ${リポジトリのURI}                             -B ${ブランチ名}
$ devtool add vss-signal-client https://github.com/AngryMane/vss-signal-client -B main 
NOTE: Starting bitbake server...
...
INFO: Recipe /home/yosuke/work/git/yocto-learning/poky/build/workspace/recipes/vss-signal-client/vss-signal-client_git.bb has been automatically created; further editing may be required to make it fully functional
```

追加したレシピは、bitbakeコマンドでビルドすることができます  

```
$ bitbake vss-signal-client
```

## レイヤに移動する

生成したレシピは/poky/build/workspaceのレイヤに存在しますが、これは作業用の一時的なレイヤです  
生成したレシピを今後も使いまわすときは、どこかのレイヤにレシピを追加してください  

たとえばmeta-pokyにレシピを追加する場合は、以下の手順になります  

まず、レイヤ内のレシピファイルの検索パス(BBFILES)を確認します  

```
$ cat meta-poky/conf/layer.conf
...
BBFILES += "${LAYERDIR}/recipes-*/*/*.bb \
            ${LAYERDIR}/recipes-*/*/*.bbappend"

...
```

meta-pokyレイヤ内の適当なディレクトリ内に配置しておけば、自動的に認識してくれることがわかります  
わかりやすい名前のディレクトリを作成し、そこに生成したレシピファイルを配置してください  

```
$ mkdir -p meta-poky/my-recipes/vss-signal-client
$ cp /home/yosuke/work/git/yocto-learning/poky/build/workspace/recipes/vss-signal-client/vss-signal-client_git.bb meta-poky/my-recipes/vss-signal-client
```

これでbitbakeが生成したレシピファイルを認識します  
