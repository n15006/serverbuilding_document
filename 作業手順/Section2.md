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
5.vagrantfileの中の
~~~~
Vagrant.configure(2) do |config|
......
.....
....
...
end
~~~~
の間に
~~~~
config.vm.network "private_network", ip:"好きなIPアドレス"
~~~~
を追記し閉じる  
6.`vagrant up`するとUSB 2.0 controller not foundのようなエラーを吐いたのでvirtualboxのサイトから以下をダウンロード
~~~~
Oracle_VM_VirtualBox_Extension_Pack-5.0.20-106931.vbox-extpack
~~~~
7.virtualboxを起動し環境設定の中の機能拡張からダウンロードしたパッケージを追加  
8.root権限で`vagrant up`をし、`vagrant ssh`でログイン  
#Wordpress設定
##2-2 Nginx + PHP + MariaDB
####yum設定
9./etc/yum.confの中に以下を追記  
~~~~
proxy=https://proxyアドレス:????
proxy=http://proxyアドレス:????
~~~~
10.`vi /etc/wgetrc`で中のproxyの行を変更  
~~~~
https_proxy = http://proxyアドレス:????/
http_proxy = http://proxyアドレス:????/
ftp_proxy = http://proxyアドレス:????/
~~~~
####proxy設定
11./etc/profileに以下を追記  
~~~~
PROXY='proxyアドレス:????'
export http_proxy=$PROXY
export HTTP_PROXY=$PROXY
export https_proxy=$PROXY
export HTTPS_PROXY=$PROXY
~~~~
12.`source /etc/profile`で変更点を更新  
####php設定  
13.`yum -y install php php-fpm php-mysql`でphpをインストール  
####nginx設定
14.`rpm -ivh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm`でリポジトリ追加  
15.エラーメッセージが出たので`yum -y install --skip-broken nginx`でインストール  
####nginxでphp-fpm動かす設定  
16./etc/php-fpm.d/www.confの中の`user = apache`と`group = apache`のapacheをnginxに書き換える  
17./var/www/に`echo '<?php echo phpinfo(); ?>' > index.php`でindex.htmlを作成
18./etc/nginx/conf.d/default.confの中を以下に書き換える
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
19.以下のコマンドでnginxとphp-fpmの自動起動設定  
~~~~
$ systemctl enable php-fpm.service
$ systemctl enable nginx.service
~~~~
20.default.confの内容を変更したのでnginxを再起動&php-fpm起動  
~~~~
$ systemctl restart nginx.service  
$ systemctl start php-fpm.service  
~~~~
21.`ip a`でipを確認しブラウザでアクセスし、phpinfo()が表示されればOK  
####MariaDB設定  
22.以下のコマンドででmariadbとmariadb-serverダウンロード&インストール  
~~~~
$ yum -y install mariadb mariadb-server
~~~~
23.自動起動設定  
~~~~
$ systemctl enable mariadb.service
~~~~
24.`systemctl start mariadb`で起動  
25.`mysql_secure_installation`で初期設定とパスワード設定  
26.`mysql -u root -p`を実行し設定したパスワードでログイン  
27.`create database データベース名`でデータベース作成  
28.以下のコマンドでユーザー作成  
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
38.`ip a`でipアドレスを確認し、ブラウザで`ipアドレス/html/wordpress/wp-admin/install.php`を開く  
39.各項目を入力し、wordpressのページが表示されればOK  
##2-3 Apache HTTP Server2.2 + PHP7.0 + (MySQL or MariaDB)
####apashe2.2の設定
1.ホームでapasheをダウンロード&展開  
~~~~
$ wget http://ftp.riken.jp/net/apache//httpd/httpd-2.2.31.tar.gz
$ tar -xvzf httpd-2.2.31.tar.gz
~~~~
2.ディレクトリに移動しビルド&コンパイル  
~~~~
$ cd httpd-2.2.31
$ ./configure
$ make
$ make install
~~~~
3.apache起動  
~~~~
$ /usr/local/apache2/bin/apachectl start
~~~~
4.httpd: Could not reliably....FQDNがおかしいので以下で修正  
~~~~
$ vi /usr/local/apache2/conf/httpd.conf

#ServerName www.example.com:80  
ServerName localhost:80  ←追記  
~~~~
5.apache再起動  
~~~~
/usr/local/apache2/bin/apachectl restart
~~~~
####php7.0の設定
6.ホームでphp7.0をダウンロード&展開  
~~~~
wget http://jp2.php.net/get/php-7.0.6.tar.bz2/from/this/mirror
tar -xvf mirror
~~~~
7.ディレクトリに移動しビルド  
~~~~
$ cd php-7.0.6
$ ./configure --with-apxs2=/usr/local/apache2/bin/apxs --with-mysqli
~~~~
8.libxml2がインストールされてなかったからエラー吐いた、以下で解決  
~~~~
yum install -y libxml2 libxml2-devel
~~~~
9.再度ビルド  
~~~~
$ ./configure --with-apxs2=/usr/local/apache2/bin/apxs --with-mysqli
$ make
$ make install
~~~~
10.php.ini作成のため以下コマンド  
~~~~
$ find / -name "php.ini-development" -ls
$ cp php.ini-development /usr/local/lib/php.ini
$ /usr/local/apache2/bin/apachectl restart
~~~~
####mariadbの設定
10.mariadbのインストール  
~~~~
$ yum -y install mariadb mariadb-devel mariadb-server
~~~~
11.mariadb起動、ログイン、データベース、ユーザー作成  
~~~~
$ systemctl start mariadb
$ mysql -u root -p
MariaDB [(none)]> create database database名;
MariaDB [(none)]> GRANT ALL PRIVILEGES ON データベース名.* TO "管理ユーザ"@"localhost" IDENTIFIED BY "パスワード"; 
MariaDB [(none)]> flush privileges;
MariaDB [(none)]> exit
~~~~
12.mariadbリスタート  
~~~~
$ systemctl restart mariadb
~~~~
####wordpressの設定
13.ホームでwordpressをダウンロード&展開  
~~~~
$ wget https://ja.wordpress.org/latest-ja.tar.gz
$ tar -xvf 
~~~~
14.htdocsにファイルを移動  
~~~~
$ mv wordpress/ /usr/local/apache2/htdocs
$ cd /usr/local/apache2/htdocs/
$ mv wordpress/* ./
~~~~
15.httpd.confを編集  
~~~~
$ vi /usr/local/apache2/conf/httpd.conf

<IfModule dir_module>
    DirectoryIndex index.html ←この行にindex.phpを追記
</IfModule>

行の最後尾に追記
<FilesMatch "\.ph(p[2-6]?|tml)$">
    SetHandler application/x-httpd-php
</FilesMatch>
~~~~
16.wordpress起動&各種入力  
~~~~
(ブラウザで)ipアドレス/html/wordpress/wp-admin/install.php
localhostのところに127.0.0.1と入力
~~~~
