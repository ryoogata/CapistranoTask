CapistranoTask
==============
Capistrano を利用してサーバを構築するテンプレートです。
特定の種類毎にレポジトリを作成はせず、Capistrano で記載しているテンプレートがすべてこのレポジトリに保存されています。

大きく分けて 2 種類のテンプレートが存在します。

* 小文字で始まるテンプレート  
補助的役割を果たすサーバやツールなどをインストールするためのテンプレート。
これ単体で利用する場合よりも、組み合わせて利用してサーバを構築するためのパーツとして利用することを目的としています。

* 大文字で始まるテンプレート  
Zabbix や FSWiki, Webistrano などのサービスをユーザに提供するようなサーバをインストールするためのテンプレート。
小文字で始まるテンプレートを組み合わせて、構成されています。  
大文字で始まるテンプレートには task :install が含まれています。


上記に記載のある 2 種類のテンプレート以外にも、特殊な用途で利用するファイルもこのレポジトリに保存されています。

* config.rb.sample  
config.rb のサンプルになります。実際に利用する場合には **.sample** を削除してください。

* nifty  
Capistrano の学習用に利用させてもらったファイルです。

* template  
新しく Capistrano のファイルを作成する際に利用するテンプレートです。


* default/example  
task の動作を確認する際に作成したファイルです。実際にこのファイルを利用してサーバの構築をすることはないです。


Requirements
------------
各 Capistrano のファイルの先頭に動作を確認している OS を記載しています。
基本的に AWS 上での利用を想定しているため、Amazon Linux や CentOS の yum でのパッケージ管理の利用を想定しています。

OS 依存があるようなものに限り Ubuntu を利用しているものもあります。


Attributes
----------
状況や構成に応じて一部設定を変更できるように Chef で言う Attributes の仕組みを取り入れています。
たとえば、Zabbix のバージョンを変更したい場合には、

set :_ZABBIX_VERSION, '2.0.6'

にある、2.0.6 を変更してあげるだけで指定されたバージョンの Zabbix がインストール可能になります。


Usage
-----
1. このレポジトリをローカルにコピーします  
  $ __git clone git://github.com/ryoogata/CapistranoTask.git__  

2. コピーした Directory へ cd  
  $ __cd CapistranoTask__

3. 設定ファイルの rename  
  $ __mv config.rb.sample config.rb__  

4. __config.rb__ の編集
 - __role :server__    
 capistrano を利用して操作したいサーバの FQDN もしくは IP Address を入力します。  
 Default では localhost が指定されています。  

 - __set :user__    
 操作対象サーバへ SSH でログインする際のユーザ名を指定します。  
 Default では ec2-user が指定されています。  

 - __ssh_options[:keys]__  
 鍵認証で操作対象サーバへ SSH ログインする際の key を指定します。

 - __ssh\_options[:auth_methods]__  
 操作対象サーバへ SSH ログインする方法を鍵認証に指定しています。パスワード認証によるログインを行う場合には本設定を削除もしくはコメントアウトしてください。

5. cap コマンドの実行  
下記のように cap コマンドを利用して -f の引数にファイル名とそのあとに実行したい Task を入力します。  
    
  $ __cap -f Zabbix serverTemplate:install__