【Amazon Linux】 GitHub CLI のインストールを諦めました

_______________________________________________________________________________
最近、GitHub CLI を使ってるんで、Amazon Linux でも使ってみようかと思ってみた。  

GitHub CLI とは、Git の操作をコマンドラインで実行するためのツールです。  
AWS CLI みたいなもんだと思ってください。  

インストール方法を検索するも、Mac・Windows・Debian系ディストリのインストール方法は簡単にヒットするのに、Debian 系だとさっぱりヒットしない。  
何やら深遠な事情があるのだろうか。  

ややこしい方法ではあるものの、Amazon Linux（Debian系ディスト）で取れる方法がいくつかありそうなので調べてみる。  

（GitHbu CLI 公式サイト）  
https://github.com/cli/cli  

yum でのインストールはできないみたい。  

取れる手段は Conda を使う方法と Spack を使う方法だろうか。  

## Conda を使う（断念）
Python のパッケージマネージャー。  
PHP で言う Composer みたいなもん。  
pip と何が違うんだろう。パッケージマネージャーの世界は、どの言語の界隈でもカオスなのだろうか・・・？  

Amazon Linux に conda をインストールする方法を探してみました。  
ヒットした内容を色々試してみましたが、見事に全滅しました。  
しかも、どれもお手軽には行けなさそうなものばかりです。  

また、Amazon Linux 2 の Python のバージョンは 2.7 で、最近は 3系を使う事が前提に説明していう記事がヒットしやすく、pyenv を使って色々やったりしてて、「別にそこまでヘビーな事したくねーよ！」と思い、採用を踏みとどまりました。  

というか、サーバーマシンには、極力余計な事したくないっちゅーに。  

## Spack を使う
[Spackによるソフトウェア管理 - ABCI 2.0 User Guide](https://docs.abci.ai/ja/tips/spack/#:~:text=Spack%E3%81%A8%E3%81%AF%E3%80%81Lawrence%20Livermore,%E4%BD%BF%E3%81%86%E3%81%93%E3%81%A8%E3%81%8C%E3%81%A7%E3%81%8D%E3%81%BE%E3%81%99%E3%80%82)  

> Spackとは、Lawrence Livermore National Laboratoryで開発されている高性能計算向けのソフトウェアパッケージ管理システムです。 Spackを使うことにより、同一ソフトウェアをバージョン、設定、コンパイラを様々に変えて複数ビルドし、切り替えて使うことができます。  

という事らしい。  

インストールコマンド、こんな感じ。  

> git clone https://github.com/spack/spack.git  
> cd ./spack  
> git checkout v0.16.0  
> source ${HOME}/spack/share/spack/setup-env.sh  

ガイドに従い、実行してみる。  
```
[root@localhost vagrant]# git clone https://github.com/spack/spack.git
Cloning into 'spack'...
remote: Enumerating objects: 368685, done.
remote: Counting objects: 100% (327/327), done.
remote: Compressing objects: 100% (129/129), done.
remote: Total 368685 (delta 203), reused 260 (delta 154), pack-reused 368358
Receiving objects: 100% (368685/368685), 165.63 MiB | 11.30 MiB/s, done.
Resolving deltas: 100% (157538/157538), done.
[root@localhost vagrant]# cd ./spack
[root@localhost spack]# git checkout v0.16.0
Note: switching to 'v0.16.0'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at 77d9acb071 update CHANGELOG.md for v0.16.0
[root@localhost spack]#  source ${HOME}/spack/share/spack/setup-env.sh
bash: /root/spack/share/spack/setup-env.sh: No such file or directory
```
失敗。  
次行こう次。  

こんなのが見つかりました。  
[spack package managerを使って最新のgccをインストール – Nabe International | Takeru Boost >> Takeruを回せ！ >>](https://www.nabe-intl.co.jp/takeruboost/spack-package-manager%E3%82%92%E4%BD%BF%E3%81%A3%E3%81%A6%E6%9C%80%E6%96%B0%E3%81%AEgcc%E3%82%92%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB/)  

> git clone https://github.com/spack/spack  
> cd spack/bin  
> ./spack [コマンド]  
> 
> ## bash/zshの場合は.bashrc/.zshrcに下記行を追記してください  
> export SPACK_ROOT=~/spack   # PATHはspackをダウンロードしたディレクトリに合わせてください  
> . $SPACK_ROOT/share/spack/setup-env.sh  
>   
> source .bashrc  

という事で実験。  
以下、実行結果。  

```
[vagrant@localhost tmp2]$ git clone https://github.com/spack/spack
Cloning into 'spack'...
remote: Enumerating objects: 368685, done.
remote: Counting objects: 100% (326/326), done.
remote: Compressing objects: 100% (128/128), done.
remote: Total 368685 (delta 202), reused 260 (delta 154), pack-reused 368359
Receiving objects: 100% (368685/368685), 166.71 MiB | 11.04 MiB/s, done.
Resolving deltas: 100% (157436/157436), done.
[vagrant@localhost tmp2]$ cd spack/bin
[vagrant@localhost bin]$ ./spack 
usage: spack [-hkV] [--color {always,never,auto}] COMMAND ...

A flexible package manager that supports multiple versions,
configurations, platforms, and compilers.

These are common spack commands:

query packages:
  list                  list and search available packages
```

spack のインストールは上手くいったみたいだ！  

続いて、GitHub CLI をインストール。  
インストールコマンドは以下。  
```
./spack install gh
```
以下、実行結果。  
```
vagrant@localhost bin]$ ./spack install gh
==> Bootstrapping clingo from pre-built binaries
==> Fetching https://mirror.spack.io/bootstrap/github-actions/v0.2/build_cache/linux-rhel5-x86_64/gcc-9.3.0/clingo-bootstrap-spack/linux-rhel5-x86_64-gcc-9.3.0-clingo-bootstrap-spack-gypv5loj2ml73duq6sr76yg5rj25te2m.spack
==> Installing "clingo-bootstrap@spack%gcc@9.3.0~docs~ipo+python+static_libstdcpp build_type=Release arch=linux-rhel5-x86_64" from a buildcache
==> Bootstrapping patchelf from pre-built binaries
==> Installing patchelf-0.13.1-pzekyoku47x6zebqvsxcxsydirrqler7
==> No binary for patchelf-0.13.1-pzekyoku47x6zebqvsxcxsydirrqler7 found: installing from source
==> Fetching https://github.com/NixOS/patchelf/releases/download/0.13.1/patchelf-0.13.1.tar.gz
==> No patches needed for patchelf
==> patchelf: Executing phase: 'autoreconf'
==> patchelf: Executing phase: 'configure'
==> patchelf: Executing phase: 'build'
==> Error: ProcessError: Command exited with status 2:
    'make' 'V=1'

2 errors found in build log:
     36    Making all in src
     37    make[1]: Entering directory `/tmp/vagrant/spack-stage/spack-stage-patchelf-0.13.1-pzekyoku47x6zebqvsxcxsydirrqler7/spack-src/src'
     38    source='patchelf.cc' object='patchelf.o' libtool=no \
     39    DEPDIR=.deps depmode=none /bin/sh ../build-aux/depcomp \
     40    g++ -DPACKAGE_NAME=\"patchelf\" -DPACKAGE_TARNAME=\"patchelf\" -DPACKAGE_VERSION=\"0.13.1.20211127.72b6d44\" -DPACKAGE_STRING=\"patchelf\ 0.13.1.20211127.72b6d4
           4\" -DPACKAGE_BUGREPORT=\"\" -DPACKAGE_URL=\"\" -DPACKAGE=\"patchelf\" -DVERSION=\"0.13.1.20211127.72b6d44\" -I.    -Wall -std=c++11 -D_FILE_OFFSET_BITS=64   -c
            -o patchelf.o patchelf.cc
     41    [spack cc] ERROR: Compiler 'gcc@7.3.1' does not have a C++ compiler configured.
  >> 42    make[1]: *** [patchelf.o] Error 1
     43    make[1]: Leaving directory `/tmp/vagrant/spack-stage/spack-stage-patchelf-0.13.1-pzekyoku47x6zebqvsxcxsydirrqler7/spack-src/src'
  >> 44    make: *** [all-recursive] Error 1

See build log for details:
  /tmp/vagrant/spack-stage/spack-stage-patchelf-0.13.1-pzekyoku47x6zebqvsxcxsydirrqler7/spack-build-out.txt

==> Bootstrapping clingo from sources
Killed
```

ダメっぽい。  

root ユーザで実行してもダメでした。  

## 結論
Amazon Linux で GitHub CLI を使うのは諦めましょう。（2022/4/24 時点）  

ちなみに、EC2 インスタンスを起動して色々実験すると、ハマッた時間の分だけ課金されるのが嫌だったので、Vagrant でローカルに Amazon Linux を起動して、その中で実験しました。  
もしかしたら、EC2 上で実行すると、また違った結果が得られるかもしれません。  

ですが、GitHub CLI の記事で、「各種OSのインストール方法」を解説している内容はいくつかヒットするものの、Debian系（Amazon Linux、CentOS 等）がことごとく避けられている事から、恐らく仮想マシンである事は関係ないような気がします。  

気が向いたら試してみたいと思わなくはないですが、そんな事よりもシュノーケリングやりてーです。  

ちなみに、Vagrant で起動したマシンにて、　/etc/system-release, /etc/os-release にてディストリを確認したみたところ、こんな感じで、EC2 の内容と同一である事を確認しました。  
```
[root@localhost vagrant]# cat /etc/system-release
Amazon Linux release 2 (Karoo)


[root@localhost vagrant]# cat /etc/os-release
NAME="Amazon Linux"
VERSION="2"
ID="amzn"
ID_LIKE="centos rhel fedora"
VERSION_ID="2"
PRETTY_NAME="Amazon Linux 2"
ANSI_COLOR="0;33"
CPE_NAME="cpe:2.3:o:amazon:amazon_linux:2"
HOME_URL="https://amazonlinux.com/"
```

