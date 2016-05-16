#CentOS7のインストール
##VirtualBoxへのインストール
1.VirtualBoxを起動し、新規をクリック  
2.指示に従いながら、仮想マシンのメモリを1GB、ストレージの容量は8GBに設定し、作成  
3.設定からストレージの枠に行きストレージツリーの空をダウンロードしてきたCentOS7にする  
4.ネットワークの枠に行きネットワークアダプタ2の割当をホストオンリーアダプタにし、名前をvboxnet0にし、メニューに戻り起動  
5.CenOS7をインストールしパーティションの設定は特に指定せず、作成  
6.インストール中にroot以外のユーザーを作成  
##ネットワークアダプター1/2へのIPアドレスの設定とssh接続の確認
7.インストール終了後、作ったアカウントにログインし、/etc/sysconfig/network-scriptにあるifcfg-enp0s?というファイルの中身の"BOOTPROTO"をyesに、"ONBOOT"をyesにする  
8.コマンド"systemctl restart NetworkManager"を入力し、変更を更新、反映  
##ssh接続の確認
9.コマンドip aでアドレスを確認し、自分のターミナルから仮想マシンにssh接続  
##インストール後の設定、アップデー 
10.etc/yum.confの中に"proxy=https://172.16.40.1:8888"と"proxy=http://172.16.40.1:8888"を追加する  
11.usr/local/etc/に.wgetrcを作成し、"use_proxy=on","http_proxy=https://172.16.40.1:8888","http_proxy=http://172.16.40.1:8888"を記述する  
12.yumで"yum -y install php-mysql php php-gd php-mbstring"でphpをインストール  
13.yumで"yum -y install mariadb mariadb-server"でmysqlをインストール  
14."mysql -u root"でルートユーザーにパスワードを設定  
15."mysql -u root -p"で設定したパスワードでログイン  
16."CREATE DATABASE mysql CHARACTER SET utf8 COLLATE utf8_bin;"でmysqlという名前でデータベースを作る  
17.  
13.wgetでwordpressのURLを入力しファイルを取ってくる  
14.ダウンロードしたファイルを/var/www/html/に移動  
15.移動したファイルを解凍、展開  
16.ブラウザで、http://192.168.56.101/wordpress/wp-admin/setup-config.phpを開く  
17.mysqlで設定したデータベース名、ユーザー、パスワード、ホスト名を入力し次へ  
18.
