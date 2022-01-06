# VagrantでDocker環境を構築する(Laravel + Laradock)

インストール
----------

ローカルマシンに下記をインストールしてください。

* [Virtualbox](https://www.virtualbox.org/)
* [Vagrant](https://www.vagrantup.com/)

設定方法
-------

### 1.vagrantプラグインを取得(Vagrant環境にてDockerを利用するため)

`vagrant-vbguest` はホストOSとゲストOSのGuestAdditionsのバージョンを自動で合わせるために利用します。

```
vagrant plugin install vagrant-docker-compose
vagrant plugin install vagrant-vbguest
```

### 2.Vagrantの起動

```
vagrant up
```
うまく起動できると`vagrant ssh` にてログインすることができます。

### 3.Laradockの初期設定

今回のサンプルではPHP8を利用するため、環境整備を行います。
まずは設定変数の準備を行います。

```
cd laradock
cp .env.example .env
```

下記を更新。PHPのバージョンは今回利用するプロジェクトに合わせての設定になります。
```
- APP_CODE_PATH_HOST=../
+ APP_CODE_PATH_HOST=../sample

- PHP_VERSION=7.4
+ PHP_VERSION=8.0
```

設定が終わったら、dockerコンテナを起動します。

```
docker-compose up -d nginx mysql
```

### 4.サンプルlaravelプロジェクトの初期設定

今回は[drehimself/laravel-shopping-cart-example](https://github.com/drehimself/laravel-shopping-cart-example)をPHP8/Laravel8に簡易的に対応したプロジェクトを
設置してあるのでそちらを利用します。

php/composerを利用するためにvagrant -> dockerコンテナの順にssh接続を行います。

```
# vagrant にssh接続
vagrant ssh
# dockerのworkspaceにssh
cd share/laradock
docker-compose exec --user=laradock workspace bash
```

packageのインストールを行います。
```
composer install
```

DB作成・サンプルデータを投入します。

```
php artisan key:generate
php artisan migrate
php artisan db:seed
```

これでホストマシンから [http://192.168.56.10](http://192.168.56.10) に接続できれば完了です。


おまけ
------

vagrantとホストマシン間でのファイルのやり取りを行いたい場合、
scpにてファイル転送するのが簡単です。

```
# ホストマシンでvagrantのssh接続情報を取得し保存
vagrant ssh-config > ssh.config
# 
scp -P 2222 -F ssh.config -r vagrant@default:/vagrant/sample /Users/****/Desktop/
```