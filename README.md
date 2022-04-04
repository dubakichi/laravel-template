# laravel-template
dockerでのlaravel開発用テンプレ。
# dockerコンテナ構成

サービス名 | 用途 | 構成
-----|-----|-----
nginx | Webサーバ　| nginx v1.21.6 + FPM
php | アプリケーション | PHP v8.0.17 Laravel v9.6.0
mysql | DBサーバ　| MySQL v8.0


# 利用方法
このテンプレを利用する時の初期設定を記載します。

## プロジェクト名変更
変更箇所
* laravel-template(ルートフォルダ) 例： jasmine
* my-laravel-app(ルート直下のapp) 例： jasmine-app
* nginx default.conf rootのパス(上記app名変更反映) 

## 環境変数設定

Project直下に以下のファイルを配置する
```
❯ cat .env
DB_NAME=任意
DB_USER=任意
DB_PASS=任意
DB_PORT=3306
TZ=Asia/Tokyo
```

アプリ直下

.env (テンプレファイルだけGAあるので、リネームして作成する)
```
DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=[DBの設定に合わせる]
DB_USERNAME=[DBの設定に合わせる]
DB_PASSWORD=[DBの設定に合わせる]
```

## docker起動

```
docker-compose up -d
```

ちなみに停止は、

```
docker-compose down
```

## composerがない時

```
docker-compose exec php bash
cd /usr/bin && curl -s http://getcomposer.org/installer | php && ln -s /usr/bin/composer.phar /usr/bin/composer
```

## composer install
vendor配下のファイルはgit管理対象外なので復旧する必要あり
```
cd /var/www/html/my-laravel-app
composer install
```

## db migrate

```
root@95d51ce5aef5:/var/www/html/my-laravel-app# php artisan migrate
Migration table created successfully.
Migrating: 2014_10_12_000000_create_users_table
Migrated:  2014_10_12_000000_create_users_table (112.52ms)
Migrating: 2014_10_12_100000_create_password_resets_table
Migrated:  2014_10_12_100000_create_password_resets_table (68.66ms)
Migrating: 2019_08_19_000000_create_failed_jobs_table
Migrated:  2019_08_19_000000_create_failed_jobs_table (93.52ms)
Migrating: 2019_12_14_000001_create_personal_access_tokens_table
Migrated:  2019_12_14_000001_create_personal_access_tokens_table (157.85ms)
```

## encription key設定

設定変更前
```
root@95d51ce5aef5:/var/www/html/my-laravel-app# cat .env | grep KEY
APP_KEY=
```
変更
```
root@95d51ce5aef5:/var/www/html/my-laravel-app# php artisan key:generate
Application key set successfully.
```
変更後確認
```
root@95d51ce5aef5:/var/www/html/my-laravel-app# cat .env | grep APP_KEY
APP_KEY=base64:ucWxxxxx[キーが設定されるようになります]
```
