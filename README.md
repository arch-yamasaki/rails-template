# Rails + postgres + docker-compose テンプレ

## 概要
localでRailsを立ち上げるテンプレ用Git Repositoryを作成した。


## セットアップ方法

全体のセットアップ方法について記載。以下の流れで行う。

1. リポジトリのクローン
2. 一部ファイルを編集
3. docker上でのrailsのインストール
4. DBのセットアップ

### 1. リポジトリをcloneする

```sh

git clone git@github.com:arch-yamasaki/rails-template.git
cd rails-template

```


### 2. 一部ファイルの編集


- `Gemfile.sample`の名前を`Gemfile`に変更
- `.gitignore`の以下の部分を消す。

```txt

# <-- Start : Please remove this file -->
**/*
!.env.sample
!.gitignore
!docker-compose.yml
!*Dockerfile*
!Gemfile
# <-- End -->

```

### 3. railsのセットアップ

```sh
# イメージを作成
docker-compose build

# rails含む各種Gemをisntallする
docker-compose run web rails new . rails_twitter \
  --database=postgresql \
  --css=bootstrap \
  --skip-git \
  --skip-jbuilder \
  --skip-action-mailbox \
  --skip-action-mailer \
  --skip-test　\　
  --skip-action-text 


# Gemfileが再作成されるのでbuildしなおす。
docker-compose build

```

CLIの引数については以下。

- `--css=bootstrap` : 今回はbootstrapを使う。使いたいcssによって適宜変え`て下さい。
- `--skip-git` : .gitignoreはすでにあるもの使用すればいいのでskipで。
- `--skip-jbuilder` : jsonで返す必要ないならいらない。必要になったら後で`入れる
- `--skip-action`-mailbox : メール処理機能必要ないことのほうが多そう。
- `--skip-action-mailer` : 同様。
- `--skip-test` : rspec使う場合はtestをskip
- `--skip-action-text` : ction textも必要なアプリ限られるはずなので一旦skip。


### 4. DBのセットアップ

```yml:database.yml
default: &default
  adapter: postgresql
  encoding: unicode
  pool: 5

development:
  <<: *default
  database: myapp_development
  host: db
  username: <%= ENV["POSTGRES_USER"] %>
  password: <%= ENV["POSTGRES_PASSWORD"] %>

```

```sh

# dbを作成する
docker-compose run web rake db:create

# runさせる
docker-compose up
```

