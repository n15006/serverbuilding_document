#VirtualBoxとVagrantをインストールする。
1.ターミナルで"sudo apt-get install virtualbox"と~~"sudo apt-get install vagrant"~~を実行する  
2.vagrantはapt-getでインストールするとなんかコケる(バグらしい)、ので公式の方からダウンロードしてくる
[公式https://www.vagrantup.com/downloads.html](https://www.vagrantup.com/downloads.html)
3.インストール
~~~~
dpkg -i vagrant_1.8.4_x86_64.deb
~~~~
4.-vでバージョンとインストール確認
