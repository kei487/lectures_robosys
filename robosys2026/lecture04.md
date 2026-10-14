---
marp: true
---
<!--
style: |
  section {
    line-height: 1.15;
  }
-->

<!-- footer: "ロボットシステム学第4回" -->

# ロボットシステム学

## 第4回: GitとGitHub

鈴木 太郎（千葉工業大学）

<span style="font-size:70%">オリジナル: 上田 隆一（千葉工業大学）[ロボットシステム学 2025](https://github.com/ryuichiueda/slides_marp/tree/master/robosys2025) を改変</span>

<br />

<span style="font-size:70%">This work is licensed under a </span>[<span style="font-size:70%">Creative Commons Attribution-ShareAlike 4.0 International License</span>](https://creativecommons.org/licenses/by-sa/4.0/).
![](https://i.creativecommons.org/l/by-sa/4.0/88x31.png)

---

<!-- paginate: true -->

## 本日の内容

- GitとGitHubを使う<br />　
1. 概要
2. 準備
3. GitHubへのコードの保存
4. ブランチ
5. コンフリクト
6. まとめ
- 付録: その他のリポジトリ操作（授業では扱わない）

---

## 1. 概要：Gitとは？

<img src="figs/git_logo.png" style="position:absolute; top:60px; right:80px; width:110px" />

- <span style="color:red">版管理（バージョン管理）システム</span>
    - ファイルの変更履歴を記録して管理する
    - コードや文章を書くときは必須と言っても過言ではない<br />　
- 履歴があると何ができるか
    - いつ，誰が，何を変えたかが分かる
    - 壊してしまったコードを動いていた時点に戻せる
    - `report_v2_最新_修正.docx`のような名前でのコピーが不要に<br />　
- どこで使われているか
    - ソフトウェア開発の現場ではほぼ標準
    - Linuxカーネル，Python，ROS 2もGitで開発
    - 研究室の卒論コードや論文の管理にも

---

## 1. 概要：GitHubとは？

<img src="figs/github_logo.png" style="position:absolute; top:50px; right:80px; width:120px" />

- Gitの<span style="color:red">リポジトリ</span>を置くためのウェブサービス
    - リポジトリ: あるソフトウェアに関するファイルと履歴の集まり
    - 手元のPC（ローカル）とGitHub（リモート）の両方に同じ履歴を持つ<br />　
- GitHubでできること
    - コードのバックアップと公開（非公開も可）
    - 他の人との共同開発，バグ報告や議論
    - 書いたコードがそのまま自分の実績に<br />　
- 本講義での使い方
    - 自分のコードをGitHubに置きながら開発
    - ROS 2のパッケージも多くがGitHubで公開されている

---

## 1. 概要：GitとGitHubの連携

- 役割分担
    - Git: 手元のPCで履歴を記録する<span style="color:red">道具</span>（GitHubがなくても使える）
    - GitHub: 記録した履歴を預かる<span style="color:red">場所</span>（Gitで操作する）

<div style="display:flex; justify-content:center; align-items:center; gap:30px; margin:15px 0; font-size:70%; line-height:1.3">
<div style="text-align:center"><img src="figs/git_logo.png" width="60" /><br />自分のPC<br />（ローカルリポジトリ）</div>
<div style="text-align:center">push →<br />← pull</div>
<div style="text-align:center"><img src="figs/github_logo.png" width="60" /><br />GitHub<br />（リモートリポジトリ）</div>
<div style="text-align:center">clone →<br />← push</div>
<div style="text-align:center"><img src="figs/git_logo.png" width="60" /><br />研究室のPC<br />（ローカルリポジトリ）</div>
</div>

- 使い方の例
    1. 自分のPCで`plus_stdin`を書き，Gitで記録（<span style="color:red">`commit`</span>）
    2. 記録をGitHubに送る（<span style="color:red">`push`</span>）$\rightarrow$消えない，人に見せられる
    3. 研究室のPCでGitHubから取り出して（<span style="color:red">`clone`</span>）続きを書く
    4. 研究室で`push`した記録を自分のPCに取り込む（<span style="color:red">`pull`</span>）

---

## 2. 準備

---

## Gitのインストール

1. （`git`と打って「ない」と言われたら）インストール
    - `sudo apt install git`
2. ユーザの設定
    ```bash
    ####自身の名前とe-mail アドレスを記録しておく###
    $ git config --global user.name "Taro Suzuki"
    $ git config --global user.email "taroz1461@gmail.com"
    ####pullのやり方を決めておく（後半のコンフリクトの演習で必要）###
    $ git config --global pull.rebase false
    
    ####確認###
    $ cat ~/.gitconfig
    [user]
    name = Taro Suzuki
    email = taroz1461@gmail.com
    [pull]
    rebase = false
    ```

---

## GitHubの利用方法

- 操作方法
    - ウェブサイト（[github.com](https://github.com/)）: アカウント・リポジトリの作成，閲覧
    - コマンドライン: `git`コマンド（`gh`コマンドは本講義では扱わず）<br />　
- 手元のPCとの通信には<span style="color:red">SSH</span>を使う（後のページで鍵を設定）
    - HTTPSでも可能だがパスワードの代わりにトークンが必要で面倒<br />　
- 知っておくとよいこと
    - 無料プラン（Free）で公開・非公開リポジトリとも作り放題
    - 学生はGitHub Educationに申請すると特典（Copilot等）が使える
    - 似たサービス: GitLab，Bitbucket

---

## GitHubでのアカウント作成

注意: 文言等はよく変更されるので，基本的にサイトの英語を読んで手続きを

1. トップページ右上の"Sign up"を押す
2. メールアドレス，パスワード，ユーザ名，国を入力して"Continue"
    - ユーザ名は恥ずかしくないものを！（全世界に公開されます）
3. "Create account"
4. 登録したメールアドレスに届いたコードを画面に入力
5. 鍵の登録（次ページ）

- 後日，2段階認証（2FA）の設定を求められる$\rightarrow$スマホの認証アプリで設定

---

## 鍵の設定（鍵の作成）

- GitHubに「自分のPCからの接続だ」と確認してもらうための鍵を作る
    - <span style="color:red">公開鍵</span>: GitHubに登録する（人に見せてよい）
    - <span style="color:red">秘密鍵</span>: 手元のPCに置いておく
        - 文字通り秘密にして他人に見せたり触れたりさせない
- 鍵の作り方
    ```bash
    $ ssh-keygen -t ed25519
    （いろいろ聞かれるけどすべてEnterで大丈夫）
    $ ls ~/.ssh/                        #確認
    id_ed25519  id_ed25519.pub          #この2つのファイルがあれば大丈夫
    ```
    - `.pub`が付いている方が公開鍵<br />　
- 参考: 鍵・暗号については過去の上田先生資料を参照のこと
    - https://lab.ueda.tech/?page=robosys_2016_12

---

## 鍵の設定（GitHubでの作業）

- 公開鍵の中身を表示してコピー
    ```bash
    $ cat ~/.ssh/id_ed25519.pub
    ssh-ed25519 AAAAC3Nza（略）  #この1行全部をマウスで選択してコピー
    ```
- GitHubに登録
    - 右上のアイコン$\rightarrow$Settings$\rightarrow$SSH and GPG keys$\rightarrow$New SSH key
    - Title: 鍵の名前（なんでもよい．例: 「WSL」「Ubuntu Note」）
    - Key: コピーした公開鍵を貼り付け$\rightarrow$"Add SSH key"
- 動作確認
    ```bash
    $ ssh -T git@github.com
    （初回は"Are you sure you want to continue connecting?"と聞かれるのでyes）
    Hi <ユーザ名>! You've successfully authenticated, but GitHub does not provide shell access.
    ```

---

## 3. GitHubへのコードの保存

- やること
    - 第3回で作った`plus_stdin`をGitHubにアップロード
    - コードを消しちゃった人は作り直しましょう（`chmod +x`も忘れずに）
        ```python
        #!/usr/bin/python3
        import sys

        x = 0.0
        for line in sys.stdin:
            x += float(line)

        print(x)
        ```

---

## リポジトリの作成

- GitHubのサイトでの操作
    - 右上の「+」を押して"New repository"を選ぶ
    - 必要事項を記入
        - Repository name: `robosys2026`
        - Description: 説明を適当に（空でもよい）
        - `Public` のままに
        - "Add a README file" にチェック
        - .gitignore は `No .gitignore`，License も `No license`（別の回で追加）
    - "Create repository" ボタンを押す<br />　
$\Longrightarrow$`README.md`がひとつ存在したリポジトリができる

---

## リポジトリを手元にコピー: git clone

- リポジトリの画面で緑の"Code"を押し，<span style="color:red">"SSH"</span>を選んでURLをコピー
    - `git@github.com:<ユーザ名>/robosys2026.git`の形
        - `https://`で始まっていたらHTTPSを選んでいるので選び直し<br />　
- ホームディレクトリで次の操作（`git clone`：<span style="color:red">クローン</span>と言う）
    ```bash
    $ cd ~
    $ git clone git@github.com:<ユーザ名>/robosys2026.git   #さっきコピーしたURL
    Cloning into 'robosys2026'...
    （略）
    Receiving objects: 100% (3/3), done.
    $ cd robosys2026/
    $ ls -a
    .  ..  .git  README.md    #.gitの中に履歴が入る（消すと履歴も消える）
    ```
    - <span style="color:red">注意: </span>鍵の設定が失敗しているとエラー$\rightarrow$鍵のページの動作確認に戻る

---

## リポジトリにコードを追加1: git add

- ホームにある`plus_stdin`をリポジトリにコピー
    ```bash
    $ cp ~/plus_stdin .      #~/robosys2026の中で実行
    $ ls
    README.md  plus_stdin
    ```
- <span style="color:red">`git add`</span>で記録の対象として選択
    - <span style="color:red">ステージングエリア</span>というところに記録される
        ```bash
        $ git add plus_stdin
        $ git status             #ステージングエリアの確認
        On branch main
        Your branch is up to date with 'origin/main'.

        Changes to be committed:
          (use "git restore --staged <file>..." to unstage)
                new file:   plus_stdin
        ```

---

## リポジトリにコードを追加2: git commit

- <span style="color:red">`git commit`</span>でステージングエリアの情報をリポジトリに反映
    - この時点で，手元のリポジトリに`plus_stdin`の記録が残る
    - `git commit`で作った1つの記録を<span style="color:red">コミット</span>と呼ぶ<br />　
        ```bash
        $ git commit -m "Add a command" #git commit -m "何をしたか短く"
        [main fa8aab8] Add a command
         1 file changed, 8 insertions(+)
         create mode 100755 plus_stdin  
        $ git log -n 1                 #最新のコミット1件を表示
        commit fa8aab8a2ade8cd33823f488fbb1bbec6d981260 (HEAD -> main)
        Author: Taro Suzuki <taroz1461@gmail.com>
        Date:   Wed Oct 14 16:58:54 2026 +0900

            Add a command
        ```

---

## GitHubへの反映


- 手元（<span style="color:red">ローカルリポジトリ</span>）からGitHub（<span style="color:red">リモートリポジトリ</span>）へ転送
    - <span style="color:red">プッシュ</span>と呼ぶ
    - コマンドは<span style="color:red">`git push`</span><br />　
        ```bash
        $ git push   #cloneしたリポジトリならこれだけでよい
        Enumerating objects: 4, done.
        Counting objects: 100% (4/4), done.
        Delta compression using up to 16 threads
        Compressing objects: 100% (3/3), done.
        Writing objects: 100% (3/3), 373 bytes | 373.00 KiB/s, done.
        Total 3 (delta 0), reused 0 (delta 0)
        To github.com:<ユーザ名>/robosys2026.git
           68d342f..fa8aab8  main -> main
        ```
    - ブラウザでリポジトリを再読み込み$\rightarrow$`plus_stdin`が追加されているのを確認

---

## 4. ブランチ

---

## GitHubを利用した開発

- GitHubにコードをアップした時点で様々な利点
    - 自分のコードを紛失する可能性が極めて低く
    - 混乱せずに様々な環境で開発可能に
    - 自分の力を見せることが可能に
        - 世の中的にはコードが書けるだけで少数派<br />　
- 面倒なこと: 少々責任が伴う
    - ライセンス等の整備（また別の回で）
    - <span style="color:red">使えないものを使えると言って置かない</span>
        - 他の人が使うかもしれない$\rightarrow$公開している場所は動く状態に保つ

---

## 動くものを残しながらの開発

- よくあるケース
    - 改良しようと結構手を加えたらコードが動かなくなった　
- どうする？
    - そのままGitHubにpushすると他の人がコードを使えなくなる
    - GitHubにpushしないで放置すると作業の記録が残せない
    - `plus_stdin_new`のように別名でコピーすると，ファイルが増えて混乱<br />　
<span style="color:red">$\Rightarrow$ブランチを分ける</span>

---

## ブランチ

- リポジトリの内容を枝分かれして開発を進める
    - ブランチ = 枝
- 今のところブランチは「main」だけ
    ```bash
    $ git branch
    * main          #ブランチはmainだけ．「*」が今いるブランチ
    ```
    - GitHubはmainブランチを優先して表示するのでここでの雑な開発は避けたい<br />　
- 開発用ブランチを作る
    ```bash
    $ git switch -c dev     #git checkout -b devでも可
    Switched to a new branch 'dev'
    $ git branch
    * dev               #devブランチができて，devに移動した
      main
    ```

---

## devブランチでの開発（その1）

- `plus_stdin`を改良: 整数入力は整数のまま足す（`15.0`ではなく`15`と出したい）
    - <span style="color:red">例外処理</span>をしてみましょう
        - 失敗しそうな処理を<span style="color:red">`try`</span>で囲む
        - 下に<span style="color:red">`except`</span>のブロックを作り，失敗したときの処理を書く
            ```python
            #!/usr/bin/python3
            import sys

            x = 0                     #もともと0.0だったのを0に変更
            for line in sys.stdin:
                try:
                    x += int(line)    #intは文字列を整数に（失敗すると例外発生）
                except ValueError:    #第3回で見たエラーの名前
                    x += float(line)

            print(x)
            ```

---

## devブランチでの開発（その2）

- 検証とコミット（とプッシュ）
    ```bash
    ###動作確認###
    $ seq 5 | ./plus_stdin
    15                        #整数として処理されていることを確認
    $ echo 0.5 | ./plus_stdin
    0.5                       #小数も計算できることを確認
    ###バグがないことを確認したらコミット###
    $ git add -A              #-Aで変更を全部ステージングできる
    $ git status              #On branch devとmodified: plus_stdinが出ればOK
    $ git commit -m "Support integer calculation"
    [dev f02a202] Support integer calculation
     1 file changed, 5 insertions(+), 2 deletions(-)
    ###GitHubにもプッシュ（必須ではないが作業途中のバックアップになる）###
    $ git push --set-upstream origin dev   #origin: GitHubにあるリポジトリのこと
    （略）
    branch 'dev' set up to track 'origin/dev'.
    ```

---

## （寄り道）ブランチの観察

- `git log --graph --oneline`で履歴を1行ずつ表示してみましょう
    ```
    $ git log --graph --oneline
    * f02a202 (HEAD -> dev, origin/dev) Support integer calculation
    * fa8aab8 (origin/main, origin/HEAD, main) Add a command
    * 68d342f Initial commit
    ```
    - ポイント
        - 各コミットには`f02a202`のような識別記号$\rightarrow$<span style="color:red">コミットハッシュ値</span>
            - 本当は40桁．先頭の数桁で指定できる（付録参照）
        - ()の中に情報
            - `HEAD`: 今いる場所（今のディレクトリの内容はこのコミット）
            - `origin/<ブランチ名>`: GitHubのリポジトリのブランチ
        - devはmainより1つ先に進んでいる$\rightarrow$次でmainをdevに追いつかせる

---

## devブランチでの開発（その3）: git merge

- mainへの<span style="color:red">マージ</span>とGitHubへのプッシュ
    - まずmainブランチに戻って変更内容の確認
        ```bash
        $ git switch main        #git checkout mainでも可
        $ cat plus_stdin
        （略．try...exceptのない古いコードに戻っている！）
        $ git diff main dev
        （略．mainとdevのコードの違いが表示される）
        ```
    - mainにdevの中身をマージ（併合）してGitHubに反映
        ```bash
        $ git merge dev          #下の出力には省略あり
         1 file changed, 5 insertions(+), 2 deletions(-)
        $ cat plus_stdin
        （略．try...exceptの入ったコードに変わった）
        $ git push
        To github.com:<ユーザ名>/robosys2026.git
           fa8aab8..f02a202  main -> main
        ```

---

## 5. コンフリクト

- Gitを使っていると，コミット同士が矛盾することがある
    - 例: 自分のPCと研究室のPCで同じ行を別々に直してしまった
    - そのままではマージできない$\rightarrow$<span style="color:red">コンフリクト</span>

そういう状況をわざと作ってみましょう．

---

## コンフリクトを起こす（その1）

- 数字の処理を関数にしてコミット，<span style="color:red">push</span>（まだコンフリクトは起きない）
    - ついでなのでPythonの関数も勉強します
        ```python
        #!/usr/bin/python3
        import sys
        #本来は関数の前後に空行を2行あける（スライドの都合で省略）
        def tonum(s):   #def 関数の名前(引数)で関数を定義
            try:
                return int(s)
            except ValueError:
                return float(s)

        x = 0
        for line in sys.stdin:
            x += tonum(line)

        print(x)
        ```

---

## コンフリクトを起こす（その2）

- GitHubのサイト上で直接編集
    - 研究室のPCなど，別の場所で作業したという想定
- 手順
    1. リポジトリの画面で`plus_stdin`をクリック
    2. 右上の鉛筆のアイコン（"Edit this file"）を押す
    3. 次の1行を書き換える
        ```python
            x += tonum(line.rstrip())   #line.rstrip()で末尾の改行を除去
        ```
    4. 右上の"Commit changes..."を押す
        - コミットメッセージを入れて"Commit changes"
- GitHubのリポジトリの履歴にコミットが1つ増える

---

## コンフリクトを起こす（その3）

- 手元のPCで同じ問題を別の書き方で直す
    - GitHub上で直したことを忘れたという想定
        ```python
       （略）
        for line in sys.stdin:
            line = line.rstrip()    #for文の下にこの行を挿入
            x += tonum(line)
       （以下略）
        ```
    - コミットしてpushするとエラー
        ```bash
        $ git push
        To github.com:<ユーザ名>/robosys2026
         ! [rejected]        main -> main (non-fast-forward)
        error: failed to push some refs to 'git@github.com:<ユーザ名>/robosys2026'
        hint: Updates were rejected because the tip of your current branch is behind
        （略）
        ```

<center>GitHub側に自分の知らないコミットがあるとpushできない</center>

---

## コンフリクトの解消（その1）

- まず`git pull`でGitHub側のコミットを取り込む
    ```bash
    $ git pull
    CONFLICT (content): Merge conflict in plus_stdin
    Automatic merge failed; fix conflicts and then commit the result.
    ```
    - 「`CONFLICT`」と出るが`pull`は完了
        - `plus_stdin`を開くと両方の直し方が並んでいる
            ```python
            for line in sys.stdin:
            <<<<<<< HEAD                #手元の直し方
                line = line.rstrip()
                x += tonum(line)
            =======                     #GitHub側の直し方
                x += tonum(line.rstrip())
            >>>>>>> de49ed093dcd9de91e2bf1bb5bc7203444547e5b
            ```

---

## コンフリクトの解消（その2）

- どちらの直し方にするか人間が決めて，`<<<<<<<`から`>>>>>>>`までを書き直す
    ```python
    （略）
    for line in sys.stdin:
        x += tonum(line.rstrip())  #今回はGitHub側を採用

    print(x)
    ```
- 直したらコミットしてpush
    ```bash
    $ git add plus_stdin
    $ git commit -m "Fix conflict"
    $ git push
    ```
    - GitHubの画面でも反映を確認

<center>コンフリクトは「どちらを残すか」を人間が決めれば解消できる</center>

---

## 6. まとめ

- Git/GitHub 
    - バージョン管理システム/サービス
        - 今日から必須の道具
            - レポート等も管理することを強く推奨
    - 他にも様々な操作が必要に
        - 困ったら仕組みから理解してみましょう　
- Pythonの文法
    - 今回出てきたもの
        - 例外処理
        - 関数
    - 分からなくなったら戻ってくること

---

## 付録: その他のリポジトリ操作

- 授業では扱いませんが、必要になったら読んで試してみてください

---

## 過去のコードの取り出し（動機）

- 昔のコードを一部復活させたいときにやりたくなる
    - 例: 次の履歴から「Add a command」時点の`plus_stdin`を取り出したい
        ```bash
        $ git log --oneline
        （略）
        f02a202 Support integer calculation
        fa8aab8 Add a command            #この時点のplus_stdinが欲しい
        68d342f Initial commit
        ```
    - コミットハッシュ値（`fa8aab8`）で時点を指定できる

---

## 過去のコードの取り出し（方法）

- 取り出すだけなら次の方法で可能
    ```bash
    $ git switch -d fa8aab8         #コミットハッシュ値の先頭何桁かを指定
    HEAD is now at fa8aab8 Add a command
    $ git branch
    * (HEAD detached at fa8aab8)    #使い捨てのブランチができる
      dev
      main
    $ cat plus_stdin
    （略．try，exceptを加える前のコード）
    $ cp plus_stdin /tmp/           #必要ならコードのコピーをとる
    $ git switch -                  #元のブランチに戻る
    Switched to branch 'main'
    ```

---

## 手元で作ったリポジトリをGitHubにアップ

- 先にコードがあるとき（今日のGitHub先行の手順とは逆順）
    ```bash
    $ cd ~/myprog               #コードのあるディレクトリで
    $ git init                  #.gitができてローカルリポジトリになる
    $ git add -A
    $ git commit -m "First commit"
    ```
- GitHubで同名の空のリポジトリを作る（READMEは<span style="color:red">作らない</span>）
    ```bash
    $ git remote add origin git@github.com:<ユーザ名>/myprog.git  #結びつけ
    $ git push -u origin main   #-u: 次回からgit pushだけでよい
    ```
- 注意: `git init`後のブランチが`master`なら`git branch -M main`で改名

---

## リポジトリの名前を変えたい

- GitHub側: リポジトリのSettings$\rightarrow$Repository nameを変えて"Rename"
    - 古い名前へのアクセスは新しい名前に転送される
- ローカル:     - ディレクトリ名は`mv robosys2026 newname`などで変更
    - 結びつけ先のURLを新しい名前に変える<br />　
    ```bash
    $ git remote -v                   #今の結びつけ先を確認
    origin  git@github.com:<ユーザ名>/robosys2026.git (fetch)
    origin  git@github.com:<ユーザ名>/robosys2026.git (push)
    $ git remote set-url origin git@github.com:<ユーザ名>/newname.git
    ```
    - 変えなくても転送でpushできるが，混乱のもとなので変えておく
