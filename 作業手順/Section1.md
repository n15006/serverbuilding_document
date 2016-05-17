#CentOS7のインストール
##VirtualBoxへのインストール
1.VirtualBoxを起動し、新規をクリック  
2.指示に従いながら、名前、osをLinuxのRedHat(64bit)、仮想マシンのメモリを1GB、ストレージの容量は8GBに設定し、作成  
3.設定からストレージの枠に行きストレージツリーの空をダウンロードしてきたCentOS7にする  
4.ネットワークの枠に行きネットワークアダプタ2の割当をホストオンリーアダプタにし、名前をvboxnet0にし、メニューに戻り起動  
5.CenOS7をインストールしパーティションの設定は特に指定せず、作成  
6.インストール中にroot以外のユーザーを作成  
##ネットワークアダプター1/2へのIPアドレスの設定とssh接続の確認
7.インストール終了後、作ったアカウントにログインし、/etc/sysconfig/network-scriptにあるifcfg-enp0s?というファイルの中身の"BOOTPROTO"をyesに、"ONBOOT"をyesにする  
8.コマンド"systemctl restart NetworkManager"を入力し、変更を更新、反映  
##ssh接続の確認
9.コマンドip aでアドレスを確認し、自分のターミナルから仮想マシンにssh接続  
##インストール後の設定、アップデート 
####yum設定
10.etc/yum.confの中に"proxy=https://172.16.40.1:8888"と"proxy=http://172.16.40.1:8888"を追加する 
11."yum -y install wget"でwgetをインストール   
####proxy設定
12./etc/profileに"PROXY='172.16.40.1:8888'","export http_proxy=$PROXY","export HTTP_PROXY=$PROXY","export https_proxy=$PROXY","export HTTPS_PROXY=$PROXY"を追記  
13."source /etc/profile"で変更点を更新  
####php設定
14.yumで"yum -y install php"でphpをインストール  
15./var/www/htmlに"echo '<?php echo phpinfo(); ?>' > index.php"でindex.htmlを作成  
####apache設定
16.yumで"yum -y install httpd"でapacheをインストール  
17."systemctl enable httpd.service"で自動起動設定  
18."systemctl start httpd.service"でapacheを起動  
19."systemctl status httpd.service"でapacheの起動確認  
####mysql設定
20."yum -y install http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm"で公式リポジトリファイルをインストール  
21."yum -y install mysql","yum -y install mysql-devel","yum -y install mysql-server","yum -y install mysql-utilities"でmysqlをインストール  
22.
25.wgetでwordpressのURLを入力しファイルを取ってくる  
26.ダウンロードしたファイルを/var/www/html/に移動  
27.移動したファイルを"gunzip ファイル名"で解凍、"tar -xf ファイル名"で展開  
28.ブラウザで、http://192.168.56.101/wordpress/wp-admin/setup-config.phpを開く  
29.mysqlで設定したデータベース名、ユーザー、パスワード、ホスト名を入力し次へ  
30.
