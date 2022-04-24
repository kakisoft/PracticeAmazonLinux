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
______________________________________________________________________________

## yumコマンド一覧
https://qiita.com/kazu56/items/3eeab8884f62be1cf3e6


________________________________________________________
# nginx メモ



sudo amazon-linux-extras enable nginx1
sudo yum install -y nginx


## nginxメインプロセス番号を照会
ps -ef | grep nginx



sudo systemctl 有効 nginx


curl: (7) Failed to connect to  after 0 ms: Connection refused
curl: (7) Failed to connect to 127.0.0.1 port 8080 after 0 ms: Connection refused



curl http://localhost/
curl http://localhost:80/
curl http://localhost:8000/
curl http://localhost:8080/

curl -I 127.0.0.1
curl -I 127.0.0.1:80/
curl -I 127.0.0.1:8000/
curl -I 127.0.0.1:8080/



vi /etc/nginx/nginx.conf

root         /usr/share/nginx/html;




========================================================================
https://i-think-it.net/ec2-startup-centos7-7/
----------------------------------------
/etc/sysconfig/selinux
SELINUX=disabled
----------------------------------------



========================================================================






