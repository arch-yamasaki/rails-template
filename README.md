# Rails + postgres + docker-compose テンプレ

## 概要
localでRailsを立ち上げるテンプレ

## セットアップ方法


### cloneする

cd rails-template


###

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
docker-compose run web bundle install --path vendor/bundle
# dbを作成
docker-compose exec web rails db:create
# rails runする
docker-compose exec web rails s

```