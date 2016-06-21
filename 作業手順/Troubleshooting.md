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
#
./configure only
php download 
同じ手順
libxml2-devel download
./configure --with-apxs2=/usr/local/apache2/bin/apxs --with-mysqli
make
make install
/usr/local/apache2/bin/apachectl start
 yum -y install mariadb mariadb-servercd
cd /usr/local/apache2/htdocs/の中にwordpressの中身をぶちまける

