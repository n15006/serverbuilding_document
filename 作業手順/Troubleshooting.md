#yumのlockが解除されなくなった場合の対処方法
yumを強制終了した後に再度yumを実行すると以下の文が時折出る  
~~~~
Another app is currently holding the yum lock; waiting for it to exit...
~~~~
これはyum.pid(yumのロックファイル)が何らかの理由で削除されなかった場合に発生するらしい、ので以下の文でyum.pidを消して直した  
~~~~
$ rm -f /var/run/yum.pid
~~~~
#Apacheが動かない(Active: failed)
以前にユーザ・グループ設定が無い状態(コメントアウトとか)で起動して強制終了させたからかもしれない、ので以下で調べる  
~~~~
ps aux | grep httpd
~~~~
該当するタスク(左から２番めの数字)を停止  
~~~~
kill ????
~~~~
#wordpress起動後サイトの表示がit works
試したこと
~~~~
ブラウザのキャッシュクリア
Ctrl + F5
~~~~
~~~~
index.htmlの削除
~~~~
#ブラウザでバージョンなどの確認
右クリック→検証→NetWork→ブラウザリロード→下タブのアドレス(192....)でサーバーやPHPの確認ができる
#apacheの圧縮転送の時の話
httpd.confに以下を記述した  
~~~~
<IfModule mod_deflate.c>
     SetOutputFilter DEFLATE
     AddOutputFilterByType DEFLATE <type>
</IfModule>
~~~~
これで圧縮転送の設定ができてるはず、と思ったら出来てない  
いろいろ調べてみるとmod_deflateがapacheに組み込まれていなかった、これを解決するには以下の方法がある  
~~~~
ソースからインストール（Apacheに組み込む場合）

Apacheのインストールを最初からやり直すことで、Apache本体へmod_deflateモジュールを組み込むことができます。Apache本体に組み込むと、モジュールのロードにかかる時間を節約できます。

　Apache本体に組み込むには、Apacheのconfigure実行時に「--enable-deflate」オプションを追加します。

# cd /Apacheのソース/
# ./configure --enable-deflate
# make
# make install
~~~~
~~~~
ソースからインストール（DSOによる動的組み込みの場合）

　DSO（Dynamic Shared Object：Apacheの基本インストールの「DSOとapxs」参照）を利用すれば、インストール済みのApacheにmod_deflateモジュールを追加することができます。

　DSOを利用してモジュールを組み込む場合は、まずmod_deflate.soファイルを作成してインストールします。mod_deflate.soのソースはApacheソースの中のmodules/filters/にあります。そのディレクトリでapxsコマンドを実行します。

# cd /Apacheのソース/modules/filters/
# /usr/local/apache2/bin/apxs -i -a -c mod_deflate.c
注：apxsのパスは適宜変更（以下同）。

　apxsコマンドに「-i」「-a」オプションを追加することで、make後にモジュールを規定の場所に移動し、httpd.confにモジュールをロードする設定が追加されます。「-c」オプションは、モジュールのソースファイルを指定します。

　この後「httpd.confの編集」の作業を行うのですが、以下のようなメッセージが出力されてモジュールの組み込みに失敗する場合があります。

Cannot load /usr/local/apache2/modules/mod_deflate.so into server: /usr/local/apache2/modules/mod_deflate.so: undefined symbol: deflate

　この場合は、mod_deflate.soに必要なライブラリを静的に組み込む必要があります。大抵はzlibを取り込むことで解決します。モジュール作成時にzlibを組み込むには、「-lz」オプションを追加します。

# /usr/local/apache2/bin/apxs -i -a -c mod_deflate.c -lz
~~~~
[参考にしたサイト(http://www.atmarkit.co.jp/ait/articles/0510/07/news107_2.html)](http://www.atmarkit.co.jp/ait/articles/0510/07/news107_2.html)

-k, --ask-pass
SSH のパスワードを尋ねる(プロンプトが出る)
~~~~
例
$ ansible -k
SSH password: vagrant
~~~~

-i INVENTORY, --inventory-file=INVENTORY
インベントリファイルを指定する(デフォルトは /etc/ansible/hosts)
~~~~
例
$ ansible -i hosts
~~~~

-u REMOTE_USER, --user=REMOTE_USER
SSH で接続するユーザー名を指定する
~~~~
例
-u vagrant
~~~~
#vagrantでpluginが導入できない
vagrantはapt-getでインストールするとなんかコケる(バグらしい)、ので公式の方からダウンロードしてくる  
[公式https://www.vagrantup.com/downloads.html](https://www.vagrantup.com/downloads.html)  
インストール  
~~~~
dpkg -i vagrant_1.8.4_x86_64.deb
~~~~
