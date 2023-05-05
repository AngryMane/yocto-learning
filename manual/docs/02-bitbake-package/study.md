
## パッケージとは

先に紹介したとおり、bitbakeコマンドは引数で指定された対象をビルドします。 例えばpython3の場合、↓のようにビルドできます  

~~~bash
$ bitbake python3
~~~

このように bitbakeコマンドがビルド対象とするもの を <span style="color: red; ">**パッケージ**</span>  と呼びます。上の図の場合、`python3`がパッケージですね  
パッケージにはサンプルのカスタマイズしたlinux OSのRuntimeや、そのRuntime用のSDKも存在します。 

~~~bash
$ bitbake core-image-minimal
$ bitbake meta-toolchain
~~~


## bitbakeコマンド
### パッケージ間の依存関係

あるパッケージをビルドする際、別のソフトウェアを必要とすることがあります  
例えば、python3(cPython3)をビルドする場合、以下のようなソフトウェアが必要です  

* ビルドのtoolchain  
    python3(cPython3)はCで開発されているためビルドにCコンパイラなどのtoolchainが必要です  
* 外部のライブラリ等  
    python3はsqlite3などの外部ライブラリを静的リンクしている(と思う)ので、ビルドにはこれらのライブラリが必要です  

これらはhostにinstall済みのものを使用することもできますが、ビルドする環境によって結果が異なってしまいます  
この問題を解決するために、**あるパッケージをビルドする時、ビルドに必要なソフトウェアのパッケージをbitbakeが検出してビルドしてくれます**  

つまり、以下の図のようにあるパッケージをbitbakeでビルドすると必要なパッケージが芋づる式にビルドされます  

![](./images/build-depends.drawio.svg)

!!! WARNING

    依存関係にはビルド時の依存関係と実行時の依存関係があるのですが、ここではあまり詳しく説明しません  

