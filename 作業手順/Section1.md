#CentOS7のインストール
##VirtualBoxへのインストール
1.VirtualBoxを起動し、新規をクリック  
2.指示に従いながら、名前、osをLinuxのRedHat(64bit)、仮想マシンのメモリを1GB、ストレージの容量は8GBに設定し、作成  
3.設定からストレージの枠に行きストレージツリーの空をダウンロードしてきたCentOS7にする  
4.ネットワークの枠に行きネットワークアダプタ2の割当をホストオンリーアダプタにし、名前をvboxnet0にし、メニューに戻り起動  
5.CenOS7をインストールしパーティションの設定は特に指定せず、作成  
6.インストール中にroot以外のユーザーを作成  
##ネットワークアダプター1/2へのIPアドレスの設定とssh接続の確認
7.インストール終了後、作ったアカウントにログインし、`/etc/sysconfig/network-script`にある`ifcfg-enp0s?`というファイルの中身の`BOOTPROTO`を`yes`に、`ONBOOT`を`yes`にする  
8.コマンド`systemctl restart NetworkManager`を入力し、変更を更新、反映  
####ssh接続の確認
9.コマンドip aでアドレスを確認し、自分のターミナルから仮想マシンにssh接続  
##インストール後の設定、アップデート 
####yum設定
10.`etc/yum.conf`の中に`proxy=https://172.16.40.1:8888`と`proxy=http://172.16.40.1:8888`を追加する 
11.`yum -y install wget`でwgetをインストール   
12.`vi /etc/wgetrc`で中のproxyの行を変更  
####proxy設定
13.`/etc/profile`に`PROXY='172.16.40.1:8888'`,`export http_proxy=$PROXY`,`export HTTP_PROXY=$PROXY`,`export https_proxy=$PROXY`,`export HTTPS_PROXY=$PROXY`を追記  
14.`source /etc/profile`で変更点を更新  
####php設定
15.yumで`yum -y install php php-mdstring php-mysql`でphpをインストール  
16.`/var/www/html`に`echo '<?php echo phpinfo(); ?>' > index.php`でindex.htmlを作成  
####apache設定
17.yumで`yum -y install httpd`でapacheをインストール  
18.`systemctl enable httpd.service`で自動起動設定  
19.`systemctl start httpd.service`でapacheを起動  
20.`systemctl status httpd.service`でapacheの起動確認  
####mysql設定
21.`yum -y install http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm`で公式リポジトリファイルをインストール  
22.`yum -y install mysql`,`yum -y install mysql-server`でmysqlをインストール  
23.`service mysqld start`でmysqlを起動  
24.`mysql_secure_installation`でmysqlの初期設定とパスワード設定  
25.`mysql -u root -p`で設定したパスワードでログイン  
26.mysqlのshell内で`create database @@@@;`でデータベースを作る  
27.`flush privileges;`で設定を更新  
##wordpress設定
28.wgetでwordpressのURLを入力しファイルを取ってくる  
29.ダウンロードしたファイルを/var/www/html/に移動  
30.移動したファイルを`gunzip ファイル名`で解凍、`tar -xf ファイル名`で展開  
31.`chown -R apache:apache /var/www/html/wordpress/*`で権限変更  
32.`iptables -A INPUT -p tcp --dport 80 -j ACCEPT`,`iptables -A INPUT -p tcp --dport 443 -j ACCEPT`,`iptables -L`でポートを開ける  
33.`sudo vi /etc/selinux/config`の中の文章を`SELINUX=disabled`に変更しSELINUXを停止
34.ブラウザで、http://192.168.56.102/wordpress/wp-admin/install.phpを開く  
35.mysqlで設定したデータベース名、ユーザー、パスワード、ホスト名を入力し次へ  
36.インストールを実行  
37.タイトル、名前、メールアドレスなどを設定  
