# PracticeAmazonLinux
https://hub.docker.com/_/amazonlinux


## start / stop
```
docker-compose up -d
docker-compose up -d --build
docker-compose down
```

## enter the container
```
docker-compose exec amazon_linux bash
docker-compose exec --user root amazon_linux bash
```

## execute setup script
``
./server-setup
``

## delete containers on exit
```
docker-compose down --rmi all
docker-compose down --rmi all --volumes
```

_______________________________________________________________________________
_______________________________________________________________________________
_______________________________________________________________________________
# CentOS
```
docker-compose -f docker-compose-contos.yml up -d
docker-compose -f docker-compose-contos.yml exec centos bash

docker-compose -f docker-compose-contos.yml down
```

_______________________________________________________________________________
_______________________________________________________________________________
_______________________________________________________________________________


amazon-linux-extras | grep 

amazon-linux-extras enable nginx1
yum install -y nginx

## yumコマンド一覧
https://qiita.com/kazu56/items/3eeab8884f62be1cf3e6




yum install php55 php55-mbstring php55-soap php55-gd php55-mcrypt php55-pdo httpd24
yum install php php-mbstring





https://blueanela.hatenadiary.org/entry/20140111/1389431180
・curlのヘッダファイルをインストール　※これをやらないと変なpdo_mysqlドライバができてしまい後々はまる
yum install curl-devel


https://pentan.info/php/pdo_mysql_install.html
PDO_MYSQLをインストールするには事前にMySQLクライアントのインストールが必要です。



https://www.codegrepper.com/code-examples/php/mysql+pdo+extension+aws+extras+php+7.4
sudo yum install -y amazon-linux-extras
sudo  amazon-linux-extras | grep php
sudo amazon-linux-extras enable php7.4
sudo yum install php php7.4-{pear,cgi,common,curl,mbstring,gd,mysqlnd,gettext,bcmath,json,xml,fpm,intl,zip,imap}





AWS EC2にPHP7.4をインストールする
https://www.it-ouji.com/2021/10/26/aws-ec2%E3%81%ABphp7-4%E3%82%92%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB%E3%81%99%E3%82%8B/
# composerのインストール
curl -sS https://getcomposer.org/installer | php
 
# pharファイルを環境PATH内に移動
sudo mv composer.phar /usr/local/bin/composer





sudo yum install php php7.4-{pear,cgi,common,curl,mbstring,gd,mysqlnd,gettext,bcmath,json,xml,fpm,intl,zip,imap}


sudo yum install php-{mbstring}
sudo yum install php8.0-{mbstring}
sudo yum install php php-{mbstring}
sudo yum install php php8.0-{mbstring,xml}

sudo yum install php php8.0-{xml}


yum -y install php-mbstring

yum install --enablerepo=remi,remi-php70 php php-devel php-mbstring php-pdo php-gd

yum install php-mbstring
sudo yum install -y php-mbstring php-devel



______
# PHP
sudo yum install -y php-bcmath
sudo yum install -y php-dom
sudo yum install -y php-gd
sudo yum install -y php-mbstring
sudo yum install -y php-mysqli
sudo yum install -y php-mysqlnd     ?
sudo yum install -y php-pdo         ?
sudo yum install -y php-pdo_mysql   ?
sudo yum install -y php-pdo_sqlite  ?
sudo yum install -y php-posix
sudo yum install -y php-redis       X
sudo yum install -y php-SimpleXML   X
sudo yum install -y php-sodium
sudo yum install -y php-sqlite3    ?
sudo yum install -y php-xml        ?
sudo yum install -y php-xmlreader  ?
sudo yum install -y php-xmlwriter  ?
sudo yum install -y php-xsl        ?
sudo yum install -y php-opcache



yum install php-opcache

sudo yum install -y php-mbstring


amazon-linux-extras enable redis6
yum install php-redis



amazon-linux-extras | grep pecl
pecl install redis







________________________________________________________

$ curl -sS https://getcomposer.org/installer | php
All settings correct for using Composer
Downloading...
Composer (version 1.8.6) successfully installed to: /home/ec2-user/composer.phar
Use it: php composer.phar

$ mv composer.phar /usr/local/bin/composer


$ composer
______
/ ____/___  ____ ___  ____  ____  ________  _____
/ /   / __ \/ __ `__ \/ __ \/ __ \/ ___/ _ \/ ___/
/ /___/ /_/ / / / / / / /_/ / /_/ (__  )  __/ /
\____/\____/_/ /_/ /_/ .___/\____/____/\___/_/
                 /_/
Composer version 1.8.6 2019-06-11 15:03:05



$ which composer
/usr/local/bin/composer
________________________________________________________


sudo curl -sS https://getcomposer.org/installer | sudo php
sudo mv composer.phar /usr/local/bin/composer
sudo ln -s /usr/local/bin/composer /usr/bin/composer
sudo composer install

________________________________________________________








 sudo yum install http://rpms.famillecollet.com/enterprise/remi-release-8.rpm
sudo yum --enablerepo=remi-php80 install php-pear
sudo yum --enablerepo=remi-php80 install php-pear

remi-php80


amazon-linux-extras enable epel
sudo yum install -y epel-release




yum -y install php-devel


pickle
pecl install redis
pickle install redis




sudo yum install redis



## AWS EC2 AmazonLinux2 peclコマンドを使用できる様にする
https://qiita.com/miriwo/items/112e66e127a8d801d429



yum install epel-release



# 公式
https://github.com/phpredis/phpredis/blob/develop/INSTALL.markdown

// If using PHP >= 7.3
pickle install redis

php pickle install redis


## Amazon Linux 2 にphp-pecl-redis入れる
この方法だと NG
```
sudo yum install php-pecl-redis 
sudo systemctl restart php-fpm
```




amazon-linux-extras | grep repo

amazon-linux-extras | grep redis
amazon-linux-extras enable redis6


yum install -y rsyslog



sudo systemctl restart php-fpm


docker run -d --privileged --name hoge amazonlinux /sbin/init

amazonlinux







