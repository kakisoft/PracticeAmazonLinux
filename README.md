# PracticeAmazonLinux
https://hub.docker.com/_/amazonlinux


## start / stop
```
docker-compose up -d
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

_______________________________________________________________________________
_______________________________________________________________________________
_______________________________________________________________________________
## amazon linuxのイメージを使用してsshできるコンテナを作る
https://qiita.com/takeshi_miyajim/items/92a525e6beddd438c76b

## 【Docker】AmazonLinuxコンテナ起動とSSHアクセスまで
https://genchan.net/it/virtualization/docker/12695/

## SSH 接続できる Amazon Linux 2 の Docker イメージを作成してシステム構築してみる
（複雑なので、別の方法を考えてみる）
https://qiita.com/aucfan-engineer/items/6881d0026b5937b2558d



ssh -i authorized_keys user1@localhost -p 2022

ssh user1@localhost -p 2022




ssh-keygen -t rsa -b 4096 -C "m71203@gmail.com" -f authorized_keys



cat ~/.ssh
ls ~/.ssh


/etc/ssh/sshd_config.




/etc/hosts.deny
/etc/hosts.allow







______________________________________________________
## CentOS 7
```
『epel download』で検索。（EPEL）
右クリック→リンク先のアドレスをコピー

wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
sudo rpm -Uvh epel-release-latest-7.noarch.rpm
sudo yum -y install ansible
ansible --version
```

______________________________________________________
## Amazon Linux2にAnisbleをインストールする方法
https://qiita.com/1_ta/items/92dcfa6fa2a33cb11442


Amazon Linux2でインストールするためには、「Amazon Linux Extras」が必要です。


bash-4.2# which amazon-linux-extras
/usr/bin/amazon-linux-extras

bash-4.2# amazon-linux-extras | grep ansible
  0  ansible2                 available    \

### 有効化
sudo amazon-linux-extras enable ansible2

### インストール
sudo yum install -y ansible


