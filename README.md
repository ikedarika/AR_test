# ARに関しての概要
## AR開発の種類
- WebAR<br>
　→ カメラがあるデバイスでサイトにアクセスしてWebサイト上で動作する
- ARアプリ<br>
　→ カメラがあるデバイスにAPPをDLしてもらってAPP上で動作させる
<br>
### WebARの開発
<p>
手軽に開発をするのなら、Webサイト上で動作するWebARが導入しやすい。<br>
開発がしやすく、AR.jsのプラグインファイルを読み込んで動かしたいARの内容を記載するだけですぐに作成できる。<br>
手軽な反面、できる機能はある程度限られてしまうし、マーカーなどのARを認識させるファイルを作成したり<br>
実際の画像自体をマーカーにした場合の精度など品質が少し劣ってしまう。<br>
単純にARで閲覧以外の場合はAPP開発が望ましい。
</p>
<br>
<br>

# WebARサイトの作成
自ら仕組みを作成する場合、商用利用可能なオープンソースの<br>
AR.jsを使用すると手軽に作成できる。

## 参考サイト
- [SEGA技術部でのAR開発に関する記事](https://segaxd.co.jp/blog/c7998bd6e8077395c31ed9925c3c5f740e935a38.html)
- [AR名刺の作成方法](https://qiita.com/MasakiOhno/items/c4c8d0a90ae0383a2ae3)
- [AR.jsのGithub](https://github.com/AR-js-org/AR.js)
- [ARマーカー作成サイト](https://jeromeetienne.github.io/AR.js/three.js/examples/marker-training/examples/generator.html)

## ARサイトを作成する際に必要なファイル
- HTML(サイト本体)<br>
サイトの本体になるファイルに、AR.jsのファイルを（scriptとして）取り込むだけで<br>
AR機能をサイト上で使用することができる<br>
- 3Dモデルファイル<br>
objファイルとmtlファイルをサーバーに配置してパスをつなげると表示ができる
- ARマーカーファイル<br>
- [ARマーカー作成サイト](https://jeromeetienne.github.io/AR.js/three.js/examples/marker-training/examples/generator.html)などでARマーカー用に画像ファイルを作成する(.patt)ARマーカーの場合は黒い枠がついた状態になる
<br>
## ファイルの用意
1. ARマーカーファイル
[ARマーカー作成サイト](https://jeromeetienne.github.io/AR.js/three.js/examples/marker-training/examples/generator.html)で拡張子が「.patt」になったARマーカーの画像を作成することができる。<br>
ARマーカーは、余白と色がある部分の比率で対象の位置や方向を認識するため、白黒の物がわかりやすく精度がよい

2. 3Dモデルファイル<br>
[blender](https://blender.jp/)などの3Dモデル作成ソフトでobjファイルとして3Dモデルをエクスポートすれば<br>
objファイルとmtlファイルが生成される。
<br>
3. PGファイル<br>
下記のプログラム内容が記載されていうHTMLファイル
<br>
<br>
## プログラム(index.html)
1. サイト本体のHTMLファイルの枠組み作成
    ```html
    <!DOCTYPE html>
    <html>
        <head>
            <meta charset="utf-8">
            <meta name="viewport" content="width=device-width,intial-scale=1">
            <title>WebAR</title>
        </head>
        <style></style>
        <body>
        </body>
    </html>
    ```
    枠組みとしては単純なHtmlファイルで問題ない。

2. AR.jsファイルを記載を追加（AR.jsファイルのリンクを追加）
    ```html
    <!-- ↓↓↓↓↓↓ AR.jsファイルのリンクを追加　↓↓↓↓↓↓ -->
    <!-- AFrameライブラリ読み込み -->
    <script src="https://aframe.io/releases/0.8.0/aframe.min.js"></script>
    <!-- AR.jsライブラリ読み込み -->
    <script src="https://jeromeetienne.github.io/AR.js/aframe/build/aframe-ar.js"></script>
    <!-- shaderライブラリ読み込み -->
    <script src="https://rawgit.com/mayognaise/aframe-gif-shader/master/dist/aframe-gif-shader.min.js"></script>
    <script src="shader-grid-glitch.js"></script>
         <!-- ↑↑↑↑↑↑ AR.jsファイルのリンクを追加　↑↑↑↑↑↑ -->
    ```

3. AR用のフォーム外枠部分追加
    ```html
    <a-scene embedded arjs="カメラや縦横の動作取得などの設定">
    <!--AR機能の中身-->
    </a-scene>
    ```
    AR機能はa-sceneというタグで囲まれた範囲内に記載することで動作します。

4. 3Dモデルの配置
    ```html
    <a-assets>
        <!-- 3Dモデル  -->
        <a-asset-item id="3DModelのID" src="objファイルパス"></a-asset-item>
        <!-- マテリアル -->
        <a-asset-item id="mtlファイルのID" src="mtlファイルパス"></a-asset-item>
    </a-assets>
    ```
    3Dモデルのobjファイルとmtlファイルを指定して項目のidを設定する。<br>
    動作させるマーカー内の記載部分で上記idからモデルを呼び出して動作させる。

5. マーカーの指定
    ```html
    <a-marker preset="custom" type="pattern" url="ARマーカーファイルパス">
        <!--ARモデルなどの内容-->
    </a-marker>
    ```
    マーカーは自分で作成したARマーカーの場合は上記のように対象のファイルパスを指定する必要がある。<br>
    デフォルトのマーカーで作成する場合は下記のような記載でも可能
    ```html
    <a-marker preset="hiro">
    </a-marker>
    ```

6. モデルの動き(アニメーション)を指定する<br>
    <br>
    モデルを上下にふわふわ動かす
    ```html
    <a-animation  
        attribute="position"
        dur="3000"
        to="0 0 0.5"
        repeat="indefinite"
        direction="alternate"
    >
    </a-animation>
    ```
    <br>
    モデルをくるくる回す
    
    ```html
    <a-animation  
        attribute="rotation"
        dur="3000"
        to="0 360 0"
        repeat="indefinite"
        easing="linear"
    >
    </a-animation>
    ```

### 完成ファイル
上記の内容を合わせてARサイトのHTMLを記載すると下記のようになります。
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width,intial-scale=1">
        <title>Name_AR</title>
    </head>

    <body style="margin:0px; overflow:hidden;">
    
        <!-- AFrameライブラリ読み込み -->
        <script src="https://aframe.io/releases/0.8.0/aframe.min.js"></script>
        <!-- AR.jsライブラリ読み込み -->
        <script src="https://jeromeetienne.github.io/AR.js/aframe/build/aframe-ar.js"></script>
        <!-- shaderライブラリ読み込み -->
        <script src="https://rawgit.com/mayognaise/aframe-gif-shader/master/dist/aframe-gif-shader.min.js"></script>
        <script src="shader-grid-glitch.js"></script>

        <!-- A-Frame開始 -->
        <a-scene embedded arjs="debugUIEnabled:false;">

            <!-- Assetの用意　 -->
            <a-assets>
                <!-- 3Dモデル  -->
                <a-asset-item id="3DModel" src="untitled2.obj"></a-asset-item>
                <!-- マテリアル -->
                <a-asset-item id="Material" src="untitled2.mtl"></a-asset-item>
            </a-assets>


            <!-- マーカー登録(デフォルトで用意されている『Hiro』マーカー) -->
            <a-marker preset="custom" type="pattern" url="pattern-marker.patt"></a-marker>

            <a-entity>
                <a-obj-model 
                    src="#3DModel"
                    mtl="#Material"
                    scale="0.5 0.5 0.5"
                    position="0 0.5 0">
                </a-obj-model>
                <a-animation  
                    attribute="rotation"
                    dur="3000"
                    to="360 360 0"
                    repeat="indefinite"
                    easing="linear">
                </a-animation>
            </a-entity>

            </a-marker>

        <!-- A-Frame終了 -->
        </a-scene>
        
    </body>
</html>
```
## フォルダ構成
- index.html
- untitled2.obj
- untitled2.mtl
- pattern-marker.patt


# 画像認識でWebARサイト作成
ARマーカーではなく、画像や物体などをマーカーとして使用する場合のARサイトの作成方法<br>
ARマーカーではないため、画像認証機能が含まれる
## 参考サイト
- [画像認識のAR.js](https://github.com/AR-js-org/AR.js#-image-tracking)
- [SEGA技術部でのAR開発に関する記事](https://segaxd.co.jp/blog/c7998bd6e8077395c31ed9925c3c5f740e935a38.html)
- [画像認証用ファイル作成サイト](https://carnaux.github.io/NFT-Marker-Creator/#/)
## ARサイトを作成する際に必要なファイル
- HTML(サイト本体)<br>
    サイトの本体になるファイルに、AR.jsのファイルを（scriptとして）取り込むだけで<br>
AR機能をサイト上で使用することができる<br>
- 3Dモデルファイル<br>
    objファイルとmtlファイルをでも可能だが、glftファイルが望ましい
- 認識させたい画像の画像認識用ファイル<br>
     [画像認証用ファイル作成サイト](https://carnaux.github.io/NFT-Marker-Creator/#/)でマーカーにしたい画像ファイルを画像認識ができるように、特徴をファイルに分割したファイルを用意する
<br>
<br>
## ファイルの用意
1. マーカーにしたい画像ファイルを画像認証用ファイルにする
    [画像認証用ファイル作成サイト](https://carnaux.github.io/NFT-Marker-Creator/#/)で画像を加工する<br>
    そこでできる「.fset」,「.fset3」,「.iset」の各拡張子がついている３ファイルには<br>
    画像の特徴がプログラムとして記載されており、画像認証時にそれらを使用する。
2. 3Dモデルファイル(上記と同じため省略)
<br>
<br>
## プログラム

1. サイト本体のHTMLファイルの枠組み作成(上記と同じため省略)

2. AR.jsファイルを記載を追加（AR.jsファイルのリンクを追加）
    ```html
    <!-- ↓↓↓↓↓↓ AR.jsファイルのリンクを追加　↓↓↓↓↓↓ -->
    <!-- AFrameライブラリ読み込み -->
    <script src="https://aframe.io/releases/1.0.0/aframe.min.js"></script> 
    <script src="https://raw.githack.com/AR-js-org/AR.js/master/aframe/build/aframe-ar-nft.js"></script> 
         <!-- ↑↑↑↑↑↑ AR.jsファイルのリンクを追加　↑↑↑↑↑↑ -->
    ```
3. AR用のフォーム外枠部分追加(上記と同じため省略)

4. 3Dモデルの配置（上記と同じでも可能）
    ```html
    <a-assets>
        <!-- 3Dモデル  -->
        <a-asset-item id="3DModelのID" src="objファイルパス"></a-asset-item>
        <!-- マテリアル -->
        <a-asset-item id="mtlファイルのID" src="mtlファイルパス"></a-asset-item>
    </a-assets>
    ```
    3Dモデルのobjファイルとmtlファイルを指定して項目のidを設定する。<br>
    動作させるマーカー内の記載部分で上記idからモデルを呼び出して動作させる。

5. マーカーとして認証したい画像でマーカータグを設定
    ```html
    <a-nft type="nft" url="">
    <a-nft > 
    ```
    nftタグは指定した画像ファイルから、画像を認識してマーカーとして使用する処理ができる<br>
    urlで画像認証したい画像の対象のフォルダとファイルを指定する。<br>
    <font color="Red">指定するファイルの拡張子は記載しない</font><br>
    例)nft-markerフォルダ内imageファイルの場合<br>
    ```
    nft-marker/image
    ```
    この場合のフォルダ構成は下記の通り
    - index.html
    - nft-marker
        - image.fset
        - image.fset3
        - image.iset
<br>
<br>
6. モデルの動き(アニメーション)の指定(上記と同じため省略)



### 完成ファイル
```html
    <!DOCTYPE html> 
    <html> 
    <head> 
        <script src="https://aframe.io/releases/1.0.0/aframe.min.js"></script> 
        <script src="https://raw.githack.com/AR-js-org/AR.js/master/aframe/build/aframe-ar-nft.js"></script> 
    </head> 
    <body> 
        <a-scene 
        vr-mode-ui="enabled: false" 
        embedded 
        arjs='sourceType: webcam; debugUIEnabled: false;' 
        > 
        <a-assets>

            <!-- 3Dモデル  -->
            <a-asset-item id="3DModel" src="untitled2.obj"></a-asset-item>
            <!-- マテリアル -->
            <a-asset-item id="Material" src="untitled2.mtl"></a-asset-item>

        </a-assets>

        <a-nft type="nft" url="nft-marker/image "> 
            <a-entity>
            <a-obj-model 
                src="#3DModel"
                mtl="#Material"
                scale="30 30 30"
                position="0 0 0">
            </a-obj-model>
            
            <a-animation  
                attribute="rotation"
                dur="3000"
                to="0 360 0"
                repeat="indefinite"
                easing="linear">
            </a-animation>
            
        </a-entity>
        </a-nft> 
            <a-entity camera></a-entity> 
        </a-scene> 
    </body> 
    </html>
  ```
  ## フォルダ構成
- index.html
- untitled2.obj
- untitled2.mtl
- nft-marker
    - image.fset
    - image.fset3
    - image.iset

### <font color="Red">テストを行う時の注意事項</font>
作成したサイトでAR機能を使用する際にはカメラ付きのデバイス（スマートフォンなど）でサイトを閲覧する必要がある。


### 10/15追記
## a-sceneタグ<br>
このタグでAR.jsの内容を読み込み、中身がARとして反映される部分です<br>
この中身の属性で表示される内容が変わります
```html
        <a-scene 
        vr-mode-ui="enabled: false" 
        embedded 
        arjs='sourceType: webcam; debugUIEnabled: false;' 
        > 
```
上記属性内のこの項目の設定でVRのように<br>
2画面で見せる機能を使用できるようになるゴーグルが表示される<br>
```html
        <!-- 表示の場合 -->
        <a-scene 
        vr-mode-ui="enabled: true" 
        > 

        <!-- 非表示の場合 -->
        <a-scene 
        vr-mode-ui="enabled: false" 
        > 
```
