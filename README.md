# PracticeAmazonLinux
https://hub.docker.com/_/amazonlinux


## 起動・終了
```
docker-compose up -d
docker-compose down

// 再起動
docker-compose restart
```

## ビルドして再起動
```
docker-compose up -d --build
```

## コンテナに入る（PHP）
```
docker-compose exec --user root amazon_linux bash
docker-compose exec amazon_linux bash
```


## 終了時にコンテナを削除
```
docker-compose down --rmi all
docker-compose down --rmi all --volumes
```

_______________________________________________________________________________
_______________________________________________________________________________
_______________________________________________________________________________


