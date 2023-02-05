# Rails + postgres + docker-compose テンプレ

## 概要
localでRailsを立ち上げるテンプレ用Git Repositoryを作成した。


## セットアップ方法

全体のセットアップ方法について記載。以下の流れで行う。
1. repositoryのクローン
2. 一部ファイルを編集
3. docker上でのrailsのセットアップ
4. DBのセットアップ

### cloneする

cd rails-template



### ファイルの編集

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

- Gemfile.sample

`Gemfile.sample`から`Gemfile`に変更

### railsのセットアップ

```sh
# イメージを作成
docker-compose build
# rails含む各種Gemをbundleでinstallする
docker-compose run web rails new . --force  --skip-git --database=postgresql 

# 他のinstall方法
# docker-compose run web rails new . cat-hotwire --css=bootstrap --skip-jbuilder --skip-action-mailbox --skip-action-mailer --skip-test --skip-active-storage --skip-action-text --skip-git --database=postgresql

# Gemfileが再作成されるのでbuildしなおす。
docker-compose build

```

### DBのセットアップ

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

