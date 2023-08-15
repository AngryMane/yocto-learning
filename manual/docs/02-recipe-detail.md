# レシピの詳細

## レシピの文法

!!! warning

    レシピファイルはbitbake固有の文法で記述されていますが、現時点でドキュメントを作成できていません  
    [公式ドキュメント](https://docs.yoctoproject.org/bitbake/2.2/bitbake-user-manual/bitbake-user-manual-metadata.html)をご参照ください  

## レシピで宣言する主要な変数

### PN

### PACKAGES

### DEPENEDS

### RDEPENDS

### PROVIDES

### RPROVIDES

### SRC_URI

### SRCREV

### FILES

## PN/PROVIDES/PACKAGES/RPROVIDESの関係

## レシピで宣言する主要な関数

## パッケージ間の依存関係

あるパッケージをビルドする際、別のソフトウェアを必要とすることがあります  
例えば、python3(cPython3)をビルドする場合、以下のようなソフトウェアが必要です  

* ビルドのtoolchain  
    python3(cPython3)はCで開発されているためビルドにCコンパイラなどのtoolchainが必要です  
* 外部のライブラリ等  
    python3はsqlite3などの外部ライブラリを静的リンクしている(と思う)ので、ビルドにはこれらのライブラリが必要です  

install済みのこれらのパッケージを使用すると、ビルド環境によってビルド結果が異なってしまいます  
この問題を解決するために、**あるパッケージをビルドする時、ビルドに必要なソフトウェアのパッケージをbitbakeが検出して自動的にビルドします**  

つまり、以下の図のようにあるパッケージをbitbakeでビルドすると必要なパッケージが芋づる式にビルドされます  
このため、ちょっとしたソフトウェアであってもyoctoでビルドすると時間がかかることがあります  

![](./images/build-depends.drawio.svg)

!!! WARNING

    依存関係にはビルド時の依存関係と実行時の依存関係があるのですが、ここでは詳しく説明しません  


