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

## execute Ansible
``
ansible-playbook my-playbook01.yml


ansible-playbook my-playbook01.yml --syntax-check
ansible-playbook my-playbook01.yml --list-task
ansible-playbook my-playbook01.yml --check

``


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

amazon-linux-extras enable nginx1





