# チーム開発 基礎

チーム開発（基礎）へようこそ！

## サンプルアプリの URL

チーム開発（基礎）で作成するアプリのサンプルはこちらです。

- [https://joint-development-project.herokuapp.com/users](https://joint-development-project.herokuapp.com/users)


## 開発環境

- Ruby 2.7.3
- PostgreSQL

Rails, PostgreSQL の環境構築が済まされていることが前提となります。まだの方は，逆転教材を参考に環境構築をお願いいたします。

- [テキスト教材](https://www.yanbaru-code.com/texts)
- [動画教材](https://www.yanbaru-code.com/?page=2)

### 開発環境構築(Ruby)

- Ruby 2.7.3 をインストールされていない場合は，以下を参考にして下さい。
  - 「インストール済みの Ruby」と「使用中のバージョン」は `rbenv versions` で確認できます
  - Ruby のバージョンごとに gem をインストールする必要があります

[参考] https://www.yanbaru-code.com/questions/39

```
cd
brew update
brew upgrade rbenv ruby-build
rbenv install 2.7.3
rbenv global 2.7.3
gem install rails
```

VSCode の教材で環境構築を行い，拡張機能を利用されている場合は以下も実行してください。

```
gem install solargraph
gem install htmlbeautifier
gem install rufo
```

【補足】もし Ruby のデフォルトバージョンを変更したくない方は，gem のインストール後に `rbenv global 指定したいバージョン` を実行して下さい。

### ブランチモデル

| ブランチ名     | 目的       | 備考                           |
| -------------- | ---------- | ------------------------------ |
| master         | リリース用 | main ではない                  |
| feature/\*\*\* | 機能実装用 | 派生元は master ブランチとする |

## 1. 開発環境構築(Rails アプリ)

チーム開発（基礎）で作成するアプリは，「ユーザー・メッセージ・タスク」の 3 つの CRUD で実装できます。

3 名で 1 つずつ担当していただきます。まずは，以下の手順で準備を行って下さい。

- ターミナルを開き，チーム開発用のアプリを入れたいディレクトリまで `cd` コマンドで移動
  - 特にこだわりがなければ次を実行すればよいでしょう

```zsh
cd ~/rails
```

- この GitHub リポジトリをクローン

```zsh
git clone リポジトリURL
```

- 作成された Rails アプリのディレクトリに `cd` コマンドで移動

- 次の 3 コマンドを順番に実行

```bash
bundle install
yarn install --check-files
rails db:create
```

- ブランチを切る（ブランチを作成して移動する）
  - 担当タスクに応じたものを 1 つだけ選んで実行して下さい

```bash
git checkout -b feature/crud-for-tasks
git checkout -b feature/crud-for-messages
git checkout -b feature/crud-for-users
```

これで，開発準備が完了です。

なお，クローンをした時点で Git の `ローカルリポジトリ` が作成され，リモート名 `origin` も登録されております。次のコマンドは**_不要_**です。

```bash
# 次は実行不要
git init
git remote add origin リポジトリURL
```

## 2. 実装

- テキスト教材「resources を使った CRUD 処理の実装」を参考に，担当箇所の CRUD 処理を全て実装して下さい。

  - 「メッセージ」が担当ならば，「メッセージ」の新規投稿・一覧表示・詳細表示・編集・削除機能を全て実装して下さい

- このチーム開発（基礎）では，コミットのタイミングを指定させていただきます。次のタイミングで**_計 7 回_**コミットを行って下さい。
  - プッシュは最後に 1 回行うだけで OK です
  - 事情により回数が増減した場合は，レビュー依頼時にその旨を報告して下さい。

1. `rails g model` コマンドで「マイグレーションファイル」と「モデル」を作成し，マイグレーションを行う
2. `rails g controller` コマンドで「コントローラ」を作成し，ルーティングを`resources`で定義
3. 新規投稿機能 `new, create` を実装（ストロングパラメータを利用）
4. 一覧表示機能 `index` を実装（【注意】サンプルアプリのように 1 つのカラムのみ表示して下さい）
5. 詳細表示機能 `show` を実装（【注意】こちらは両方のカラムを表示するようにして下さい）
6. 編集機能 `edit, update` を実装
7. 削除機能 `destroy` を実装

- コミットメッセージは「変更内容を誰が見ても分かるよう簡潔に」書きましょう。日本語で OK です。

- Rails サーバーを起動して，動作確認を行いましょう

## 3. 実装完了後

- GitHub にプッシュ

```bash
git push origin HEAD
```

- プルリクを出す

  - プルリクを出す際，「タイトル」と「内容」を書くようにして下さい。日本語で OK です。
  - 「内容」ではサンプル定型文に沿って記載して下さい。Markdown 形式で記述できます。不要な文言は削除して下さい。
  - コードの差分（変更内容）はプルリクの `Files changed` タブで確認できます。何を実装したかが伝わりやすいようにまとめましょう。
  - 「レビューをお願いします」などの文言を GitHub に書く必要はありません

- Slack にレビュー依頼のメッセージを投稿
  - 必ず，プルリクの URL を貼るようにして下さい
  - URL は `Conversation` の方を貼り付けて下さい。差分 `Files changed` の方を貼らないようにして下さい。

【注意】「コンフリクト」が出ていないか必ず確認をして下さい。コンフリクトが出ている場合は，解消をしてからレビュー依頼して下さい。

プルリクのページの一番下に次のような表示が出ていれば，コンフリクトが起きています。

```
This branch has conflicts that must be resolved

Conflicting files

config/routes.rb
db/schema.rb
```

## 4. コンフリクトの解消手順

### 4.0 コンフリクトとは

マージした際，同じ箇所が変更されているときに起こるのが「コンフリクト（競合）」です。

【例】

1 行目に「おはよう」と書かれている「sample.txt」があるとします。

A さんと B さんがブランチを切り，チーム開発を行うとします。

- A さんは「おはよう」を「こんにちは」に変更

- B さんは「おはよう」を「こんばんは」に変更

このとき，A さんが master ブランチをマージした後，B さんがマージした場合，Git は「おはよう」を「こんにちは」にすべきか「こんばんは」にすべきかが分かりません。

そのため「コンフリクト」が起こります。

### 4.1 コンフリクトが起きているかどうかの確認

チーム開発（基礎）でコンフリクトが起こる可能性があるのは，次の 2 つのファイルです。

- `config/routes.rb`
- `db/schema.rb`

【注意】コンフリクトの解消は，**GitHub から行わないで下さい**。以下の手順で解消して下さい。

### 4.2 ローカルで master ブランチをマージ

（コミットがまだであれば，先に行ってください）

```bash
# 現在のブランチが作業ブランチになっているか確認

git branch

# フェッチ（リモートブランチを取り込む）

git fetch

# リモートの master ブランチをマージ

git merge origin/master
```

### 4.3 コンフリクトの確認

`git status` を実行した際に， `both added: ファイル名`と書かれているファイルがコンフリクトしています

コンフリクトが起きているファイルをエディタで開きましょう。

`<<<<<<<`, `=======`, `>>>>>>>`が追加されている箇所でコンフリクトしています。

master ブランチをマージした場合は，`======`の下側が元の内容です。それに自分の変更を加えるのが原則です。

（単純に「両方残せばよい」「片方を削ればよい」という話ではありません）

### 4.4 コンフリクトの解消

- `config/routes.rb`

両方残すように修正しましょう。`<<<<<<<`, `=======` などが含まれる行は削除しましょう。

- `db/schema.rb`

このファイルは，マイグレーションを行った際に自動的に編集されるファイルです。エディタで直接編集するのは望ましくありません。コマンドで対応しましょう。

```
# データベースを作成し直し，マイグレーションを実行
# データベースのデータが全て消えますので，一般にはマイグレーションを理解した対応が必要です
rails db:migrate:reset
```

### 4.5 マージを完了させる

- `git diff` で差分を確認

  - `<<<<<<<`, `=======`などが残っていないか確認

- サーバーを起動して動作確認を行う

- コンフリクトの解消後，コミットを行う必要があります。その後，プッシュしましょう。

```
git add .
git commit

# vimが起動します。コミットメッセージはデフォルトでよいので，shiftキーを押しながら「z」を2回

git push origin HEAD
```

## 5. レビュー後の修正手順

- 該当箇所をエディタで修正

- コミット・プッシュ

  - 何をどう修正したのかが分かるようにコミットメッセージを書きましょう。「修正」のようないい加減なメッセージを書かないこと。

- Slack にレビュー依頼のメッセージを投稿
  - 必ず，プルリクの URL を貼るようにして下さい
  - 前回のプルリクをそのまま使用できます。プルリクを新しく出さないで下さい。

## 6. アプリ完成後

全員のタスクが完了し，アプリが完成したらマージして動作確認をしましょう。

```bash
git checkout master
git pull origin HEAD
rails db:migrate
rails s
```

これでチーム開発（基礎）は完了です。お疲れさまでした！
