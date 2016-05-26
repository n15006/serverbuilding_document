#Vagrant設定
1.先生からCentos7のboxファイルを受け取る  
2.`以下のコマンドでboxをvagrantに登録する  
~~~~
$ vagrant box add CentOS7 ~/ファイルのある場所/CentOS7-1503.box --force
~~~~
3.vagrantの作業用ディレクトリを作り、`vagrant init`でvagrantfileを作る  
4.viでvagrantfileを開き
~~~~
config.vm.box = "base"
~~~~~
を
~~~~
config.vm.box = "CentOS7"
~~~~
に書き換える  
5.vagrantfileの中の`Vagrant.configure(2) do |config|`~`end`の間に`config.vm.network "private_network", ip:"好きなIPアドレス"`を追記し閉じる  
6.`vagrant up`するとUSB 2.0 controller not foundのようなエラーを吐いたのでvirtualboxのサイトから`Oracle_VM_VirtualBox_Extension_Pack-5.0.20-106931.vbox-extpack`をダウンロードしてくる  
7.virtualboxを起動し環境設定の中の機能拡張からダウンロードしたパッケージを追加  
8.root権限で`vagrant up`をし、`vagrant ssh`でログイン  
#Wordpress設定
####yum設定
9./etc/yum.confの中に以下を追記  
~~~~
proxy=https://proxyアドレス:????
proxy=http://proxyアドレス:????
~~~~
10.`yum -y install wget`でwgetをインストール
11.`vi /etc/wgetrc`で中のproxyの行を変更
~~~~
https_proxy = http://proxyアドレス:????/
http_proxy = http://proxyアドレス:????/
ftp_proxy = http://proxyアドレス:????/
~~~~
####proxy設定
12./etc/profileに以下を追記  
~~~~
PROXY='proxyアドレス:????'
export http_proxy=$PROXY
export HTTP_PROXY=$PROXY
export https_proxy=$PROXY
export HTTPS_PROXY=$PROXY
~~~~
13.`source /etc/profile`で変更点を更新  
####php設定  
14.`yum -y install php php-fpm`でphpをインストール  
15./var/www/にecho '<?php echo phpinfo(); ?>' > index.phpでindex,htmlを作成  
####nginx設定
16.`rpm -ivh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm`でリポジトリ追加  
17.エラーメッセージが出たので`yum -y install --skip-broken nginx`でインストール  
####nginxでphp-fpm動かす設定  
18./etc/php-fpm.d/www.confの中の`user = apache`と`group = apache`のapacheをnginxに書き換える  
19./etc/nginx/conf.d/default.confの中を以下に書き換える
~~~~
  root   /var/www/;
  index  index.php;

 location ~ \.php$ {   
        root           /var/www/;  
        fastcgi_pass   127.0.0.1:9000;  
        fastcgi_index  index.php;  
        fastcgi_param  SCRIPT_FILENAME  /var/www$fastcgi_script_name;  
        include        fastcgi_params;  
     }
~~~~  
に書き換える  
20.以下のコマンドでnginxとphp-fpmの自動起動設定  
~~~~
$ systemctl enable php-fpm.service
$ systemctl enable nginx.service
~~~~
21.default.confの内容を変更したのでnginxを再起動&php-fpm起動  
~~~~
$ systemctl restart nginx.service  
$ systemctl start php-fpm.service  
~~~~
22.`ip a`でipを確認しブラウザでアクセスし、phpinfo()が表示されればOK  
####MariaDB設定  
23.以下のコマンドででmariadbとmariadb-serverダウンロード&インストール  
~~~~
$ yum -y install mariadb mariadb-server
~~~~
24.自動起動設定  
~~~~
$ systemctl enable mariadb.service
~~~~
25.`systemctl start mariadb`で起動  
26.`mysql_secure_installation`で初期設定とパスワード設定  
27.`mysql -u root -p`を実行し設定したパスワードでログイン  
28.`create database データベース名`でデータベース作成  
29.以下のコマンドでユーザー作成  
~~~~
Mariadb>  GRANT ALL PRIVILEGES ON データベース名.* TO "管理ユーザ"@"localhost" IDENTIFIED BY "パスワード";
~~~~
29.`flush privileges;`で設定を更新  
####wordpress設定  
30.以下のコマンドを実行しwordpressをダウンロード  
~~~~
$ wget https://ja.wordpress.org/latest-ja.tar.gz
~~~~
31.ダウンロードしたファイルを`gunzip ファイル名で解凍`、`tar -xf ファイル名`で展開  
32./var/www/html/に解凍、展開したファイルを移動  
33.`chown -R nginx:nginx /var/www/html/wordpress/*`で権限変更  
34./var/www/html/wordpress/の中にある`wp-config-sample.php`をcpコマンドで`wp-config.php`に名前を変えてコピーする  
35.wp-config.phpの中のデータベース名、ユーザー名、パスワードをmysqlで設定した内容に書き換える  
36.以下のコマンドでポートを開ける  
~~~~
$ iptables -A INPUT -p tcp --dport 80 -j ACCEPT  
$ iptables -A INPUT -p tcp --dport 443 -j ACCEPT  
$ iptables -L  
~~~~  
37./etc/selinux/configの中の`SELINUX=enable`を`SELINUX=disabled`に変更しSELINUXを停止  
