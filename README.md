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


## delete containers on exit
```
docker-compose down --rmi all
docker-compose down --rmi all --volumes
```

## execute setup script
``
./server-setup
``

_______________________________________________________________________________
_______________________________________________________________________________
_______________________________________________________________________________


amazon-linux-extras enable nginx1





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



kakisoftnoMacBook:ryuki-prd kakisoft$ ls -la | grep ryuki
-rwxr-xr-x   1 kakisoft  staff  2688  4  1 12:14 ryuki
 751

kakisoftnoMacBook:PracticeLaravel8-01 kakisoft$ ls -la | grep myapp
-rwxr-xr-x   1 kakisoft  staff  2881  3 16 22:44 myapp



