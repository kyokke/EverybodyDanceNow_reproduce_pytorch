# 動作確認のための環境をメモ

##  オリジナルのレポジトリの環境
Ubuntu 16.04 <br>
Python 3.6.5 <br>
Pytorch 0.4.1  <br>
OpenCV 3.4.4  <br>


## 環境構築についてのメモ 

- 想定環境
 - Ubuntu 16.04. 
   - たぶん 18.04 でもいける ( つまり google collabo でもいけるはず) 

1. 下記をインストール
 - Python 3.6.5
 - CUDA 9.2 と それらをサポートする GPU & ドライバ
  - 松本は下記をインストール
    - RTX2080 Ti
    - CUDA V9.2.148 
    - nvidia driver 415

2. 検討用 仮想環境を作成
```
$ python3 -m venv env_pose2pose
```
3. 仮想環境に入る
```
$ source env_pose2pose/bin/activate
```
4. 必要なPythonパッケージをインストール
```
$ cd [repository root]
$ pip install -r requirements.txt
```
5. Pytorch 0.4.1 のインストール

 - 2019/02/21 現在、最新版は1.0。.whl ファイルからのインストール
 - https://pytorch.org/get-started/previous-versions/ 
 から CUDA 9.2 用 Pytorch の .whl ファイルをダウンロード, repositry root において..
```
$ pip install torch-0.4.1-cp36-cp36m-linux_x86_64.whl
$ pip install torch-vision
```
[補足]
- OpenCV 3.4.4
 - 非公式の opencv-python というパッケージが利用できるが、最新版を入れるとOpenCV v4 が入ってしまうので、バージョンを指定する。
 - pip install opencv-python=3.4.4.19
 これで、必要なバイナリも一緒にインストールされるようである。


## テスト動作させる上での注意点
基本的には、root repositry から すべてのコードを実行するようになっている。

## TODO
 - Pytorch 1.0 で実行できるようにしたいなぁ。。今更 0.41 使いたくない

 - face enhancement は一通り実行するのに、現状のレポジトリの状態ではface_enhance の結果は使われていない様子 -> やる? 
   ->  faece enhancement の動作確認は skip した (エイヤで動かそうとしたら、必要なファイルがないと怒られるが原因は追ってない)

 - 各 python コードの入出力関係を整理して、評価しやすくする

## その他メモ (松本用)

### face_enhancer/prepare.py

そのままだと関係するパスに矛盾があったので、 
```
$ python face_enhancer/prepar.py と実行する前提で、
```
../data などから始まるパスの指定はすべて ./data と1階修正。
また、prepare.py の6行目以降で指定されているディレクトリが一部存在しないと怒られる。自分でフォルダーを作る必要がある

### face_enhancer/main.py 

同じく、フォルダの指定パスが、おかしい。
現状、こちらは、
```
$ cd face_enhancer
$ python main.py
```
と、フォルダを潜って実行する前提で、エラーを解消した


また、一部 内部モジュールが存在しないと怒られたため、
__init.__.pyを適宜追加している。

main.py の is_debug = Falseにする。
(そうでないと毎バッチモデルデータが保存されてディスク容量があふれる)
