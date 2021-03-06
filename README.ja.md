# <img src="https://raw.githubusercontent.com/kitao/pyxel/master/pyxel/examples/assets/pyxel_logo_152x64.png">

[ [English](https://github.com/kitao/pyxel/blob/master/README.md) | [日本語](https://github.com/kitao/pyxel/blob/master/README.ja.md) | [Other Languages](https://github.com/kitao/pyxel/wiki) ]

**Pyxel (ピクセル)** はPython向けのレトロゲームエンジンです。

使える色は16色のみ、同時に再生できる音は4音までなど、レトロゲーム機を意識したシンプルな仕様で、Pythonでドット絵スタイルのゲームづくりが気軽に楽しめます。

<a href="https://github.com/kitao/pyxel/blob/master/pyxel/examples/01_hello_pyxel.py" target="_blank">
<img src="https://raw.githubusercontent.com/kitao/pyxel/master/pyxel/examples/screenshots/01_hello_pyxel.gif" width="48%">
</a>

<a href="https://github.com/kitao/pyxel/blob/master/pyxel/examples/02_jump_game.py" target="_blank">
<img src="https://raw.githubusercontent.com/kitao/pyxel/master/pyxel/examples/screenshots/02_jump_game.gif" width="48%">
</a>

<a href="https://github.com/kitao/pyxel/blob/master/pyxel/examples/03_draw_api.py" target="_blank">
<img src="https://raw.githubusercontent.com/kitao/pyxel/master/pyxel/examples/screenshots/03_draw_api.gif" width="48%">
</a>

<a href="https://github.com/kitao/pyxel/blob/master/pyxel/examples/04_sound_api.py" target="_blank">
<img src="https://raw.githubusercontent.com/kitao/pyxel/master/pyxel/examples/screenshots/04_sound_api.gif" width="48%">
</a>

<a href="https://github.com/kitao/pyxel/blob/master/pyxel/editor/screenshots/image_tilemap_editor.gif" target="_blank">
<img src="https://raw.githubusercontent.com/kitao/pyxel/master/pyxel/editor/screenshots/image_tilemap_editor.gif" width="48%">
</a>

<a href="https://github.com/kitao/pyxel/blob/master/pyxel/editor/screenshots/sound_music_editor.gif" target="_blank">
<img src="https://raw.githubusercontent.com/kitao/pyxel/master/pyxel/editor/screenshots/sound_music_editor.gif" width="48%">
</a>

Pyxelのゲーム機の仕様やAPI、パレットなどは、
[PICO-8](https://www.lexaloffle.com/pico-8.php)や[TIC-80](https://tic.computer/)のデザインを参考にしています。

Pyxelはオープンソースで、無料で自由に使えます。Pyxelでレトロゲームづくりを始めましょう！

## 仕様

- Windows、Mac、Linux対応
- Python3によるコード記述
- 16色固定パレット
- 256x256サイズ、3画像バンク
- 256x256サイズ、8タイルマップ
- 4音同時再生、定義可能な64サウンド
- 任意のサウンドを組み合わせ可能な8ミュージック
- キーボード、マウス、ジョイスティック (予定)
- 画像・サウンド編集ツール

### カラーパレット

<img src="https://raw.githubusercontent.com/kitao/pyxel/master/pyxel/examples/screenshots/05_color_palette.png">

## インストール方法

### Windows

[Python3](https://www.python.org/)をインストールした後に、以下の`pip`コマンドでPyxelをインストールします。

```sh
pip install pyxel
```

### Mac

[Python3](https://www.python.org/)と[glfw](http://www.glfw.org/) (バージョン3.2.1以上) をインストールをした後に、`pip`コマンドでPyxelをインストールします。

[Homebrew](https://brew.sh/)を導入している環境では、以下のコマンドで必要なパッケージが一通りインストールできます。

```sh
brew install python3 glfw
pip3 install pyxel
```

### Linux

各ディストリビューションに適した方法で必要なパッケージをインストールしてください。[glfw](http://www.glfw.org/)はバージョン3.2.1以上である必要があります。

**Arch:**

AURヘルパーで[`python-pixel`](https://aur.archlinux.org/packages/python-pyxel/)をインストールします。

```sh
yay -S python-pyxel
```

**Debian:**

```sh
apt-get install python3 python3-pip libglfw3 libportaudio2 libasound-dev
pip3 install pyxel
```

**Fedora:**

```sh
dnf install glfw portaudio
pip3 install pyxel
```

### サンプルのインストール

Pyxelインストール後に、以下のコマンドでカレントディレクトリにPyxelのサンプルコード一式をコピーできます。

```sh
install_pyxel_examples
```

サンプルは通常のPythonコードと同様に実行できます。

```sh
cd pyxel_examples
python 01_hello_pyxel.py
```

または

```sh
cd pyxel_examples
python3 01_hello_pyxel.py
```

## 使い方

### アプリケーションの作成方法

Pythonコード内でPyxelモジュールをインポートして、`init`関数でウィンドウサイズを指定した後に、`run`関数でPyxelアプリケーションを開始します。

```python
import pyxel

pyxel.init(160, 120)

def update():
    if pyxel.btnp(pyxel.KEY_Q):
        pyxel.quit()

def draw():
    pyxel.cls(0)
    pyxel.rect(10, 10, 20, 20, 11)

pyxel.run(update, draw)
```

`run`関数の引数にはフレーム更新処理を行う`update`関数と、描画処理を行う`draw`関数を指定します。

実際のアプリケーションでは、以下のようにクラスでPyxelの処理をラップするのがおすすめです。

```python
import pyxel

class App:
    def __init__(self):
        pyxel.init(160, 120)
        self.x = 0
        pyxel.run(self.update, self.draw)

    def update(self):
        self.x = (self.x + 1) % pyxel.width

    def draw(self):
        pyxel.cls(0)
        pyxel.rect(self.x, 0, self.x + 7, 7, 9)

App()
```

### 特殊操作

Pyxelアプリケーション実行中に、以下の特殊操作を行うことができます。

- `Alt(Option)+1`  
スクリーンショットをデスクトップに保存する
- `Alt(Option)+2`  
画面キャプチャ動画の録画開始時刻をリセットする
- `Alt(Option)+3`  
画面キャプチャ動画 (gif) をデスクトップに保存する (最大30秒)
- `Alt(Option)+0`  
パフォーマンスモニタ (fps、update時間、draw時間) の表示を切り替える
- `Alt(Option)+Enter`  
フルスクリーン表示を切り替える

### Pyxel Editor

付属するPyxel EditorでPyxelアプリケーションで使用する画像やサウンドを作成することができます。

Pyxel Editorは任意のリソースファイル名を指定して起動します。

```sh
pyxeleditor pyxel_resource_file
```

作成したリソースファイル(.pyxel)はPyxelアプリケーションから`load`関数で読み込めます。

Pyxel Editorには次の編集モードがあります。

**イメージエディタ:**

イメージバンクの画像を編集する画面です。

<img src="https://raw.githubusercontent.com/kitao/pyxel/master/pyxel/editor/screenshots/image_editor.gif">

**タイルマップエディタ:**

イメージバンクの画像をタイル状に並べたタイルマップを編集する画面です。

<img src="https://raw.githubusercontent.com/kitao/pyxel/master/pyxel/editor/screenshots/tilemap_editor.gif">

**サウンドエディタ:**

サウンドを編集する画面です。

<img src="https://raw.githubusercontent.com/kitao/pyxel/master/pyxel/editor/screenshots/sound_editor.gif">

**ミュージックエディタ:**

サウンドを再生順に並べたミュージックを編集する画面です。

<img src="https://raw.githubusercontent.com/kitao/pyxel/master/pyxel/editor/screenshots/music_editor.gif">

### その他のリソース作成方法

Pyxel用の画像やタイルマップは以下の方法で作成することもできます。

- `Image.set`や`Tilemap.set`関数で文字列のリストから作成する
- `Image.load`関数でPyxel向け配色のpngファイルを読み込む

Pyxelは[PICO-8](https://www.lexaloffle.com/pico-8.php)と同じパレットを使用しているため、Pyxel向け配色のpngファイルを作成する場合は、[Aseprite](https://www.aseprite.org/)をPICO-8パレット設定にして使用するのがおすすめです。

Pyxel用のサウンドやミュージックは以下の方法で作成することもできます。

- `Sound.set`や`Music.set`関数で文字列から作成する

各関数の使い方はAPIリファレンスを参照してください。

## APIリファレンス

### システム

- `width`, `height`  
画面の幅と高さ

- `frame_count`  
経過フレーム数

- `init(width, height, [caption], [scale], [palette], [fps], [border_width], [border_color])`  
Pyxelアプリを画面サイズ (`width`, `height`) で初期化する。画面の最大の幅と高さは255  
`caption`でウィンドウタイトル、`scale`で表示倍率、`palette`でパレット色、`fps`で動作フレームレート、`border_width`と`border_color`で画面外側のマージン幅と色を指定できる。`palette`は24ビットカラーの16要素のリスト、`border_color`は24ビットカラーで指定する

- `run(update, draw)`  
Pyxelアプリを開始し、フレーム更新時に`update`関数、描画時に`draw`関数を呼ぶ

- `quit()`  
現在フレーム終了時にPyxelアプリを終了する

### リソース

- `save(filename)`  
実行スクリプトのディレクトリにリソースファイル (.pyxel) を保存する

- `load(filename)`  
実行スクリプトのディレクトリからリソースファイル (.pyxel) を読み込む

### 入力
- `mouse_x`, `mouse_y`  
現在のマウスカーソル座標

- `btn(key)`  
`key`が押されていたら`True`、押されていなければ`False`を返す ([キー定義一覧](https://github.com/kitao/pyxel/blob/master/pyxel/constants.py))

- `btnp(key, [hold], [period])`  
そのフレームに`key`が押されたら`True`、押されなければ`False`を返す。`hold`と`period`を指定すると、`hold`フレーム以上ボタンを押し続けた際に`period`フレーム間隔で`True`が返る

- `btnr(key)`  
そのフレームに`key`が離されたら`True`、離されなければ`False`を返す

- `mouse(visible)`  
`visible`が`True`ならマウスカーソルを表示し、`False`なら非表示にする。マウスカーソルが非表示でも座標は更新される。

### グラフィックス

- `image(img, [system])`  
イメージバンク`img`(0-2) を操作する (イメージクラスを参照のこと)。`system`に`True`を指定すると、システム用のイメージバンク3にアクセスできる  
例：`pyxel.image(0).load(0, 0, 'title.png')`

- `tilemap(tm)`  
タイルマップ`tm`(0-7)を操作する (タイルマップクラスを参照のこと)。

- `clip(x1, y1, x2, y2)`  
画面の描画領域を (`x1`, `y1`)-(`x2`, `y2`) にする。`clip()`で描画領域をリセットする

- `pal(col1, col2)`  
描画時に色`col1`を`col2`に置き換える。`pal()`で初期状態にリセットする

- `cls(col)`  
画面を色`col`でクリアする

- `pix(x, y, col)`  
(`x`, `y`) に色`col`のピクセルを描画する

- `line(x1, y1, x2, y2, col)`  
色`col`の直線を (`x1`, `y1`)-(`x2`, `y2`) に描画する

- `rect(x1, y1, x2, y2, col)`  
色`col`の矩形を (`x1`, `y1`)-(`x2`, `y2`) に描画する

- `rectb(x1, y1, x2, y2, col)`  
色`col`の矩形の輪郭線を (`x1`, `y1`)-(`x2`, `y2`) に描画する

- `circ(x, y, r, col)`  
半径`r`、色`col`の円を (`x`, `y`) に描画する

- `circb(x, y, r, col)`  
半径`r`、色`col`の円の輪郭線を (`x`, `y`) に描画する

- `blt(x, y, img, u, v, w, h, [colkey])`  
イメージバンク`img`(0-2) の (`u`, `v`) からサイズ (`w`, `h`) の領域を (`x`, `y`) にコピーする。`w`、`h`それぞれに負の値を設定すると水平、垂直方向に反転する。`colkey`に色を指定すると透明色として扱われる

- `bltm(x, y, img, tm, tu, tv, tw, th, [colkey])`  
イメージバンク`img`(0-2) をタイルマップ`tm`(0-7) の (`tu`, `tv`) からサイズ (`tw`, `th`) のタイル情報に従って (`x`, `y`) にコピーする。`colkey`に色を指定すると透明色として扱われる。タイルマップは1タイルが8x8のサイズで描画され、タイル番号が0ならイメージバンクの (0, 0)-(7, 7) の領域、1なら (8, 0)-(15, 0) の領域を表す

- `text(x, y, s, col)`  
色`col`の文字列`s`を (`x`, `y`) に描画する

### オーディオ

- `sound(snd, [system])`  
サウンド`snd`(0-63) を操作する (サウンドクラスを参照のこと)。`system`に`True`を指定すると、システム用のサウンド64にアクセスできる  
例：`pyxel.sound(0).speed = 60`

- `music(msc)`  
ミュージック`msc`(0-7) を操作する (ミュージッククラスを参照のこと)

- `play(ch, snd, loop=False)`  
チャンネル`ch`(0-3) でサウンド`snd`(0-63) を再生する。`snd`がリストの場合順に再生する

- `playm(msc, loop=False)`  
ミュージック`msc`(0-7) を再生する

- `stop([ch])`  
全チャンネルのサウンドの再生を停止する。`ch`(0-3) を指定すると該当チャンネルのみを停止する

### イメージクラス

- `width`, `height`  
イメージの幅と高さ

- `data`  
イメージのデータ (NumPy配列)

- `get(x, y)`  
イメージの (`x`,`y`) のデータを取得する

- `set(x, y, data)`  
(`x`, `y`) に値または文字列のリストでイメージのデータを設定する  
例：`pyxel.image(0).set(10, 10, ['1234', '5678', '9abc', 'defg'])`

- `load(x, y, filename)`  
(`x`, `y`) に実行スクリプトのディレクトリからpngファイルを読み込む

- `copy(x, y, img, u, v, w, h)`  
イメージバンク`img`(0-2) の (`u`, `v`) からサイズ (`w`, `h`) の領域を (`x`, `y`) にコピーする

### タイルマップクラス

- `width`, `height`  
タイルマップの幅と高さ

- `data`  
タイルマップのデータ (NumPy配列)

- `get(x, y)`  
タイルマップの (`x`,`y`) のデータを取得する

- `set(x, y, data)`  
(`x`, `y`) に値または文字列のリストでタイルマップのデータを設定する  
e.g. `pyxel.tilemap(0).set(0, 0, ['000102', '202122', 'a0a1a2', 'b0b1b2'])`

- `copy(x, y, tm, u, v, w, h)`  
タイルマップ`tm`(0-7) の (`u`, `v`) からサイズ (`w`, `h`) の領域を (`x`, `y`) にコピーする

### サウンドクラス

- `note`  
音程 (0-127) のリスト (33 = 'A2' = 440Hz)

- `tone`  
音色 (0:Triangle / 1:Square / 2:Pulse / 3:Noise) のリスト

- `volume`  
音量 (0-7) のリスト

- `effect`  
エフェクト (0:None / 1:Slide / 2:Vibrato / 3:FadeOut) のリスト

- `speed`  
1音の長さ (120 = 1音1秒)

- `set(note, tone, volume, effect, speed)`  
文字列で音程、音色、音量、エフェクトを設定する。音色、音量、エフェクトの長さが音程より短い場合は、先頭から繰り返される

- `set_note(note)`  
'CDEFGAB'+'#-'+'0123'または'R'の文字列で音程を設定する。大文字と小文字を区別せず、空白は無視される  
例：`pyxel.sound(0).set_note('G2B-2D3R RF3F3F3')`

- `set_tone(tone)`  
'TSPN'の文字列で音色を設定する。大文字と小文字を区別せず、空白は無視される  
例：`pyxel.sound(0).set_tone('TTSS PPPN')`

- `set_volume(volume)`  
'01234567'の文字列で音量を設定する。大文字と小文字を区別せず、空白は無視される  
例：`pyxel.sound(0).set_volume('7777 7531')`

- `set_effect(effect)`  
'NSVF'の文字列でエフェクトを設定する。大文字と小文字を区別せず、空白は無視される  
例：`pyxel.sound(0).set_effect('NFNF NVVS')`

### ミュージッククラス

- `ch0`  
チャンネル0で再生するサウンド (0-63) のリスト

- `ch1`  
チャンネル1で再生するサウンド (0-63) のリスト

- `ch2`  
チャンネル2で再生するサウンド (0-63) のリスト

- `ch3`  
チャンネル3で再生するサウンド (0-63) のリスト

- `set(ch0, ch1, ch2, ch3)`  
全チャンネルのサウンド (0-63) のリストを設定する  
例：`pyxel.music(0).set([0, 1], [2, 3], [4], [])`

- `set_ch0(data)`  
チャンネル0のサウンド (0-63) のリストを設定する

- `set_ch1(data)`  
チャンネル1のサウンド (0-63) のリストを設定する

- `set_ch2(data)`  
チャンネル2のサウンド (0-63) のリストを設定する

- `set_ch3(data)`  
チャンネル3のサウンド (0-63) のリストを設定する

## コントリビューション方法

### 問題の報告

不具合の報告や機能の要望は[Issue Tracker](https://github.com/kitao/pyxel/issues)で受け付けています。
新しいレポートを作成する場合は、同じ内容のものがないか事前に確認をお願いします。

新しいレポートの作成は[こちら](https://github.com/kitao/pyxel/issues/new)から行えます。

### 動作確認

動作確認を行い、[Issue Tracker](https://github.com/kitao/pyxel/issues)で不具合の報告や改善の提案をしてくれる方は大歓迎です！

### プルリクエスト

パッチや修正はプルリクエスト(PR)として受け付けています。提出の前に問題がすでに解決済みでないか[Issue Tracker](https://github.com/kitao/pyxel/issues)で確認をお願いします。

提出されたプルリクエストは[MITライセンス](https://github.com/kitao/pyxel/blob/master/LICENSE)で公開することに同意したものを見なされます。

## その他の情報

- [Wiki](https://github.com/kitao/pyxel/wiki)
- [Subreddit](https://www.reddit.com/r/pyxel/)

## ライセンス

Pyxelは[MITライセンス](http://en.wikipedia.org/wiki/MIT_License)です。ソースコードやライセンス表示用のファイル等で、[著作権とライセンス全文](https://raw.githubusercontent.com/kitao/pyxel/master/LICENSE)の表示を行えば、自由に販売や配布をすることができます。
