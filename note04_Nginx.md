【Amazon Linux】Vagrant で Nginx を起動してみる

_______________________________________________________________________________

## 状況
EC2 に Nginx をインストールし、Webサーバとして動かす。  

Nginx で色々実験したい事が出てきたんで、好き勝手触って検証できる環境をローカルに用意したい。  

Amazon Linux の Vagrant box があったので、それを使う。  

ちなみに Amazon Linux の Docker コンテナ（公式）もあったのですが、爆死しました。  
詳細はこちらを。  
[【Docker】Amazon Linux のコンテナ（AWS公式）を使って開発環境を構築したら、EC2 デプロイのアプリは幸せになれるんじゃ無いかと思ったが、見事に頓挫した](https://kaki-note-02.netlify.app/2022/04/25/)  


## Vagrant ： box
こちらを使用しました。  
https://app.vagrantup.com/gbailey/boxes/amzn2  


## Vagrantfile ファイル
ポートのリダイレクト設定を追加。  
設定内容の詳細は[Vagrant公式](https://www.vagrantup.com/docs/networking/forwarded_ports)をご参照ください。  
```rb
Vagrant.configure("2") do |config|
  config.vm.box = "gbailey/amzn2"
  config.vm.network "forwarded_port", guest: 80, host: 8080
end
```

起動コマンドは、  
vagrant up  

Box へのログインは、  
vagrant ssh  


## Nginx のインストール
```
sudo amazon-linux-extras enable nginx1
sudo yum install -y nginx
```

## Nginx の起動設定
```
sudo systemctl enable nginx
sudo systemctl start nginx
```

## 起動確認
Box 内から実行
```
curl http://localhost/
```
『curl: (7) Failed to connect to localhost port 80 after 0 ms: Connection refused』といったメッセージが出た場合、「systemctl status nginx」等のコマンドにて、Nginx の起動状態を確認。  

ホストから実行（ブラウザ）
```
http://localhost:8080/
```

____________________________________



## SELinux
https://qiita.com/enokawa/items/9622405c7350706837c8
SELinux無効化


## firewall

### firewall : インストール
sudo yum install -y firewalld

### firewall : 有効設定
sudo systemctl enable firewalld.service

### firewall : 開始
sudo systemctl start firewalld.service

### 状態確認
```
sudo systemctl status firewalld.service
```
active(running) となっていれば、有効となっています。


## iptables
パケットフィルタリング

Amazon LinuxでFirewallを設定する(iptables)
https://piitre.com/blog/programming/amazon-linux-firewall/
https://www.designet.co.jp/faq/term/?id=aXB0YWJsZXM
https://pig-log.com/aws-initialsetting-amazonlinux2/
https://www.bit-hive.com/articles/20190807



