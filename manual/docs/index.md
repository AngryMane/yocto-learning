---
title: はじめに
---

# はじめに

## このドキュメントの目的
このドキュメントの目的はYoctoの使い方を可能な限り簡単に説明することです  

## 対象読者

以下の全ての条件を満たす読者を想定しています  

* yoctoでやりたいことがあるが、yoctoのことをあまり知らない
* Linuxでのソフトウェア開発に慣れており、シェルでコマンドを実行することに抵抗がない

---

<a id="目次"></a>
# 目次


# Software design

Software designはYoctoの外形的な設計を説明します  
このページはHowtoを説明しませんが、Howtoを理解するのに必要です  

最初はYoctoのページのみを参照し、その後必要に応じて他のページを参照してください  

* [Yocto](./software-design/01-yocto.md)
* [レシピファイル](./software-design/02-recipe-file.md)
* [グローバルなコンフィグファイル](./software-design/03-global-configfile.md)
* [レイヤ固有のコンフィグファイル](./software-design/04-layer-configfile.md)

# How-to 

How-toはユーザーがやりたいことの具体的な手順を説明します  
ここではYoctoの概念等の説明はしないため、わからないことがあればComponentsを参照してください  

* [サンプルOSをビルドする](./how-to/01-build-sample-os.md)
* [存在するパッケージ名のリストを取得する](./how-to/02-get-pakcage-list.md)
* [その他(OS以外)のパッケージをビルドする](./how-to/03-build-package.md)
* [パッケージのパラメータを確認する](./how-to/04_check-package-params.md)
* [パッケージ名を変更する](./how-to/05_change-package-name.md)
* [レシピのバージョンを変更する](./how-to/06_change-recipe-version.md)


</br>
</br>


# まだ記述できていないHow to
* nativeパッケージの話
* プロジェクト全体の設定を変更する
    * 新しいパッケージを作成し、インストール対象に追加する
    * 既存のパッケージをインストール対象に追加する
    * レイヤをビルド対象に追加する
    * レイヤをビルド対象から削除する
    * ダウンロードキャッシュやsstateキャッシュのパスを変更する
* 既存のパッケージのレシピファイルにパッチ当てをする
    * ソースコードのキャッシュディレクトリを変更する
    * C/C++のコンパイラやリンカなどのコマンドと引数を変更する
* 分散ビルドする
* SDKをビルドする
* SDKを使ってソフトウェアを開発する
* eSDKをビルドする
* eSDKを使ってソフトウェアを開発する
* パッケージグループを追加する
* 新しいイメージパッケージを作成する
* 一度ビルドしたパッケージをレシピのパースなしで再度ビルドする
* 新しいタスクを定義する
* パッケージのライセンスを確認する
* 複数機種向けのビルドを設定する(multi-config)
* 一つのパッケージが複数のレシピファイルを持つとき、選択したレシピファイルを使用する(PREFFRED_PROVIDER/PREFFERED_VERSION)
* 新しいカーネルモジュールを作成する(modules.bbclass)


# トラブルシューティング
* パッケージのfetchに失敗する
* パッケージのcompileに失敗する
* パッケージのdo_package_qaに失敗する