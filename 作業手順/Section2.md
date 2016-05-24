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
####nginx設定
15.`rpm -ivh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm`でリポジトリ追加  
16.エラーメッセージが出たので`yum -y install --skip-broken nginx`でインストール  
####nginxでphp-fpm動かす設定  
17./etc/php-fpm.d/www.confの中の`user = apache`と`group = apache`のapacheをnginxに書き換える  
18./etc/nginx/conf.d/default.confの中の`root   /usr/share/nginx/html;`を`root   /var/www;`、`index  index.html index.htm;`を`index  index.php;
`、  
` #location ~ \.php$ {  
    #    root           html;  
    #    fastcgi_pass   127.0.0.1:9000;  
    #    fastcgi_index  index.php;  
    #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;  
    #    include        fastcgi_params;  
    #}`  
を  
`location ~ \.php$ {   
        root           /var/www;  
        fastcgi_pass   127.0.0.1:9000;  
        fastcgi_index  index.php;  
        fastcgi_param  SCRIPT_FILENAME  /var/www$fastcgi_script_name;  
        include        fastcgi_params;  
     }`  
に書き換える
