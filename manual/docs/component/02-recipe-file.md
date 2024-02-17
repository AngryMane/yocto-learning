# レシピファイル

## レシピの文法

!!! warning

    レシピファイルはbitbake固有の文法で記述されていますが、現時点でドキュメントを作成できていません  
    [公式ドキュメント](https://docs.yoctoproject.org/bitbake/2.2/bitbake-user-manual/bitbake-user-manual-metadata.html)をご参照ください  

## レシピ毎に定義される主要な変数

### PN
`PN` 変数は `パッケージ` を宣言しています。 `パッケージ` の名前をbitbakeに引数として渡すことでビルドができます  

```
$ bitbake hello # これでhelloパッケージをビルドできる
```

同じレシピ内の他の記述はこのパッケージのビルド方法などのパラメータを宣言しています  

### PACKAGES
`PACKAGES` 変数は `ランタイムパッケージ` を宣言しています  
`ランタイムパッケージ`はbitbakeがインストーラ(\*.deb,\*.rpmなど)を作成する単位です  

### DEPENEDS
パッケージをビルドする際に必要になる別のパッケージを定義します  
例えば、c++で開発されたソフトウェアをビルドするためにはc++コンパイラ(gcc/clang)が必要です  
このような場合、c++コンパイラを提供するパッケージをDEPENDSに追加します  

```
DEPENDS += "clang"
```

### RDEPENDS
パッケージに含まれるソフトウェアを実行する際に必要になる **ランタイムパッケージ** を定義します  
例えば、GUIアプリをLinuxで動かすとき、WindowManager(X,westonなど)が必要になります  
このような場合、WindowManagerを提供する **ランタイムパッケージ** をRDEPENDSに追加します  

```
RDEPENDS += "weston"
```

!!! Note

    指定するのはパッケージではなく、ランタイムパッケージであることに注意してください  

### PROVIDES
今はパッケージの別名を定義する変数だと思ってください  

### RPROVIDES
今はランタイムパッケージの別名を定義する変数だと思ってください  

### SRC_URI
パッケージのソースコードを取得するURIです  
例えば、ninjaのレシピファイルには以下のような記述があります  

```
SRC_URI = "git://github.com/ninja-build/ninja.git;branch=release;protocol=https"
SRCREV = "e72d1d581c945c158ed68d9bc48911063022a2c6"
```

この場合、ninjaのソースコードをgit://github.com/ninja-build/ninja.gitから取得します  
`branch=release;protocol=https` については何となく意味が分かると思います  

### SRCREV
パッケージのソースコードのリビジョン(gitのハッシュ値)です  
例えば、ninjaのレシピファイルには以下のような記述があります  

```
SRC_URI = "git://github.com/ninja-build/ninja.git;branch=release;protocol=https"
SRCREV = "e72d1d581c945c158ed68d9bc48911063022a2c6"
```

この場合、ninjaのリポジトリのe72d1d581c945c158ed68d9bc48911063022a2c6のコミットハッシュのソースコードを使用します  

### FILES
ランタイムパッケージ毎にインストールするファイルを列挙して定義する変数です  
例えば、以下のようにランタイムパッケージ毎にインストールするファイルを分けて定義します  

```
PACKAGES =+ "runtime-package-a runtime-package-a"
FILES:runtime-package-a += "file_for_a1 file_for_a2"
FILES:runtime-package-b += "file_for_b2 file_for_b2"
```

### PACKAGECONFIG
パッケージの挙動を制御するための便利な変数です  
以下のようにcmakeのビルドパラメータ等を制御するのに使用します  

```
PACKAGECONFIG = "featureA"
PACKAGECONFIG[featureA] = "\
    --FEATURE_A=ON, \             # PACKAGECONFIGがfeatureAを含むとき、ビルドに使用するパラメータ(cmakeなどに渡します)
    --FEATURE_A=OFF, \            # PACKAGECONFIGがfeatureAが含まないとき、ビルドに使用するパラメータ(cmakeなどに渡します)
    build-deps-for-f1, \          # PACKAGECONFIGがfeatureAを含むとき、DEPENDSに追加するパッケージ名
    runtime-deps-for-f1, \        # PACKAGECONFIGがfeatureAを含むとき、RDEPENDSに追加するランタイムパッケージ名
    runtime-recommends-for-f1, \  # PACKAGECONFIGがfeatureAを含むとき、RRECOMMENDSに追加するランタイムパッケージパッケージ名
    featureB"                     # PACKAGECONFIGがfeatureAを含むとき、PACKAGECONFIGが含んでいてはいけないfeature
PACKAGECONFIG[featureB] = "\
   # 省略
"
```

### PV
パッケージのバージョン(Package Version)の略語です  
文字列であれば何でも入力できると思いますが、例えば以下のような例が多いです  

```
PV = "1.0.0"
```

ソースコードのバージョンではないことに注意してください  

### PR
パッケージのリビジョン(Package Revision)の略語です  
PVは変えたくないけどパッケージの内容は変化しているというような場合にインクリメントして使用します  
文字列であれば何でも入力できると思いますが、例えば以下のような例が多いです  

```
PR = "r1"
```

### PE
パッケージのエポック(Package Epoch)の略語です  
PV/PRは変えたくないけどパッケージの内容は変化しているというような場合にインクリメントして使用します。整数値を入力します  

```
PR = "2"
```

## レシピで宣言する主要な関数

### do_fetch

### do_patch

### do_configure

### do_build

### do_package

### do_install


## パッケージ間の依存関係

あるパッケージをビルドする際、別のソフトウェアを必要とすることがあります  
例えば、python3(cPython3)をビルドする場合、以下のようなソフトウェアが必要です  

* ビルドのtoolchain  
    python3(cPython3)はCで開発されているためビルドにCコンパイラなどのtoolchainが必要です  
* 外部のライブラリ等  
    python3はsqlite3などの外部ライブラリを静的リンクしている(と思う)ので、ビルドにはこれらのライブラリが必要です  

既にPCにインストール済みのこれらのパッケージを使用すると、ビルド環境によってビルド結果が異なってしまいます  
この問題を解決するために、**あるパッケージをビルドする時、ビルドに必要なソフトウェアのパッケージをbitbakeが検出して自動的にビルドします**  

つまり、以下の図のようにあるパッケージをbitbakeでビルドすると必要なパッケージが芋づる式にビルドされます  
このため、ちょっとしたソフトウェアであってもyoctoでビルドすると時間がかかることがあります  

![](./images/build-depends.drawio.svg)

!!! WARNING

    依存関係にはビルド時の依存関係と実行時の依存関係があるのですが、ここでは詳しく説明しません  


