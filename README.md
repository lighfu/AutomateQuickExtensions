# アプリをインストールしないでください。このアプリにはセキュリティ上の問題があります。Do not Install this App
セキュリティ上の問題があるためこのアプリをインストールしないでください。
問題：
- 権限を持たない他のアプリケーションがBroadcast経由でこのアプリの機能を利用してユーザーの画面上にテキストを表示する事ができます。
- 許可なくビデオエンコード、デコードが利用できます。




# Quick Extensions for Automate (QeFa) とは
Androidの自動化アプリケーションである、Automate のために作成された拡張機能アプリケーションです。
主に、Automate にはない機能を搭載し、今後も仕様の変更や機能の追加などが行われます。ただし、例外を含めます。

Quick Extensions for Automate はこれ以降、QeFaと呼んでいきます。

## 機能など
- OverlayTextOrder (テキストを画面上にオーバーレイ表示)
- DialogOrder (アイコン付きのダイアログ、その他)
- FFmpegOrder (FFmpeg が利用可能)
- ToastOrder (カスタムトーストを表示)
今後追加予定です。

## 機能の対応

|      Orders      |     Start     |      Support     |     Description      |
|------------------|---------------|------------------|----------------------|
| OverlayTextOrder | t1.0a         | Android 4.1 - 11 | None |
| DialogOrder      | 1.0           | Android 4.0+     | None |
| FFmpegOrder      | 1.1           | Android 4.1 - 10 | 今後は別パッケージをインストールして利用が可能になるよう調節します。 |
| ToastOrder       | 1.0.alpha     | Android 4.0+     | None |

## 使い方

![画像](https://i.gyazo.com/b82d6c4dbcc7b527f6e59d317ac26a76.png)

Automate の Broadcast Send / Recive ブロックを使用して拡張機能を実行、リクエストできます。
要注意点として、拡張機能の設定で、Service を有効にする必要があります。

![画像](https://i.gyazo.com/f485c0d8d7a43c4f2ccea644720740e3.png)

Serviceを有効にした時点ですべての拡張機能へのアクセスを許可するという意味になります。 


###
 Extras の使い方

----

QeFaでは、機能のリクエストに、
- パッケージ名
- クラス名
- アクション名 (QeFaでは、大体 **Order** で通ります。)
- Extras
が必要になります。

### 予定更新内容
- 各オーダータイトルの変更
- ログの表示
- 実行権限（厳格モード）

## ライセンス
現在複製や改竄は認めていません。Github のリリースから必ずダウンロードしてください。
また、Github 以外で、リリースを公開していません。

今後、MITライセンスにするかもしれません。その場合は、ソースコードをここに全て置きたいと思います。


# QeFa 開発
QeFa は１人の開発者によって作成されています。
不完全なレイアウト、動作などが多くあると思われます。Github の Issues に問題を詳しく報告することで迅速に修正を行うことができます。
よろしくお願いします。

----

# Status 取得
QeFa は、Broadcast Send(結果待ち) でQeFa自体のステータスを取得することができます。

例えば、拡張機能が利用できるかどうか、拡張機能が許可されたかどうか…などなど。

## 取得方法

> "**Broadcast decision?**" ブロックを使用します。

QeFaから状態を知るには、以下のクラス名を使用します。

**Class**: `com.lighfu.quickextensions.AppStatusReceiver`

ステータス名は、**Action** に設定します。

現在使用できるステータス名は以下の通りです。

- checkMainService
- checkOverlayPermission
- checkStartUp
- checkUpdateCheckWhenStartApp
- checkExecutableFunction

---

### checkMainService
状態がQeFa側で確認された場合は、直ちにブロックの動作は続行されます。

戻り値（code）:  code = 0 || code = -1

![画像](https://i.gyazo.com/9ea10c00e297a6d55e5eea7f1382f0fa.png)

上記の式が、

条件        |  サービス
------------|--------------------
成立(Yes)   | 無効(Off)
不成立(No)  | 有効(On)

----


### checkOverlayPermission
QeFaでの画面上にオーバーレイ表示する権限が取得しているか、していないかを確認できます。

戻り値（code）: code = 1 ならば、取得済み。

戻り値（code）: code = 0 ならば、未許可。

Android 6.0+ ではこのチェックは有用ですが、それ以下のバージョンでは、結果は必ず "code = 1" になります。


----


### checkStartUp
QeFaの自動起動が有効か無効かを確認できます。

戻り値（code）: code = 1 ならば、有効。

戻り値（code）: code = 0 ならば、無効。


----


### checkUpdateCheckWhenStartApp
QeFaの起動時にアップデートを自動で確認するかしないかの設定を取得できます。

戻り値（code）: code = 1 ならば、確認する。

戻り値（code）: code = 0 ならば、確認されない。


----


### checkExecutableFunction
QeFa v1.3.0 から追加された、"オーダー許可" の状態を確認できます。
このアクションには、追加のExtrasが必要になります。

#### Extras:
```
{
    "function" : "[Order名]"
}
```

上記のようなExtrasを記述します。

それぞれのオーダーには名称があります。その名称を **[Order名]** で利用できます。**[]** は必要ありません。

- FFmpegOrder
- OverlayTextOrder
- ToastOrder
- DialogOrder

#### 戻り値（code）

戻り値（code）: code = 1 ならば、有効。

戻り値（code）: code = 0 ならば、無効。


----



# QeFaイベントを取得する
QeFa 内では、何かイベントが発生するとブロードキャストを送信するようになっています。

> 「**Broadcast Recive**」 ブロックを使用します

QeFaから何かイベント取得したい場合は、あらかじめ、「Broadcast recive」ブロックを実行させます。

たとえば、"Service" を有効にした時のイベントを取得したい場合は、以下のアクション名でブロードキャストが送信されます。

## QeFa.StartService
QeFaのサービスを有効にしたときに受信できるアクション。

![画像](https://i.gyazo.com/f8863932228edc6a2e652966997bd6e0.png)

取得できるExtras: 無し

「Broadcast recive」ブロックはこのアクションを取得するまで待機し続けます。




----
----

# OverlayText

![画像](https://i.gyazo.com/84e99d32a1554dc36e9401c3e62f5e51.png)

TextView を画面上に表示することが可能です。

ユーザーは、テキスト、色、背景色、サイズ、タッチアンカー、位置などを調節することができます。

そのため、手軽にユーザーに必要な情報を、通知や、Toastを介さずにそのまま表示することができます。

しかし、時々原因不明なエラーが発生することがあります。
その場合は、アプリがクラッシュするので注意してください。

## 必須条件
- Android 4.1 – Android 11
- 画面上に表示する 権限を付与すること。

## 注意点
- 特に上限なく、同時並行で利用できますが、新規呼び出しが多すぎるとシステムに負荷が掛かります。
- NoTouch を利用するとタッチを無視できますが、手動で消すことができなくなります。
- (調査中ですが)クラッシュすることがあります。

うまく動作しない（呼び出した直後にQeFaがクラッシュする）場合は、QeFaのアプリケーション設定から、画面上にオーバーレイ表示を許可する権限が与えられていないために、クラッシュしている可能性があります。
そのため、その権限を許可してみてください。
また、Serviceが有効になっていることも確認してください。


##  オーバーレイを開始する
> "Broadcast Send" ブロックを使用します。

**package**: `com.lighfu.quickextensions`

**class**: `com.lighfu.quickextensions.orders.OverlayTextOrder`

**Action**: **`Order`**

**id** : `OverlayText ID (String)` 制御IDを設定

> 制御IDをしっかりと指定してください。このIDを元に要素をいつでも変更できたり、停止させることが可能になります。


## オーバーレイを変更する

> "Broadcast Send" ブロックを使用します。

**package**: `com.lighfu.quickextensions`

**class**: 
`com.lighfu.quickextensions.orders.OverlayTextViewChangeOrder`

**Action**: **`Order`**

**オーバーレイ設定を変更、またはオーバーレイを削除したい場合にこのクラスを使用してください。**

----
 
### **Extras**:


`{"id":"set", "text": "(a.0) hello\n" ++ Now, "size" as Float: 24, "gravity" as StringArray: ["CENTER"]}` **(Ex.)**

----

**id**: 制御IDを設定
> 制御IDをしっかりと指定してください。このIDを元に要素をいつでも変更できたり、停止させることが可能になります。

----

**text**: オーバーレイ表示したい文字列
```
"text" as String: "Hello Automate!"
"text": "Hello QeFa!"
```

----

**size**: フォントサイズ px (**as Float**)

`"size" as Float: 8`

----

**gravity**: オーバーレイの基本位置

> **gravity** は  _StringArray_ を使用し、
同時に**2つ**まで指定することができます。

- "LEFT" : left
- "RIGHT": right
- "TOP" : top
- "BOTTOM" : bottom
- "CENTER" : center

#### Extras: gravity の例
```
"gravity" as StringArray: ["CENTER"]
"gravity" as StringArray: ["CENTER", "BOTTOM"]
```


----


**color**: 文字色 (Int 0xAARRGGBB)

`"color" as Int: 0xAARRGGBB` 

----


**backcolor**: 背景色 (Int 0xAARRGGBB)

`"backcolor" as Int: 0xAARRGGBB` 

`"backcolor" as Int: 0x88000000`  これは半透明な黒色

----

**pos-dp** OR **pos-px**: 表示位置調節 ( IntArray [ x,  y ] )
- **pos-dp** : スクリーンのdpiに従う。
- **pos-px** : 直接ピクセル単位で調節する。

----
**end**: オーバーレイを終了させるキーです。

値は必要ありませんが、このキーを付与するとオーバーレイが終了します。単体で動作します。すべて終了することはありません。

----

**NoTouch** : タッチ判定の有無を設定します。
```
"NoTouch" as Boolean: 0  // タッチ判定が有効 
"NoTouch" as Boolean: 1  // タッチ判定が無効
```
Boolean 型として扱います。
**NoTouch** を無効にするとタッチイベントが取得できなくなったり、手動でオーバーレイを終了できなくなります。

その代わりに、無効にすることで、タッチ判定がなくなるため誤った操作を防ぐことができます。

オーバーレイが削除できなくなった場合は、アプリの停止を設定から行うか、端末の再起動でリセットされます。

----



- ⚠️ 画面上に表示する設定を有効にしてください。
- ⚠️ There is a bug.
- ⚠️ Press and hold to remove the overlay.

### Extras の例
```
{
    "id": "test_id_00",
    "text": "(a.000) hello\nTextOverlay!!",
    "size" as Float: 34,
    "gravity" as StringArray: ["CENTER"]
}
```

----

## オーバーレイタッチイベントのキャッチ

**Extras** で、**NoTouch** を _有効_ にしていなければ、オーバーレイをタッチした判定を、Broadcast Recive で取得することができます。

### Broadcast Recive ブロック

**Action**: com.lighfu.quickextensions.OverlayText.Result.[ID]

[ID] は、オーバーレイ開始時に指定したIDを指定します。

取得できるExtras:
resultCode as Int, resultStatus as String

**resultCode**: 結果コード。
**resultStatus**: 結果テキスト。（英語/日本語）


- Broadcast Recive では、ブロードキャストの受信ができたらファイバーを直ちに続行します。


## オーバーレイを停止させる

> "Broadcast Send" ブロックを使用します。

**package**: `com.lighfu.quickextensions`

**class**: 
`com.lighfu.quickextensions.orders.OverlayTextViewChangeOrder`

**Action**: **`Order`**

削除したい対象のIDを、Extras に指定します。

また、"end" キーを Extras に追加します。

{"id": "set", **"end":""**}




----









# FFmpegOrder

![https://raw.githubusercontent.com/tanersener/mobile-ffmpeg/master/docs/assets/mobile-ffmpeg-logo-v7.png](https://github.com/tanersener/ffmpeg-kit/raw/main/docs/assets/ffmpeg-kit-icon-v9.png)

FFmpegKit: [https://github.com/tanersener/ffmpeg-kit](https://github.com/tanersener/ffmpeg-kit)

FFmpegOrder は、FFmpegコマンドラインを提供する特別な機能です。

FFmpegのほぼ全ての機能を利用することができ、
動画、音声ファイルのエンコードやデコードを行うことができます。

> この機能はAPKサイズを巨大化させるため、現在、ライブラリが同梱されていますが、com.lighfu.quickextensions の他のパッケージに移行する予定で、今後は、この別のパッケージをインストールしてFFmpegを利用できるようにする予定です。

## 必須条件
Android 4.1 - 10 までのデバイスでのみ動作します。
Amdroid 11+ では回避策を検討していますが、実装できていません。

## 注意点
- 並行処理ができるようになっていますが、処理を行うとかなりのシステムリソースを消費します。
- 多用すると端末全体のパフォーマンスが低下するので注意してください。
- **既存のファイルに上書きすることはありません。保存先にすでにファイルが存在する場合は処理に即失敗します。**

## FFmpeg を実行する
Broacast Send ブロックを使用して、FFmpegコマンドを実行できます。

### Broadcast send
**Package**: `com.lighfu.quickextensions`
**Class**: `com.lighfu.quickextensions.orders.FFmpegOrder`

**Action**: `Order`

**_Extras_**:
"id" as String: 任意のID文字列
"cmd" as String: 任意の実行コマンド（'`ffmpeg `'は不要）（`-i` は必要です。）

> id の指定を必ず行ってください。id は動作を識別するほか、ユーザー側で操作を可能にするものです。

### Extras の例
```
{
    "id" as  String: "test.0000.6a0cf7.ffmpeg.order",
    "cmd" as String: "-i /sdcard/test.mp4 /sdcard/test.m4a"
}
```


## 処理中のOrderを停止させる
**id**の指定が行われている処理スレッドでは、意図的にFFmpegの処理を停止させることができます。

操作は、Broadcast Send ブロックを使用して行います。

### Broadcast send
**Package**: `com.lighfu.quickextensions`

**Class**: `com.lighfu.quickextensions.action.execute.ffmpeg.FFmpegCancelReceiver`

**Action**: `Order`

**Extras**:

"**id**" as String: 停止させたい動作ID

### Extras の例
```
{
    "id" as  String: "test.0000.6a0cf7.ffmpeg.order"
}
```

## 処理の結果を取得する
idから処理の結果を受けとることができます。
結果の種類は、
1. 成功
2. ユーザーによるキャンセル
3. 失敗
の3つがあります。

Broadcast Recive を使用して結果を受けとります。

### Broadcast receive
**Action**: `com.lighfu.quickextensions.FFmpeg.Result.{id}`

####  取得可能な extras
```
resultCode as Int: 
0: Success
1: User Cancel
2: Failed
```

`resultStatus as String:`  QeFaからの処理に関するメッセージ文字列

## 処理の進捗を取得する
FFmpegで処理中の進捗を取得することができます。
更新時間は1秒です。

### Broadcast receive
**Action**: `com.lighfu.quickextensions.FFmpeg.Statistic.{id}`


### extras の戻り値:

**Bitrate** as `Double`

**Size** as `Long`

**Speed** as `Double`

**Time** as `Int`

**VideoFps** as `Float`

**VideoFrameNumber** as `Int`

**VideoQuality** as `Float`




----




# ToastOrder

![Toast](https://i.gyazo.com/fbb602dae61fe40799257cfe57a0ccbf.png)


ToastOrder は基本四角形のトーストを提供します。

文字色、背景色、サイズ、Padding等を指定してスタイル付けすることができます。

## 必須条件
ほぼ全てのAndroidで動作可能です。

## 注意点
- トーストの表示タイミングはAndroidOSによって管理されています。

## トーストを表示する

> 

**Package**: `com.lighfu.quickextensions`

**Class**: `com.lighfu.quickextensions.orders.ToastOrder`

**Action**: `Order`

### Extras:


"text": String

トーストに表示したい文字

----

"size" as Float: Number

トーストに表示する文字の大きさ。 px単位

----

"color" as Int: 0xAARRGGBB
Int 型として、16進数で表記する。

AA が、16進数のアルファ値。 (00-FF)

RR が、16進数のアルファ値。 (00-FF)

GG が、16進数のアルファ値。 (00-FF)

BB が、16進数のアルファ値。 (00-FF)

赤色の場合は、

`"color" as Int: 0xFFFF0000`

緑色の場合は、

`"color" as Int: 0xFF00FF00`

青色の場合は、

`"color" as Int: 0xFF0000FF`

**0x** フラグを使用して16進数で分かりやすい形式として、扱うことをオススメします。

> 0x フラグは、Automate内で数値に変換されます。

例えば、
**0xFFFF0000** は **-65536** に変換されます。
（0xFFFF0000 は 赤色です）

----

"**backcolor**" as Int: 0xAARRGGBB

背景色を決めます。上記と色の指定はまったく同じです。

----

"**duration**" as Int: 0 or 1

トーストの表示時間を制御するものです。

- **0** では、トースト表示時間は短いです。
- **1** では、トースト表示時間は少し長めです。

**注意**: これは秒数ではありません。

----

**padding**: [Left, Top, Right, Bottom]

"**padding**" as IntArray: [64, 64, 64, 64]

文字と外枠との空間の幅を設定できます。

----

**offset**: [x, y]

"**offset**" as IntArray: [0, 0]

ピクセル単位でトーストの位置を微調整できます。

----

"**gravity**" as StringArray: ["CENTER"]

**gravity** as StringArray: [String]  もしくは、

**gravity** as StringArray: [String, String]

使える重力フラグは以下の通りです。

- "**LEFT**"
- "**RIGHT**"
- "**TOP**"
- "**BOTTOM**"
- "**CENTER**"

必ず単体の重力フラグを指定するときでも、StringArray、文字列配列として扱う必要があります。

たとえば、右寄せの場合。
```
"gravity" as StringArray: ["RIGHT"]
```

右寄せ & 下方 の場合。
```
"gravity" as StringArray: ["RIGHT","BOTTOM"]
```

このフラグは、必ず同時に2つまでしか利用できません。

**gravity** Extras を付与したのに、フラグを記述しないとQeFaではエラーが発生します。

----

## トーストの今後

今現在は、まだ四角形のトーストしか表示できませんが、将来的に色々な形を表示できるようにしたい。できれば。
