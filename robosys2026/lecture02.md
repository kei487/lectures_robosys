---
marp: true
---
<!--
style: |
  section {
    line-height: 1.15;
  }
-->

<!-- footer: "ロボットシステム学第2回" -->

# ロボットシステム学

## 第2回: Linux環境でのPythonプログラミング I

鈴木 太郎（千葉工業大学）

<span style="font-size:70%">オリジナル: 上田 隆一（千葉工業大学）[ロボットシステム学 2025](https://github.com/ryuichiueda/slides_marp/tree/master/robosys2025) を改変</span>

<br />

<span style="font-size:70%">This work is licensed under a </span>[<span style="font-size:70%">Creative Commons Attribution-ShareAlike 4.0 International License</span>](https://creativecommons.org/licenses/by-sa/4.0/).
![](https://i.creativecommons.org/l/by-sa/4.0/88x31.png)

---

<!-- paginate: true -->

## 今日やること

- 前半: Pythonの入門
    - C言語をちょっとやったくらいの人を想定
    - 変数、関数、リスト、for文
        - とりあえずこれくらい覚えておけば、できることは結構多い<br />　
- 後半: LinuxでPython等のコードを使うときの方法
    - Linux環境においてPythonスクリプトの作り方と実行の仕方
    - ROS2でのPythonプログラム作成につながる内容

---

## 前半: Python入門

---

## なぜPythonなのか？

- 言語の特徴
    - コンパイル不要で書いたらすぐ実行できる
    - ロボットは「動かして直す」の繰り返し $\rightarrow$ 試行錯誤の速さが重要<br />　
- ロボティクス分野での実績
    - 画像処理、機械学習、数値計算のライブラリが豊富
        - OpenCV、PyTorch、NumPyなど
    - 速度が必要な部分だけC++で書き、Pythonで組み合わせるのが定番<br />　
- ROSでのサポート
    - ROS 2の公式対応言語はPythonとC++。チュートリアルもPythonが基本
    - ROSのノードはシバンをつけたPythonスクリプト $\rightarrow$ 今日の後半<br />　
- [人気](https://spectrum.ieee.org/top-programming-languages-2025)

---

## 最初のコード

- `nano` または `Vim` または `Emacs` で書きましょう
    - ファイル名は`hello.py`
        ```bash
        $ nano hello.py
        ```
    - `hello.py`の中身（2行だけ）
        ```python
        print("hello")              #文字列を出力
        print(3.14)                 #数字を出力
        ```
        - 「`#`」より後ろはコメント
- 実行（<span style="color:red">`python3`</span>コマンドの引数として読み込ませる）
    ```bash
    $ python3 hello.py
    hello
    3.14
    ```

---

## Pythonの文字列、数字と関数

- 文字列、数字（<span style="color:red">リテラル</span>）
    - 文字列は`""`や`''`で囲む
        ```python
        "hello", 'hello', "That's a pen."                                                
        ```
    - 数字はそのまま記述
        ```python
        3.14, -1, 5
        1.23e+100 #1.23かける10の100乗                                             
        ```
- 「**関数**」: 「`関数名(引数, 引数, ...)`」と記述
    - 例:  `print`関数
        - 字を端末に出力する
        - C言語の`puts`や`printf`などに相当
        - 引数として文字列も数字も（他のいくつかの型も）受け付ける

---

## Pythonの変数

- 要点
    - 値（数字や文字列、その他のもの）に名前を付与できる
        - 「値を変数に代入」というより「値を名前で参照」
    - 型は書かなくてよい
    ```python
    ### コードの例 ###
    name = "鈴木"    #「鈴木」という文字列にnameという名前を付ける
    money = 5       #5という数字にmoneyという名前をつける
    print("{}の所持金: {}円".format(name, money) ) #文字列に対してformatメソッドを呼び出し                      
    ```
    - 「**メソッド**」: 値や変数に対してなにか操作するための関数のようなもの
    - 実行（`var.py`というファイル名で）
        ```bash
        $ python3 var.py
        鈴木の所持金: 5円                                                          
        ```

---

## Pythonのリストとfor文

- 要点
   - **リスト**: 複数の値を並べて格納できる変数
   - **for**文: リストの要素をひとつずつ処理するもの
       - for文の中身は右側に余白（<span style="color:red">インデント</span>）
        ```python
        ### コードの例 ###
        fruits = ["apple", "banana", "cherry" ]  #文字列3つのリストをfruitsと命名                   

        for f in fruits:
            print(f + "はおいしい")             # + で文字列を連結できる
        #↑インデントは半角空白4文字が標準（講義では厳密に守ること）
        ```
    - 実行（`fruits.py`というファイル名でコードを保存）
        ```python
        $ python3 fruits.py
        appleはおいしい
        bananaはおいしい
        cherryはおいしい
        ```
---

## リスト要素の取得

- 要点
    - 基本はC言語と同じ
    - 負の値を指定すると後ろから要素を指定可能
    ```python
    ### コードの例（list.py） ###
    fruits = ["apple", "banana", "cherry" ]
    print("最初の要素: " + fruits[0])      #先頭は0番目
    print("次の要素: " + fruits[1])        #次の要素は1番目
    print("最後の要素: " + fruits[-1])     #最後の要素
    ```
- 実行結果
    ```bash
    $ python3 list.py
    最初の要素: apple
    次の要素: banana
    最後の要素: cherry
    ```

---

## リスト要素の範囲指定（スライス）

- リストを部分的に取り出して新たにリストを作成できる
    - `[start:stop:step]`
    ```python
    ### コードの例（list2.py） ###
    fruits = ["a", "b", "c", "d", "e" ]
    print("0番目から2番目の要素: ", fruits[0:3])  #終わりの番号(3)は含まれない
    print("2番目以降の要素: ", fruits[2:])
    print("ひとつ飛ばしでリストを作成: ", fruits[0::2])
    ```
    - 注意: 上のprintでは出力したい文字列をふたつの引数に分けている
- 実行結果
    ```bash
    $ python3 list2.py 
    0番目から2番目の要素:  ['a', 'b', 'c']
    2番目以降の要素:  ['c', 'd', 'e']
    ひとつ飛ばしでリストを作成:  ['a', 'c', 'e']
    ```

---

## 練習

- 次のようなPythonのスクリプトを作ってみましょう
    - なにかリストを作成して、作ったリストについて、奇数番目の要素だけ出力
      - スライスを利用する<br />　
    - さらに余裕があれば、出力する際になにか文字を付加
        - 例: 7ページ or 8ページ

---

## 後半: Linuxとスクリプト

---

## hello.pyの実行方法の観察

- C言語: ソースを<span style="color:red">機械語に翻訳</span>してから、翻訳結果を実行
    ```bash
    $ gcc hoge.c    #hoge.cを機械語のファイルa.outに翻訳（コンパイル）
    $ ./a.out       #CPUが実行するのはa.out
    ```
- Python: 翻訳せず、<span style="color:red">`python3`</span>がソースを読みながらその場で実行
    ```bash
    $ python3 hello.py    #CPUが実行するのはpython3、hello.pyはただのテキスト
    ```
    - このように、ソースを読んで実行するプログラム: <span style="color:red">インタプリタ</span>
    - 翻訳者（コンパイラ）と通訳者（インタプリタ）の違いと思えばよい

---

## 問題: 次のような需要が存在

- `./a.out`と同じように、`python3`をつけずに実行したい
    ```bash
    $ ./hello.py          #こう打ちたい（今はまだ動かない）
    ```
    - 理由: プログラムを使う側は、インタプリタが何かを意識したくない
        - 中身の言語でコマンドの打ち方が変わるのは不便
        - 適切なインタプリタを勝手に選んでほしい<br />　
- 可能とするには2つ作業が必要
    - 作業1: どのインタプリタで動かすかをファイルに書く（<span style="color:red">シバン</span>）
    - 作業2: Linuxにこのファイルを実行してよいと伝える（<span style="color:red">実行権限</span>）

---

## 作業1: インタプリタの指定

- <span style="color:red">shebang</span>（「シバン」と呼ぶ）: 1行目に「`#!<インタプリタのパス>`」と書く
    ```python
    #!/usr/bin/python3
                        #シバンとの間の空行は読みやすさのため
    print("hello")
    print(3.14)
    ```
    - `<>`は「必ず書く」という意味のカッコで、実際には書かない
    - `#!`の前に空白や空行を入れない（ファイルの先頭2文字が`#!`であること）
    - インタプリタはパスで指定（<span style="color:red">`which python3`</span>で調査可能）

- Linuxがシバンを読んでインタプリタを起動
    - Pythonから見るとシバンはコメント（`#`で始まる）、実行には影響しない
---

## 作業2: 実行権限の付与

- テキストファイルは作っただけでは実行できない（うっかり実行の防止）
    - 実行できるかは`ls -l`の1列目に「`x`」があるかで決まる: <span style="color:red">パーミッション</span>
    ```bash
    $ ./hello.py                           #実行してみる
    bash: ./hello.py: Permission denied    #できない（「許可がない」の意味）
    $ ls -l hello.py                       #ls -lで確認
    -rw-r--r-- 1 taro taro    47 Sep 30 20:50 hello.py   #hello.pyにはxがない
    ```
- 付与の方法: <span style="color:red">`chmod`</span>で「`x`」を追加
    ```bash
    $ chmod +x hello.py
    $ ls -l hello.py
    -rwxr-xr-x 1 taro taro 47 Sep 30 20:50 hello.py   #xがついた
    $ ./hello.py                                      #実行できる
    hello
    3.14
    ```

---

## パーミッション

- `ls -l`の1列目: そのファイルを「誰が」「何してよいか」を表す10文字
    ```bash
    $ ls -l hello.py
    -rwxr-xr-x 1 taro taro 47 Sep 30 20:50 hello.py
    ```
    ```
    -     rwx      r-x        r-x
    種類   所有者   グループ    その他の人      ←自分のファイルなら当面は所有者の3文字だけ見ればよい
    ```
    - 種類: `-`は普通のファイル、`d`はディレクトリ
- `r`: 読める、`w`: 書ける、`x`: 実行できる。`-`はその権利がない
    - 例: `-rw-r--r--` $\rightarrow$ 自分は読み書きできるが実行できない。他人は読むだけ
    - `chmod +x`は、この並びに`x`を追加する操作だった（前ページ）

---

## hello（.py）の実行

- 拡張子を除去してもよい（使う側に言語を意識させない）
    ```bash
    $ mv hello.py hello
    $ ./hello
    （略）
    ```
    - <span style="color:red">`mv`</span>: 移動、名前の変更<br />　
- 「`./`」なしで呼びたい: シェルは<span style="color:red">`PATH`</span>のディレクトリの中からコマンドを探す
    ```bash
    $ echo $PATH             #「:」区切りでディレクトリが並んでいる（/usr/bin など）
    $ PATH=$PATH:~           #末尾にホームディレクトリを追加
    $ hello                  #「パスが通った」ので、どのディレクトリにいても呼べる
    （略）
    $ which hello            #/home/taro/hello が出る
    ```
    - <span style="color:red">`echo`</span>: 引数をそのまま表示、「$変数名」で変数の値に置き換わる

---

## 練習

- 自分用のコマンドを1つ作り、パスの通ったコマンドとして実行できるようにする
    - 中身は簡単でよい（`print`、リスト、for文で十分）
    - 名前は本物のコマンドらしく: 拡張子なし、短い英単語
    - 手順: シバン $\rightarrow$ `chmod +x` $\rightarrow$ `PATH=$PATH:~` $\rightarrow$ 別のディレクトリから名前だけで実行<br />　
- 例1：`todo`: やることリストを表示するコマンド
- 例2：`cheat`: 忘れがちなコマンドのメモを示すコマンド
---

## まとめ
- 前半: Python入門
    - 変数、リスト、for文、スライス<br />　
- 後半: 
    - <span style="color:red">シバン</span>: 1行目でインタプリタを指定（Linuxが読む、Pythonにはコメント）
    - <span style="color:red">パーミッション</span>: `x`がないと実行できない $\rightarrow$ `chmod +x`
    - <span style="color:red">`PATH`</span>: ディレクトリの中からシェルがコマンドを探す<br />　
- 重要語句: メソッド、インタプリタ、シバン、パーミッション、パスが通った
- コマンド: `python3`、`which`、`mv`、`chmod`、`echo`

