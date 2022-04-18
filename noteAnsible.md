# パッケージリストはどこを見たら？
```yaml
    - name: Install nodejs
      yum:
        name: nodejs
        state: present
```

fatal: [localhost]: FAILED! => {"changed": false, "msg": "No package matching 'nodejs' found available, installed or updated", "rc": 126,   "results": ["No package matching 'nodejs' found available, installed or updated"]}

_____________________________________________
# get_url

```yaml

    - name: download bind
      get_url:
        url: https://rpm.nodesource.com/setup_16.x
        dest: .
    - name: installing latest version of node.js
      become: yes
      yum:
        name: nodejs
        state: present
```

fatal: [localhost]: FAILED! => {"changed": false, "msg": "No package matching 'nodejs' found available, installed or updated", "rc": 126, "results": ["No package matching 'nodejs' found available, installed or updated"]}



[WARNING]: Consider using the get_url or uri module rather than running 'curl'.  If you need to use command because get_url or uri is insufficient you can add 'warn: false' to
this command task or set 'command_warnings=False' in ansible.cfg to get rid of this message.


_____________________________________________

「 | bash -」を外すとエラーとなる。なぜ？
```yaml
    - name: setup nodejs6.x
      shell: curl -sL https://rpm.nodesource.com/setup_16.x | bash -
    - name: installing latest version of node.js
      yum:
        name: nodejs
        state: present
```


_____________________________________________
# dnf リポジトリ
https://docs.ansible.com/ansible/latest/collections/ansible/builtin/dnf_module.html


_____________________________________________
_____________________________________________
_____________________________________________
_____________________________________________
_____________________________________________
_____________________________________________
_____________________________________________
_____________________________________________
_____________________________________________
_____________________________________________
# Ansible

こういう構成

冪等性

## 構成
EC2 のインスタンスに、PHP、Node.js、nginx といった


## 環境を作るのは結構面倒くさい

ハタから見たら

remi 
うっわ。何これ？

Ansible を使う事で、その辺を抽象化して yaml ファイルに最低限のことを書くだけで後はよしなにやってくれるかと思いきや、
リポジトリの参照先を示さないと


## プレイブックを書くのが結構面倒くさい


## マニュアルが使いづらい
https://docs.ansible.com/ansible/2.9/index.html  

yaml



## 参考にする playbook が情報過多
上記のように


「CentOS」


## 構文が変わってる
構文が古いと怒られた

ポリシーが全く感じられない。  

よくある事だが、

「あー。確かにね。今まで統一感なかったもんね。」と受け入れる事はできるが、変更内容に全くポリシーを感じない。

探せばどこかに書いてあるかもしれないけど、


## 冪等性は結局書き手が担保する必要がある？
Ansible のメリットに、「冪等性」

多分、  
「何度実行しても同じ結果になる」  
じゃなくて、  
「何度実行しても同じ結果になるような記述を表現できる」  
が正しい気がする。  

というか、EC2 に適用する事を前提とするなら、EC2 インスタンスを一回ぶっ壊して、もっかい最初からやった方がよくね？　とは思っている。  


## 何だかんだ言って、直接触れないと不安
web サーバを

例えば、nginx のサービスをスタートする時、
```yaml
  service:
    name: nginx
    enabled: yes
```

ただ、

クッソ問題あるよな。


## 結論
Ansible 結構しんどいので、シェルスクリプトを使った方がいいんじゃないかな。（よっぽど巨大な構成でなければ）  

結構クセが強くて学習コストが高く、インフラ専属でないエンジニアが片手間に触るのは、なかなか厳しいんじゃないかと思います。※個人の感想です  
「そこまで Ansible の機能を使い倒さず、必要最小限の内容をシンプルに書く」という使い方もあるかもしれませんが、それだと「この程度なら、シェルスクリプトでよくね？」という内容になりがちなので、結局 Ansible の出番が無い気がします。  

というか、元々、冒頭で書いたような小さい構成のアプリに適用するような代物ではないのかもしれません。  
そう言う意味では、Ansible がどうこうと言うより、自分の技術選定がミスってたと言う事になります。  

ただ、安易に導入すると、後で余計な苦労を背負い込む事は分かったので、「Ansible 使うといいよ！」というアドバイスをもらったとしても、

なかなか地獄じみた運用をした事があるが、そんな感じ。

今後、自分が Ansible とどう向き合っていくかは決めていないが、インフラしか触らない人の「Ansible いいよ！」は、ちょっと疑いの目を向けて聞く事にしようと思う。（インフラの構築のみを請け負う会社の人なら、警戒心さらに高めに設定。この手の仕事って、一度作って納品してしまえば、納品先が運用苦しい内容になってても、基本知ったこっちゃないだろうし。）  

ただ、「バックエンドが専門だけど、インフラも触る」という人の「Ansible いいよ！」という意見なら、真剣に耳を傾けたいと思う。  


## どんな場面で Ansible を使うか（ブログ主の場合）
何百台のサーバを管理するような管理するようなガチのインフラの仕事を請けているわけじゃないし、その予定も特に無いので、使うとしたら別の用途となりそう。  

例えば、オープンソースのアプリケーションを公開して、ディストリごとの環境構築ファイルを playbook で用意する、という用途だろうか。  

でも、よく考えたらそういう事したいならコンテナを使う事を前提とした作りにするし、デプロイもコンテナを前提とするな。  

使うとしたら、ガチのインフラエンジニアに転向する時だろうか。（今のところ、その予定は無いけど。）  
どう考えても、バックエンドとプロマネやりながら出来る仕事じゃ無さそう。  



