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

## yumコマンド一覧
https://qiita.com/kazu56/items/3eeab8884f62be1cf3e6


________________________________________________________







