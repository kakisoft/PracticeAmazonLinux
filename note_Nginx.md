## Nginx を CentOS 7 にインストールする手順
https://weblabo.oscasierra.net/nginx-centos7-install/

_________________________________________________________
# Install

## Amazon Linux
```
sudo amazon-linux-extras enable nginx1
sudo yum install -y nginx

sudo systemctl enable nginx
sudo systemctl start nginx
```

# start/stop
こっちの方が安定している気がする。
```
sudo systemctl start nginx

sudo systemctl restart nginx

sudo systemctl stop nginx
```

nginx コマンドを使う場合
```
sudo nginx

sudo nginx -s restart

sudo nginx -s quit
sudo nginx -s stop
```
-s stop terminates the nginx process immediately while -s quit does a graceful shutdown.


# Config Check
```
sudo nginx -t
```

# Config Reload
```
sudo systemctl reload nginx

sudo nginx -s reload
```

# status check
```
systemctl status nginx
ps ax | grep nginx
```

# Files

|  場所                      |  説明                           |
|:-------------------------|:------------------------------|
|  /etc/nginx/             |  Nginx の設定ファイルが保存されているディレクトリ  |
|  /etc/nginx/nginx.conf   |  メインとなる設定ファイル                 |
|  /var/log/nignx/         |  Nginx のログが保存されるディレクトリ        |
|  /usr/share/nginx/html/  |  WEBサイトの公開ディレクトリ              |


# Truble shoot

## Port error
```
nginx: [emerg] bind() to 0.0.0.0:80 failed (98: Address already in use)
nginx: [emerg] bind() to [::]:80 failed (98: Address already in use)
```
ポートが占有されている。  
終了に失敗すると、プロセスが残っている事がある。  

```
sudo lsof -i | grep nginx

sudo kill -9 3254
sudo kill -9 3255
```

