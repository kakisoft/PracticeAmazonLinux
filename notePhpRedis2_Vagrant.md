【Amazon Linux】 PhpRedis のインストールに凄まじくハマった(Vagrant編)

前回の続き。

## タスク
EC2 に PHPの実行環境を作る。  

本番２台、ステージング２台、開発用１台と、複数に設定が必要なため、シェルスクリプトを作成して必要なツールやライブラリのインストールや設定はコマンド一発で完了できるようにしたい。  

## 方針
AWS が Amazon Linux の公式イメージを Docker Hub にて配布しているので、それを使って実験してみたが、どうにも上手くいかなかった。  

が、よく考えるとコンテナは OS をエミュレートするものではなくプロセス単位で動作するので、OSに近いレイヤーの操作は制限されるのでは？  
と推測したので、OSをエミュレートできる環境（OS の上に OSが乗っかっている環境）を用意できる Vagrant で実験してみることにした。  

AWS 公式の box は無いみたいなんで、ダウンロード数の最も多い box を使用する事にしました。  
最終更新も１日前と、フレッシュな状態を保っていました。（実験時の日付：2022/4/24）  
https://app.vagrantup.com/gbailey/boxes/amzn2  


ちなみに、PhpRedis の公式サイトは以下。

（PhpRedis 公式サイト）  
https://github.com/phpredis/phpredis

（PhpRedis インストールガイド）  
https://github.com/phpredis/phpredis/blob/develop/INSTALL.markdown

PHP のバージョンは 8.0。

## yum によるインストール（失敗）
以下、実行コマンド。  

root ユーザで実行しました。  
```
amazon-linux-extras enable epel
sudo yum install -y epel-release
sudo yum install -y https://rpms.remirepo.net/enterprise/remi-release-7.rpm
sudo yum install -y php80-php-pecl-redis5.x86_64
echo "extension=redis.so" >> /etc/php.ini
php -m | grep redis
```

インストール状況を確認するために「php -m | grep redis」を入力すると、こんなメッセージが出ました。  
```
[root@localhost vagrant]# php -m | grep redis
PHP Warning:  PHP Startup: Unable to load dynamic library 'redis.so' (tried: /usr/lib64/php/modules/redis.so (/usr/lib64/php/modules/redis.so: cannot open shared object file: No such file or directory), /usr/lib64/php/modules/redis.so.so (/usr/lib64/php/modules/redis.so.so: cannot open shared object file: No such file or directory)) in Unknown on line 0
```

どうやら上手くいっていない模様。


## pickle によるインストール（成功）
公式では、この方法がトップに出ています。（PHP >= 7.3 の場合）

以下、実行コマンドです。  

root ユーザで実行しました。  
```
amazon-linux-extras enable php8.0
sudo yum install -y php
sudo yum install -y php-bcmath php-dom php-gd php-mbstring php-mysqli php-posix php-sodium php-opcache
sudo yum install -y php-devel
wget https://github.com/FriendsOfPHP/pickle/releases/latest/download/pickle.phar
php pickle.phar
chmod +x pickle.phar
php pickle.phar info apcu
mv pickle.phar pickle
php pickle install redis
echo "extension=redis.so" >> /etc/php.ini
php -m | grep redis
```

「php pickle install redis」と入力した時、色々聞かれますが、全部デフォルトにしました。  

特に変なメッセージは出ずに、無事 PhpRedis がインストールされたようです。  

ですが、一部公式からコマンドを変えてます。  

公式では「pickle install redis」と、pickle というコマンドがあるかのように説明していますが、実際には「php pickle」と、php のサブコマンドのように使うわないと not found となってしまうので、上記のように使ってみました。  

もしかしたら公式サイトのミスかもしれないので、気が向いたら issue を送ってみます。  


## pecl によるインストール（成功）
公式では、この方法がトップに出ています。（PHP >= 7.3 の場合）

以下、実行コマンドです。  

root ユーザで実行しました。  
```
amazon-linux-extras enable epel
sudo yum install -y epel-release
sudo yum install -y http://rpms.famillecollet.com/enterprise/remi-release-7.rpm
sudo yum -y --enablerepo=remi-php74 install php-pear
pecl install redis
echo "extension=redis.so" >> /etc/php.ini
php -m | grep redis
```

「pecl install redis」と入力した時、色々聞かれますが、全部デフォルトにしました。  

インストール後、以下のようなメッセージが出たので、php.ini への追記は必須と思われます。  
```
Build process completed successfully
Installing '/usr/lib64/php/modules/redis.so'
install ok: channel://pecl.php.net/redis-5.3.7
configuration option "php_ini" is not set to php.ini location
You should add "extension=redis.so" to php.ini
```

## 結論
Amazon Linux に PhpRedis をインストールする場合、PHP 8 以上なら pickle を使うといいんじゃないかな。  

何やらマイナーなパッケージマネージャーで、このライブラリの他にどんな使い所があるかはよく分かりませんが。  


