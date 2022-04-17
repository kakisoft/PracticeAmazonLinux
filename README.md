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



amazon-linux-extras | grep ansible
amazon-linux-extras enable php8.0


超久々に Ansible の playbook 書いてるけど、Amazon Linux 用に書く場合って、remiとかから引っ張って来なくても、「amazon-linux-extras enable php8.0」みたいな感じで、簡単に ON/OFF ができるのな...





amazon-linux-extras


amazon-linux-extras enable nginx1


---
一応対象PHPバージョンの拡張パッケージをインストールする方法がこれです：
sudo yum install php[バージョン]-php-[extension]
例：
sudo yum install php74-php-mbstring




sudo yum remove php



amazon-linux-extras | grep node




curl -sL https://rpm.nodesource.com/setup_8.x | sudo bash -



curl -fsSL https://deb.nodesource.com/test | bash -



yum install nodejs12






curl -sL https://rpm.nodesource.com/setup_12.x | bash -
sudo yum install -y nodejs




curl -fsSL https://rpm.nodesource.com/setup_16.x | sudo bash -
sudo yum install -y nodejs


curl -fsSL https://rpm.nodesource.com/setup_16.x | bash -


curl -sL https://rpm.nodesource.com/setup_16.x | bash -
curl -sL https://rpm.nodesource.com/setup_16.x
sudo yum install -y nodejs





[WARNING]: Consider using the get_url or uri module rather than running 'curl'.  If you need to use command because get_url or uri is insufficient you can add 'warn: false' to
this command task or set 'command_warnings=False' in ansible.cfg to get rid of this message.



