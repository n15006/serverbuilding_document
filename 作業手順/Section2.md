#Vagrant設定
1.先生からCentos7のboxファイルを受け取る  
2.`vagrant box add CentOS7 ~/ファイルのある場所/CentOS7-1503.box --force`コマンドでboxをvagrantに登録する  
3.vagrantの作業用ディレクトリを作り、`vagrant init`でvagrantfileを作る  
4.viでvagrantfileを開き、`config.vm.box = "base"`の行を`config.vm.box = "CentOS7"`に変える  
5.vagrantfileの中の`Vagrant.configure(2) do |config|`~`end`の間に`config.vm.network "private_network", ip:"好きなIPアドレス"`を追記し閉じる  
6.`vagrant up`するとUSB 2.0 controller not foundのようなエラーを吐いたのでvirtualboxのサイトから`Oracle_VM_VirtualBox_Extension_Pack-5.0.20-106931.vbox-extpack`をダウンロードしてくる  
7.virtualboxの環境設定の中の機能拡張からダウンロードしたパッケージを追加  
8.root権限で`vagrant up`をし、`vagrant ssh`でログイン  
#Wordpress設定
####yum設定
9.`etc/yum.conf`の中に`proxy=https://172.16.40.1:8888`と`proxy=http://172.16.40.1:8888`を追加する
10.`yum -y install wget`でwgetをインストール
11.`vi /etc/wgetrc`で中のproxyの行を変更
####proxy設定
12.`/etc/profile`に`PROXY='172.16.40.1:8888'`,`export http_proxy=$PROXY`,`export HTTP_PROXY=$PROXY`,`export https_proxy=$PROXY`,`export HTTPS_PROXY=$PROXY`を追記
13.`source /etc/profile`で変更点を更新  
####php設定  
14.`yum -y install php`でphpをインストール  
15./var/www/にecho '<?php echo phpinfo(); ?>' > index.phpでindex,htmlを作成  
####nginx設定
16.`rpm -ivh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm`でリポジトリ追加  
17.エラーメッセージが出たので`yum -y install --skip-broken nginx`でインストール  
####nginxでphp-fpm動かす設定  
18./etc/php-fpm.d/www.confの中の`user = apache`と`group = apache`のapacheをnginxに書き換える  
19./etc/nginx/conf.d/default.confの中の`root   /usr/share/nginx/html;`を`root   /var/www/;`、`index  index.html index.htm;`を`index  index.php;
`、
~~~~  
 #location ~ \.php$ {  
    #    root           html;  
    #    fastcgi_pass   127.0.0.1:9000;  
    #    fastcgi_index  index.php;  
    #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;  
    #    include        fastcgi_params;  
    #}
~~~~  
を  
~~~~
 location ~ \.php$ {   
        root           /var/www/;  
        fastcgi_pass   127.0.0.1:9000;  
        fastcgi_index  index.php;  
        fastcgi_param  SCRIPT_FILENAME  /var/www$fastcgi_script_name;  
        include        fastcgi_params;  
     }
~~~~  
に書き換える  
20.`systemctl start php-fpm.service`でphp-fpmを起動  
21.`chkconfig php-fpm on`で自動起動設定  
22.default.confの内容を変更したので`systemctl restart nginx.service`でnginxを再起動  
####MariaDB設定  
23.`yum -y install mariadb mariadb-server`でmariadbとmariadb-serverをダウンロー&インストール  
24.`systemctl start mariadb`で起動  
25.`systemctl enable mariadb`で自動起動設定  
26.`mysql_secure_installation`で初期設定とパスワード設定  
27.`mysql -u root -p`を実行し設定したパスワードでログイン  
28.`create database ****`でデータベース作成  
29.flush privileges;で設定を更新  
30.wgetコマンドでwordpressのURL(https://ja.wordpress.org/latest-ja.tar.gz)を実行しファイルをダウンロード  
31.ダウンロードしたファイルを`gunzip ファイル名で解凍`、`tar -xf ファイル名`で展開  
32./var/www/html/に解凍、展開したファイルを移動  
33.`chown -R nginx:nginx /var/www/html/wordpress/*`で権限変更  
34.
~~~~
$ iptables -A INPUT -p tcp --dport 80 -j ACCEPT  
$ iptables -A INPUT -p tcp --dport 443 -j ACCEPT  
$ iptables -L  
~~~~
でポートを開ける
