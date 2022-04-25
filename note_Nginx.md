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

# Install
```
systemctl start nginx

systemctl stop nginx
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




