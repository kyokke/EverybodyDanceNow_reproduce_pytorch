# 動作確認のための環境をメモ

##  オリジナルのレポジトリの環境
Ubuntu 16.04 <br>
Python 3.6.5 <br>
Pytorch 0.4.1  <br>
OpenCV 3.4.4  <br>


## 環境構築についてのメモ

とりあえず最初の動作確認では、書かれている環境になるべく近づけることにした。
Pythonパッケージ系については、

pip install -r requirements.txt 

すればよい。

- Ubuntu 16.0.4 を使っているのでOK

- Python 3.6.5
 - pyenv を使って 本プロジェクトに対して python 3.6.5 を使用するように

- Pytorch 0.4.1 
 - 2019/02/21 現在、最新版は1.0のため、.whl ファイルからのインストール
 - https://pytorch.org/get-started/previous-versions/ 
  - これによると、0.4.1 のビルド済みパッケージは CUDA 9.2 版が最新なので、NVIDIA系の環境もそれに合わせた
    - CUDA 9.2.148
    - nvidia driver 415
    - ちなみに GPUは、RTX2080 Ti & GTX-1080 Ti 

- OpenCV 3.4.4
 - 非公式の opencv-python というパッケージが利用できるが、最新版を入れるとOpenCV v4 が入ってしまうので、バージョンを指定する。
 - pip install opencv-python=3.4.4.19

- その他
 - 上記を準備しただけでは、エラーが多発するため、都度見つからないモジュールをインストール

## TODO

- Pytorch 1.0 で実行できるようにしたいなぁ。。今更 0.41 使いたくない
- ゆくゆくは 設定済みのnvidia-docker を提供したい. 
 
# 動作させるまでのメモ

## とりあえず python 0.4.1 で
実行したら、

### face_enhancer/prepare.py

そのままだと関係するパスに矛盾があったので、

python face_enhancer/prepar.py 
と実行する前提で、

../data などから始まるパスの指定はすべて
./data と1階層直した
 6行目からのディレクトリがない場合は、作る。

### face_enhancer/main.py 
なおフォルダの指定パスが、おかしいので
cd face_enhancer
python main.py 
としないと、相対パスの解決ができない

face_enahncer モジュールがないと言われる
-> 実際には、 face_enhancer/ から main.py を実行するので　
import している utils を パッケージとして認識してもらえるように, 
__init__.py を utils フォルダに追加

main.py の is_debug = Falseに
(そうでないと毎バッチモデルデータが保存されてディスクがあふれる)

TODO: これもレポジトリのルートから実行できるようにパスを変更する