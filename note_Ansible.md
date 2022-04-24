## execute Ansible
``
ansible-playbook my_setup01.yml


ansible-playbook my_setup01.yml --syntax-check
ansible-playbook my_setup01.yml --list-task
ansible-playbook my_setup01.yml --check
``

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
