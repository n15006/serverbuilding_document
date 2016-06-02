#6-0 AWSコマンドラインインターフェイスのインストール
1.pipが無かったのでインストール  
~~~~
$ sudo apt-get install python-pip
~~~~
2.awsコマンドラインインターフェイスをインストール  
~~~~
$ pip install awscli
~~~~
3.pipをアップグレードしてくださいと出たのでアップグレードする  
~~~~
$ pip install --upgrade pip
~~~~
#6-1 AWS EC2 + Ansible
####aws設定とssh接続
4.awsにログイン  
5.右上の地域の設定がオレゴンになってたので東京に設定する  
6.サービスの中からEC2を選択  
7.Launch Instanceをクリック  
8.OS(一番上のAmazon Linux AMI)を選択  
9.インスタンスの数やタイプ、その他詳細設定、ストレージデバイスをデフォルトにして進める  
10.Nameに任意のタグをつける  
11.セキュリティグループはデフォルトにし、80ポートと443ポートを追加  
12.任意のkye pairを設定し、作成  
13.自分のターミナルから作成したインスタンスにsshできないのでgrusのサーバーを使う  
14.ダウンロードしたkey pairをgruサーバーにアップロード  
~~~~
$ scp key_pair_name grus_server_address:~
~~~~
15.grusサーバーにssh接続  
~~~~
$ ssh address
~~~~
16.awsにssh接続  
~~~~
ssh -i key_pair_name instance_URL
~~~~
####
