amazon-linux-extras | grep redis
amazon-linux-extras enable redis6

PhpRedis

php-redis

（公式）
https://github.com/phpredis/phpredis

インストールガイド（公式）
https://github.com/phpredis/phpredis/blob/develop/INSTALL.markdown

_______________________________________
【Amazon Linux】 PhpRedis のインストールに凄まじくハマった

## タスク
EC2 に PHPの実行環境を作る。  

本番２台、ステージング２台、開発用１台と、複数に設定が必要なため、シェルスクリプトを作成して必要なツールやライブラリのインストールや設定はコマンド一発で完了できるようにしたい。  

AWS が Amazon Linux の公式イメージを配布しているので、ローカルでそれを使って実験してみる。  
実験用の EC2 を立てて、失敗したらインスタンス作り直し、という方法で進めるよりも効率よさそう。という事で、スクリプトはローカルの Amazon Linux コンテナに入って作成。  

ちなみに配布先は、ここ。  
https://hub.docker.com/_/amazonlinux  


## 状況
Redis を PHP から使う時のライブラリに、「PhpRedis」を使おうとしているが、どうにも上手く行かない。  
以下、その実験記録。  

（PhpRedis 公式サイト）  
https://github.com/phpredis/phpredis

（PhpRedis インストールガイド）  
https://github.com/phpredis/phpredis/blob/develop/INSTALL.markdown



## yum でインストールしてみる
公式の内容  

> yum install php-pecl-redis  

実行結果  
```
bash-4.2# yum install php-pecl-redis
Loaded plugins: ovl, priorities
No package php-pecl-redis available.
Error: Nothing to do
```
ダメっぽい。  

試しに redis をインストールしてみる。  
```
amazon-linux-extras | grep redis
amazon-linux-extras enable redis6
yum install -y redis
```

その後、yum install php-pecl-redis を実行しても、結果変わらず。  

公式サイトによると、EPEL repository から引っ張ってこれるらしい。  
https://docs.fedoraproject.org/en-US/epel/#_el9  

なんじゃりゃ。  

まず、EL9、EL8、EL7 と、どれを使えばいいのかが分からん。  

dnf コマンドを使っているが、Amazon Linux ではデフォルト状態だと使えないみたい。  
これは Red Hat系で使われている RPMパッケージを扱うためのパッケージ管理コマンドらしい。  

そんなのがあったのか...　rpm パッケージは、だいたい curl や wget で拾ってきてるイメージだった。 

インストールできるのかと思い、「amazon-linux-extras | grep dnf」で探すも見つからず。  

試しにインストールしてみる。
```
bash-4.2# yum -y install dnf
Loaded plugins: ovl, priorities
amzn2-core
No package dnf available.
Error: Nothing to do
```
ダメっぽい。  

そもそも、パッケージマネージャーをパッケージマネージャーでインストールする、というのはどういう状況やねん。（割とよく見る光景だけど）  

これも試してみたが、インストールできず。  
[CentOS7.5にdnfをインストールする](https://qiita.com/piyojiro/items/58f9a89120808e1c1e84)  

と思いきや、こんなの見つけた。AWS 公式見解なので、信頼度高め。  
https://aws.amazon.com/jp/amazon-linux-2/faqs/ 

> Q:Amazon Linux 2 が「yum」パッケージマネージャーとして Python 2.7 から切り替えられない、または Python 3 ベースの DNF に移行しない理由は何ですか?  
>
> オペレーティングシステムの LTS リリース中は、別のパッケージマネージャーに根本的な変更を加えたり、置き換えたり、追加したりするリスクが非常に高くなります。  
> したがって、Amazon Linux 用の Python 3 移行を計画する際に、Amazon Linux 2 内ではなく、メジャーリリースの境界を越えて行うことを決定しました。  
> これは、LTS 契約を結んでいない場合でも、他の RPM ベースの Linux ディストリビューションで共有されるアプローチです。  

Amazon Linux 2 では dnf は使用できないらしい。  

という事で、公式で説明されている dnf を使用したインストールは実行できないため、別の方法を探すことになります。  

試しに、yum install epel-release を実行。  
```
bash-4.2# yum install epel-release
Loaded plugins: ovl, priorities
amzn2-core
No package epel-release available.
Error: Nothing to do
```
ダメっぽい。  

続いて、公式で説明しているこの方法。

> subscription-manager repos --enable rhel-*-optional-rpms \  
>                            --enable rhel-*-extras-rpms \  
>                            --enable rhel-ha-for-rhel-*-server-rpms  
> yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm  


最初のコマンドはコケた。  

```
bash-4.2# subscription-manager repos --enable rhel-*-optional-rpms \  
                           --enable rhel-*-extras-rpms \  
                           --enable rhel-ha-for-rhel-*-server-rpms  
bash: subscription-manager: command not found  
```


が、その次のコマンドは上手く行ったみたい。  

```
bash-4.2# yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm  

（中略）

Installed:
  epel-release.noarch 0:7-1

Complete!
```

無事、epel がインストールできたので、「yum install php-pecl-redis」を再び実行。  
が、以下のように上手く行かず。  

```
bash-4.2# yum install php-pecl-redis  
Loaded plugins: ovl, priorities  
amzn2-core  
epel/x86_64/metalink  

（中略）  
Error: Package: php-pecl-redis-2.2.8-1.el7.x86_64 (epel)  
（中略）  
Error: Package: php-pecl-igbinary-1.2.1-1.el7.x86_64 (epel)  
（中略）  
Error: Package: php-pecl-redis-2.2.8-1.el7.x86_64 (epel)  
（中略）  
Error: Package: php-pecl-igbinary-1.2.1-1.el7.x86_64 (epel)  
```

「php -m | grep redis」で検索しても、当然、インストールされた様子は無い。  

そんな感じで、yum を使った公式説明によるインストールは行き詰まりました。  


## pickle でインストールしてみる
公式にこんな記述がありました。

> pecl install redis
> 
> // If using PHP >= 7.3
> pickle install redis

pickle が何なのかはよく分からないが、パッケージマネージャーのようなものだろうか。  

「amazon-linux-extras | grep pickle」で探しても見つからず。  

yum でもインストールできないみたい。  

```
bash-4.2# yum install -y pickle
Loaded plugins: ovl, priorities
214 packages excluded due to repository priority protections
No package pickle available.
Error: Nothing to do
```

という事で、pickle の公式サイトを見てみる。   

https://github.com/FriendsOfPHP/pickle  

インストールガイドの通りに操作してみる。

これはOK。
```
wget https://github.com/FriendsOfPHP/pickle/releases/latest/download/pickle.phar
```

ここでエラー出た。
```
php pickle.phar
```

> bash-4.2# php pickle.phar
> PHP Fatal error:  Uncaught Exception: Extension 'mbstring' required but not loaded, full required list: zlib, mbstring, simplexml, json, dom, openssl, phar in phar:///var/tmp/files/pickle.phar/src/Console/Application.php:96
> Stack trace:
> #0 phar:///var/tmp/files/pickle.phar/src/Console/Application.php(51): Pickle\Console\Application::checkExtensions()

php のモジュールが色々要るみたい。  

必要そうなのをブッ込んだ後、再実行。
```
sudo yum install -y php-bcmath php-dom php-gd php-mbstring php-mysqli php-posix php-sodium php-opcache
```

上手く行ったみたい。  
```
bash-4.2# php pickle.phar

██████╗ ██╗ ██████╗██╗  ██╗██╗     ███████╗
██╔══██╗██║██╔════╝██║ ██╔╝██║     ██╔════╝
██████╔╝██║██║     █████╔╝ ██║     █████╗
██╔═══╝ ██║██║     ██╔═██╗ ██║     ██╔══╝
██║     ██║╚██████╗██║  ██╗███████╗███████╗
╚═╝     ╚═╝ ╚═════╝╚═╝  ╚═╝╚══════╝╚══════╝  v0.7.9
```

続いて、インストールガイドに従い、以下を実行。

```
chmod +x pickle.phar

pickle.phar info apcu
```

この時点でエラー発生。

```
bash-4.2# pickle.phar info apcu
bash: pickle.phar: command not found
```

何事？  

が、マニュアルをよく読むと、「pickle.phar」を使う時、「php pickle.phar」と、先頭に "php" を付けているコマンドがある事に気付く。  

という事で、"php" を付けて実行。  

```
ash-4.2# php pickle.phar info apcu
  - Installing apcu (latest-stable): Downloading (100%)         
+-----------------------------------+--------+
| Package name                      | apcu   |
| Package version (current release) | 5.1.21 |
| Package status                    | stable |
+-----------------------------------+--------+
（以下略）
```

上手く行ったみたい。  

続きのコマンドを実行。  

```
mv pickle.phar pickle
```

Amazon Linux を使う場合、これで終わりなのか？  

インストールコマンドは、こんな感じらしい。
```
pickle install memcache
```

実行結果
```
bash-4.2# pickle install memcache
bash: pickle: command not found
```
ダメじゃん。  

というか、先頭に "php" を付けないといけないのでは？  

```
bash-4.2# php pickle install memcache
  - Installing memcache (latest-stable): Downloading (100%)

In Loader.php line 154:

  Version mismatch - '4.0.5.2' != '8.0' in source vs. XML
```

最終的にエラーとなったものの、コマンドそのものは正常に動いている模様。  

という事で、本命の phpredis をインストール。  

公式の説明では、「pickle install redis」となっているので、先頭に "php" を付けて「php pickle install redis」と実行。  


```
bash-4.2# php pickle install redis
  - Installing redis (latest-stable): Downloading (100%)
+-----------------------------------+--------+
| Package name                      | redis  |
| Package version (current release) | 5.3.7  |
| Package status                    | stable |
+-----------------------------------+--------+
use system liblzf (default: ): 
use system libsztd (default: ): 
use system liblz4 (default: ): 
whether to enable sessions (default: yes): 
whether to enable json serializer support (default: yes): 
whether to enable igbinary serializer support (default: no): 
whether to enable msgpack serializer support (default: no): 
whether to enable lzf compression (default: no): 
whether to enable Zstd compression (default: no): 
whether to enable lz4 compression (default: no): 
The following error(s) happened: phpize failed
Would you like to read the log?
1: phpize
2: Can't find PHP headers in /usr/include/php
2: The php-devel package is required for use of this command.
```

途中、「use system liblzf (default: )」とか「use system libsztd (default: ):」といった、ワケわからんダイアログを求められた。  

全部エンターでデフォルトの内容を採用。  

エラーが発生している。  

> The following error(s) happened: phpize failed

phpize、こんなものらしい。  


[phpizeとは何？ Weblio辞書](https://www.weblio.jp/content/phpize#:~:text=%E6%8B%A1%E5%BC%B5%E3%83%A2%E3%82%B8%E3%83%A5%E3%83%BC%E3%83%AB%E3%82%92%E3%83%93%E3%83%AB%E3%83%89%E3%81%99%E3%82%8B,%E3%83%93%E3%83%AB%E3%83%89%E3%81%99%E3%82%8B%E3%81%93%E3%81%A8%E3%81%8C%E3%81%A7%E3%81%8D%E3%82%8B%E3%80%82)

> 拡張モジュールをビルドする低レベルなビルドツール。autoconfやautomake m4等のビルドツールが別途必要になる。これを使用することにより、PHPをソースから再コンパイルすることなく拡張モジュールをビルドすることができる。

Amazon Linux では、デフォルトで使えるのだろうか。  
と試してみる。  

```
bash-4.2# phpize
Can't find PHP headers in /usr/include/php
The php-devel package is required for use of this command.
```

どうやらコマンドそのものは生きているらしい。  

php-devel が必要らしいので、ブッ込んでみる。  

```
sudo yum install -y php-devel

（中略）
Complete!
```

以下のように、メッセージが変わりました。
```
bash-4.2# phpize
Cannot find config.m4. 
Make sure that you run '/usr/bin/phpize' in the top level source directory of the module
```

気を取り直して、「php pickle install redis」を再実行。  

```
bash-4.2# php pickle install redis
  - Installing redis (latest-stable): Downloading (100%)
+-----------------------------------+--------+
| Package name                      | redis  |
| Package version (current release) | 5.3.7  |
| Package status                    | stable |
+-----------------------------------+--------+
use system liblzf (default: ): 
use system libsztd (default: ): 
use system liblz4 (default: ): 
whether to enable sessions (default: yes): 
whether to enable json serializer support (default: yes): 
whether to enable igbinary serializer support (default: no): 
whether to enable msgpack serializer support (default: no): 
whether to enable lzf compression (default: no): 
whether to enable Zstd compression (default: no): 
whether to enable lz4 compression (default: no): 


The following error(s) happened: configure failed, see log at /tmp/pickle-4e045fea2efae63727f7c1886b7851521df98c92\config.log
Would you like to read the log?1: phpize
2: Configuring for:
2: PHP Api Version:         20200930
2: Zend Module Api No:      20200930
2: Zend Extension Api No:   420200930
1: /tmp/redis/redis-5.3.7/configure --enable-redis=shared  --without-liblzf --without-libzstd --without-liblz4
2: checking for grep that handles long lines and -e... /usr/bin/grep
2: checking for egrep... /usr/bin/grep -E
2: checking for a sed that does not truncate output... /usr/bin/sed
2: checking for pkg-config... no
2: checking for cc... no
2: checking for gcc... no
2: configure: error: in `/tmp/pickle-4e045fea2efae63727f7c1886b7851521df98c92':
2: configure: error: no acceptable C compiler found in $PATH
2: See `config.log' for more detailsbash-4.2
```

ダメでした。  
そろそろ心が折れそう。  

see log at... って言ってるけど、そんなファイル生成されてないぞ。  

試しに「pickle redis configure failed」でググってみるも、解決策は見つからず。  

とういうか何なんだこのパッケージマネージャーは。  

一応、pickle を使っているサイトは見かけたが、どれも「pickle install redis」というコマンド。  
えぇぇ。。こっちだと「command not found」って出るから、頭に "php" 付けてるのに...  

https://niwacchi.hatenablog.com/entry/2021/08/08/103347

https://github.com/docker-library/php/issues/1118

いくら調べても、解決に役に立ちそうな情報は見つからず。  

Amazon Linux 特有の事情でもあるんかな、と思い「pickle Amazon Linux」で検索しても、それらしいものは見つからず。  

これは、pickle を使うのは諦めた方がよさそう。  


## pecl でインストールしてみる
公式の内容  
https://github.com/phpredis/phpredis/blob/develop/INSTALL.markdown  

> pecl install redis  
>  
> // If using PHP >= 7.3  
> pickle install redis  

PHP 7.3 以上は pickle を使え、と言ってるが、できなかったんで pecl を使ってみよう。  

pecl はデフォルトでは使えないみたいなので、有効化する必要がある。  

[AWS EC2 AmazonLinux2 peclコマンドを使用できる様にする](https://qiita.com/miriwo/items/112e66e127a8d801d429)  


```
bash-4.2# sudo yum install http://rpms.famillecollet.com/enterprise/remi-release-7.rpm
Loaded plugins: ovl, priorities
(中略)
Error: Package: remi-release-7.9-3.el7.remi.noarch (/remi-release-7)
           Requires: epel-release = 7
 You could try using --skip-broken to work around the problem
 You could try running: rpm -Va --nofiles --nodigest
```

epel-release が必要らしい。  

という事で、用意した上で再実行。
```
amazon-linux-extras enable epel
yum install -y epel-release
yum install -y http://rpms.famillecollet.com/enterprise/remi-release-7.rpm


Complete!
```
大丈夫みたいだ。  



sudo yum -y --enablerepo=remi-php74 install php-pear


sudo yum -y --enablerepo=remi-php74 install php-pear

```
bash-4.2# pecl
Commands:
```
という事で、インストール。


```
bash-4.2# pecl install redis
WARNING: channel "pecl.php.net" has updated its protocols, use "pecl channel-update pecl.php.net" to update
（中略）
running: phpize
Can't find PHP headers in /usr/include/php
The php-devel package is required for use of this command.
ERROR: `phpize' failed
```
phpize は pickle を触ってる時も出てきたな。  
という事で、以下を実行。  
```
sudo yum install -y php-devel
pecl install redis
```
実行結果
```
configure: error: in `/var/tmp/pear-build-defaultusermXZdYX/redis-5.3.7':
configure: error: no acceptable C compiler found in $PATH
See `config.log' for more details
ERROR: `/var/tmp/redis/configure --with-php-config=/usr/bin/php-config --enable-redis-igbinary=no --enable-redis-lzf=no --enable-redis-zstd=no' failed
```
いくつか対話形式の質問が来たが、全部デフォルトにしてインストールしてみるも、こんな感じ。  

ググっても解決につながる情報が見つからないし、ダメなパターンかな。これ。  


## docker-php-ext-install でインストールしてみる
こんな方法があるみたい。  
[DockerでRedisのコンテナを作成しLaravelと接続する](https://zenn.dev/marushin/articles/4903cd0bbbcee0)  

> FROM php:7.4-fpm  
>  
> RUN git clone https://github.com/phpredis/phpredis.git /usr/src/php/ext/redis && \  
>   docker-php-ext-install redis  

実行
```
git clone https://github.com/phpredis/phpredis.git /usr/src/php/ext/redis
yum install docker-php-ext-install
```
実行結果
```
bash-4.2# docker-php-ext-install redis
bash: docker-php-ext-install: command not found
```
docker-php-ext-install が使えるようになればいいのか？  

という事で、実験。  
```
bash-4.2# yum install docker-php-ext-install
Loaded plugins: ovl, priorities
amzn2-core
No package docker-php-ext-install available.
Error: Nothing to do

bash-4.2# amazon-linux-extras | grep docker-php-ext-install
```
ダメっぽい。  

調べてみると、docker-php-ext-install はコンテナに事前に入ってる拡張機能をつかうためのものらしい。  

[本番運用に備えたPHP7.3.0のコンテナを作る](https://qiita.com/tomoyamachi/items/ff5ac4a96063a12409cd)  

> docker-php-ext-configureとdocker-php-ext-installは、コンテナイメージに事前に圧縮されて入っているエクステンションを操作できます。  
>  
> configure : 設定をカスタマイズしたいものの設定を行う ex) gd, intl  
> install : 解凍してインストール  

[Dockerの公式PHPのDockerfileを頑張って読んで理解しようとしてみた](https://unskilled.site/docker%E3%81%AE%E5%85%AC%E5%BC%8Fphp%E3%81%AEdockerfile%E3%82%92%E9%A0%91%E5%BC%B5%E3%81%A3%E3%81%A6%E8%AA%AD%E3%82%93%E3%81%A7%E7%90%86%E8%A7%A3%E3%81%97%E3%82%88%E3%81%86%E3%81%A8%E3%81%97%E3%81%A6/)  

> docker-php-ext-*はdocker-php-ext-configure、docker-php-ext-enable、docker-php-ext-installの3つのスクリプトのことで、PHPエクステンションを簡単にインストール・有効化するために用意されたものです。

|  スクリプト                     |  役目                            |
|:---------------------------|:-------------------------------|
|  docker-php-ext-configure  |  引数を元にphpizeやconfigure実行してくれる  |
|  docker-php-ext-install    |  引数を元にエクステンションをインストールしてくれる     |
|  docker-php-ext-enable     |  引数を元にエクステンションを有効にしてくれる        |

[【PHP・Laravel】peclとは何か？pecl install と docker-php-ext-installの違い。docker-php-ext-enableなどの使い方を実例で解説（pearとの違い）](https://prograshi.com/language/php/pecl-install-docker-php-ext-install/)

> docker-php-ext-installはPHPに標準で備わっている拡張パッケージのインストール＆有効化ができます。  
>
> pecl installはPHPに標準で備わっていないパッケージをインストールできます。  

Amazon Linux は PHP に特化してるわけじゃないんで、こういった拡張項目が入ってないものと思われます。  

という事で、この方法でのインストールは諦めます。  


## yum でインストール（リポジトリを追加）
以下では、yum でインストールできたみたい。  
[Amazon Linux 2 にphp-pecl-redis入れる](https://jpdebug.com/p/2508550)

このコマンドで行けるらしい。  

> sudo yum install php-pecl-redis  
> sudo systemctl restart php-fpm  

。。。って、冒頭で失敗したやつじゃねえか。  

```
ash-4.2# sudo yum install php-pecl-redis 
Loaded plugins: ovl, priorities
No package php-pecl-redis available.
Error: Nothing to do
```

ダメでした。  

> うまくいかない場合にはお使いのPHPのバージョンのrepoが有効化されているか確認して下さい。

というものの、「お使いのPHPのバージョンのrepoが有効」するにはどうすれば？  

[CentOSでremiとEPELを使いphpのバージョンをアップ/ダウングレードする方法 | ソフトウェア開発のギークフィード](https://www.geekfeed.co.jp/geekblog/centos-remi-epel-php)  

> remiを使用するには、先に依存関係にあるEPELというリポジトリを追加します。
>
> yum install epel-release

という事で実行。  
```
bash-4.2# yum install epel-release
Loaded plugins: ovl, priorities
amzn2-core
No package epel-release available.
Error: Nothing to do
```

ダメっぽい。  

extras にはあるみたい。  
```
bash-4.2# amazon-linux-extras | grep epel
 24  epel                     available    [ =7.11  =stable ]
```

これを有効化したらいいのか？  
```
amazon-linux-extras enable epel
yum install -y epel-release
```

実行結果
```
Installed:
  epel-release.noarch 0:7-11

Complete!
```
上手くいったみたい。  

という事で、以下のコマンドを再び実行。
```
sudo yum install php-pecl-redis
```
実行結果
```
bash-4.2# sudo yum install php-pecl-redis
Loaded plugins: ovl, priorities
epel/x86_64/  etalink | 3.4 kB  00:00:00
epel
（中略）
Error: Package: php-pecl-redis-2.2.8-1.el7.x86_64 (epel)
（中略）
Error: Package: php-pecl-igbinary-1.2.1-1.el7.x86_64 (epel)
（中略）
Error: Package: php-pecl-redis-2.2.8-1.el7.x86_64 (epel)
（中略）
Error: Package: php-pecl-igbinary-1.2.1-1.el7.x86_64 (epel)
（中略）

bash-4.2# php -m | grep redis
bash-4.2#
```
ダメでした。  

もしかしたら、これが無いのが原因か？

> yum install -y https://rpms.remirepo.net/enterprise/remi-release-7.rpm

```
bash-4.2# yum install https://rpms.remirepo.net/enterprise/remi-release-7.rpm
Loaded plugins: ovl, priorities
remi-release-7.rpm

Complete!


bash-4.2# sudo yum install php-pecl-redis
Error（以下略）
```
結果、変わらず。  

何か解決策は無いかと探してみると、こんなのがあった。  

[PhpRedisのパッケージインストールをしたい](https://qiita.com/Emily-wata/items/9403d36192e5561c2747)  

まずはこのコマンドで検索。（PHP7 -> 8 に読み替えています）  
```
yum list | grep php8 | grep redis

php80-php-pecl-redis5.x86_64            5.3.7-1.el7.remi              remi-safe 
php80-php-phpiredis.x86_64              1.0.1-2.el7.remi              remi-safe 
php81-php-pecl-redis5.x86_64            5.3.7-1.el7.remi              remi-safe 
php81-php-phpiredis.x86_64              1.0.1-3.el7.remi              remi-safe 
```
見つかった。  

多分、初期ではリストに出てこなくて、「yum install -y https://rpms.remirepo.net/enterprise/remi-release-7.rpm」を実行した後に出てくるんじゃ無いかと思う。  

```
bash-4.2# php -v
PHP 8.0.16 (cli) (built: Mar  1 2022 00:31:45) ( NTS )
Copyright (c) The PHP Group
Zend Engine v4.0.16, Copyright (c) Zend Technologies
```

PHP のバージョンが8 なので、80 を使えばいいのかな。  
という事で実行。  

```
yum install -y php80-php-pecl-redis5.x86_64

（中略）

Complete!
```

上手く行ったみたいだぞ！  

以下のコマンドでインストール状況を確認できるらしい。  
```
yum list installed | grep redis
```
確認
```
bash-4.2# yum list installed | grep redis 
php80-php-pecl-redis5.x86_64     5.3.7-1.el7.remi             @remi-safe     
```
インストールされた事が確認できました。  

しかし、php もモジュール検索ではヒットせず。  
```
bash-4.2# php -m | grep redis
bash-4.2# 
```
有効化するための設定が必要なのだろうか。  

これが参考になりそう。  
[Redis と php-pecl-redis のインストールメモ（CentOS7.1.1503）](https://blog.apar.jp/linux/2959/)  

```
bash-4.2# systemctl enable redis
Created symlink /etc/systemd/system/multi-user.target.wants/redis.service, pointing to /usr/lib/systemd/system/redis.service.
bash-4.2# systemctl start redis
Failed to get D-Bus connection: Operation not permitted
bash-4.2# redis-cli
Could not connect to Redis at 127.0.0.1:6379: Connection refused
not connected> php -m | grep redis
Could not connect to Redis at 127.0.0.1:6379: Connection refused
```
何やら深遠な事情でエラーとなっている。  

これについては、考えられる原因がいくつかあるが、別トピックの話になるので次回に。  

推測だけど、Docker コンテナに PhpRedis をインストールして有効化する事はできない気がします。  
理由は続編にて。  



