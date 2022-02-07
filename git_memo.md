---
title: "git 学習"
tags: "non-label"
---

# git 操作　基本

- ローカルリポジトリの新規作成 init
- 記録 add, commit
    - git add . 
    - git commit -m 'commit messages'
- 状況確認 diff, status
    - diff 変更内容をチェック
        - git diff
    - status 変更ファイルをチェック
        - git diff --status
        - git status
- 履歴の確認 log
    - git log
- 元に戻す　restore, add
    - git restore file_name
    - git restore --staged file_name



<br/>
<br/>

## 2021/08/20学んだこと
- ソフトウエア開発では、様々な版を追跡処理するバージョン管理が必要
    - 前のバグが無い版を再利用するとき
    - どのバージョンにどのバグが入っていたかを管理するとき
    - 出荷済みソフトウエアについてメンテナンスするとき
- それぞれの関係者が、ある特定の既存バージョンに行き着くことができ、そのバージョンに含まれるファイル群を参照できなければならない





<br/>
<br/>


## 2021/08/22学んだこと
- リポジトリ　→　ファイルを保存するストレージ領域
- コミット　→　保存された変更
    - なんでも保存する価値のある変更を行った際には、必ずバージョンを改めるべきである
    - 個々のバージョンがなんであるのか把握することは、プロのソフトウエア開発者を目指す上で欠かせない初歩的なステップである
- ブランチ　→　開発における経路の一つ


### Gitを代表するバージョン管理システムの３つの共通点
- バージョニング　→　改版
- オーディティング　auditing →　監査
- ブランチング　→　分岐
### gitの３つの特徴
- 分岐されたリポジトリ
- 素早いブランチ
- ステージングエリア


### 分岐されたリポジトリ
- gitでは中心的なサーバー（リポジトリがあるような）は存在しない
- どの開発者にもリポジトリのコピーが与えられている
- バージョン管理の操作は全てローカルで行われる

### 素早いブランチ
- ソフトウエアはリポジトリにメインあるいはトランクの部分に含まれるコードで開発される
- バージョンをリリースする準備ができたら、ブランチを作る　
- このブランチはそのバージョンを表現するコードを全部コピーしたもの（スナップショット）である


### ステージングエリア
- ステージングエリアを使うと、ファイルの内、自分が提出したい部分だけを選択してコミットできるようになる


## 手順
1. リポジトリのクローニング



## 2021/09/09
- git remote # リモートのショートカット名を表示
- git remote -v # リモートのURLを表示 
- git remote add <リモート名> <リモートのURL> # GitにGitHubのリポジトリを登録する



## 2021/09/19
- git pull origin master = 
- git fetch + git merge origin/master

- git pul origin develop = 
- git fetch + git merge origin/develop

["git pull origin master" の正体](https://qiita.com/nasutaro211/items/c590994a5d5091206c08)
←最後のまとめが大事！！！
- git fetch 
	- 「リモートリポジトリとローカルリポジトリの同期するコマンド」
	- ローカルにはリモート追跡ブランチが存在する  
	- origin/masterブランチがリモートのmasterブランチの状態をそのまま表すブランチということになる
	- 注意点：追跡ブランチは存在するものの、そのままでは、リモートの状態を自動的に反映されるわけではない
	- 解決策：git fetch で追跡ブランチにリモートの状態を反映させるコマンド
	- 結果：remote/origin等追跡ブランチは、現在のリモートリポジトリの状態を忠実に表すブランチになる

- git merge origin/master
	- 現在のブランチがmasterだとする
	- 「masterブランチにリモートリポジトリのmasterブランチ(origin/master)の状態をマージするコマンド」



# 2021/09/20
- マージコマンド：
    - git fetch origin(←remote repo url) 
    - git merge origin/develop(←remote repo)
    - (= git pull develop)
- リベースコマンド:
    - git fetch origin 
    - git rebase origin/develop develop 
    - git push origin -f
- コンフリクト解消までの流れ
    - AさんとBさんが同じブランチの同じファイルの同じ行を編集した場合を想定する
    - 今回はfile1を編集したとする
    - AさんBさんともにファイルをコミットしておく
    - Aさんが先にPush（リモートリポジトリに反映させる） → （それを知って）Bさんが後からPull（リモートリポジトリとローカルリポジトリを同期）
    - コンフリクト発生
```
Enter passphrase for key '/c/Users/brosm/.ssh/id_rsa':
From github.com:akihiro-coder/git_lesson
 * branch            develop    -> FETCH_HEAD
Auto-merging file1.txt
CONFLICT (content): Merge conflict in file1.txt
Automatic merge failed; fix conflicts and then commit the result.
```


- （続き）
    - Bさんはファイルを開く
    - ギザギザマークがある。また、BさんとAさんのそれぞれの編集内容が並ぶ。
    - ギザギザマークなど、変更部分のコード以外を消す。
    - Bさんは自分のコードを消して、Aさんのコードの上に自分が編集内容を書く。
    - BさんはコミットしてPushする。
    - Bさんの編集内容をリモートリポジトリに反映できた。
- git stashコマンド
    - git stash -> コミットしていないファイルの変更を消す
    - git stash pop -> 消した変更をもとに戻す
    - いつ役立つか？？
        - 自分が想定していない違うブランチで開発をしていたとき
    - 実際にどう使うのか？？
        1. ファイルを編集 
        2. 違うブランチで編集してた！ということに気がつく
        3. コミットしない
        4. ここで、git stash → 一度メモリに格納される
        5. git checkout <ブランチ名>
        6. 本来のブランチに移動できたら、git stash popコマンド
        7. 本来のブランチの該当ファイルに自分の編集が反映される



# 2021/09/21
- リモートリポジトリのクローン
    - git clone [url]
    - git pull origin master
    - git remote add origin [url]

- リモートにpushするまで
    - git add .
    - git commit -m "messages"
    - git push origin master(or develop)

- ローカルを最新にしたいとき
    - git pull origin master(or develop)

- 開発者同士で同じブランチを使って開発をしてpushの時間差でどちらかにエラー（reject）が起きたとき
    - git fetch origin 
    - git merge develop
    - git push origin develop








# 2021/10/02（全体復習）
[【Git入門】Git + Github使い方入門講座🐒Gitの仕組みや使い方を完全解説！パーフェクトGit入門！](https://www.youtube.com/watch?v=LDOR5HfI_sQ&t=1728s)
- git status 
    - ステージングの状態を見る
    - ステージングされていないと赤色
    - ステージングされると緑色
- git add file名
    - git管理下に置くステージング操作
    - git statusコマンドでファイルは緑色に変化する
- git commit -m "messages"
    - gitに保存する行為 
- git push origin master 
    - gitのリモートサーバーに対してアップロード（Push）する
- git clone [url]
    - 新規開発者がリモートサーバの最新バージョンをローカルサーバーに保存したいとき
    - 新規にリポジトリをローカル側にコピーする時に使用するコマンド
- git pull origin master
    - リポジトリにある最新の情報の取得するとき
- ここで、新人登場！
    - 新人がそのまま間違っているかもしれないコードをpushしないように、ブランチを切る必要がある（他の歴史を作る）
- git checkout -b develop master 
    - 新人のpcローカルで、masterブランチからdevelopブランチを切って、developブランチへ移動
- git checkout master 
    - developブランチからmasterブランチへ切り替え
- git checkout develop
    - masterブランチからdevelopブランチへ切り替え
- git push origin develop 
    - developをリモートサーバーにPush
    - リモートサーバーのmasterブランチ（本番環境）には影響をこの段階では与えない
    - gitgunサイトで手動でブランチを切り替えることが出来る
- 新人がリモートにpushしたら(developブランチで)、ベテランプログラマーは自身の手元にdevelopブランチを持ってきて、新人が書いたコードが正しいか判断する
- git pull origin 
    - ベテランプログラマーはgit pull originコマンドでdevelopブランチを手元に取得する
- git checkout master 
- git merge develop
    - masterブランチに対してdevelopブランチをマージしたいときの操作
    - ブランチの切り替え
    - マージ



- ここで、新人が新たなタスクをもらう（ここでは先輩社員が手元にPullしてレビューしなくてもいい他の方法を試す）
- タスクを別のブランチを切って開発を行い、プルリクエストを行う
    - プルリクエストとは、先輩社員に対して開発に使ったブランチをｍasterにマージしてくださいとお願いすること
- 新人はプルリクエスト画面のURLを先輩社員に渡して、実際にプルリクエストをレビューしてもらう
- 先輩はコメントを残すなどしてレビューを行い、新人とやり取りを行う中で、最終的にオーケーだと思ったら、先輩はMerge pull Requestボタン（緑色）を押す



- 次にAさんとBさんが同じブランチ(今回はdevelopブランチとする)で開発作業をする場合を考えてみる
- 別のコミット履歴がそれぞれにローカルに残されていく
- 先にAさんがコミット（Push）したあとに、Bさんがコミットするとエラーを以下のような吐く
```
$ git push origin develop
Enter passphrase for key '/c/Users/brosm/.ssh/id_rsa': 
To github.com:akihiro-coder/git_lesson-.git
 ! [rejected]        develop -> develop (fetch first)
error: failed to push some refs to 'github.com:akihiro-coder/git_lesson-.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```
- Bさんはどうすればいいのか
- 答えはリモートから最新の情報を取ってきて、自分の手元にマージを行う
    - リモートのdevelopブランチをBさんのローカルdevelopブランチにマージを行う
- git fetch origin
- git merge origin/develop
    - 手元のdevelopブランチにリモートのorigin/developブランチをマージした
- git pull develop（上２つのショートカット）
- git push origin develop
    - 最後に、リモートにAさんの変更をマージしたローカルブランチをPushする



- 次は、「コンフリクト」
    - コンフリクトとは、複数人が同じブランチの同じファイルの同じ行に対して編集が行われると発生する問題
- この章は分からなくなったら、もう一度ビデオを見ることにする


- resetコマンドについて
- git reset --hard HEAD
    - 最後のコミットまで戻ってくれるコマンド

- stash これも詳しくはビデオを再度参照すること
    - git stash 
        - メモリに格納
    - git checkout 他のブランチ名
        - ブランチに切り替え
    - git stash pop 
        - 格納されていた編集を蘇らせる
