#Linuxの教科書まとめ
[小さな会社の新米サーバー/インフラ担当者のためのLinuxの常識](http://www.socym.co.jp/book/942)
##内容
####CAHPTER1 lINUXの基礎知識  
**1-1サーバーとは**  
・サービスを提供する側を**サーバー**、処理を要求する側を**クライアント**、この形式を**クライアント・サーバー**  
・応用ソフトウェアを動作させるための基本ソフトウェアを**オペレーティングシステム**、略して**OS**  
・堅牢性や専用のハードウェア等に対応したOSを**サーバーOS**  
**1-2代表的なサーバーソフトウェア**  
・Webサイトを公開するためのサーバを**Webサーバー**  
・電子メールをやり取りするためのサーバーを**メールサーバー**  
・ネットワーク上でファイルを共有できるようにするためのサーバーを**ファイルサーバー**  
・ドメイン名をIPアドレスに変換するサーバーを**DNSサーバー**  
・クライアントの代理としてWebサーバーやFTPサーバーにアクセスするサーバーを**プロキシサーバー**  
・Webアプリケーションを実行するための基盤を提供するサーバーを**アプリケーションサーバー**  
**1-3Linuxとは**  
・インターネット初期の頃を構成していたOSを**UNIX**、このUNIXと互換性のあるOSを**Linux**  
・ハードウェアの違いを吸収しアプリケーション・ソフトウェアが動作するための基盤を提供するOSの中核部分を**カーネル**  
・カーネルに様々なソフトウェアを組み合わせる1つのOSとして完成させたものを**ディストリビューション**  
**1-4Linuxの使いみち**  
・Linuxの主な使いみちはインターネットサーバーやイントラネットサーバー  
・スマートフォンのAndrooidの中核部分はLinux  
####CHAPTER2 Linuxの基本操作
**2-1コマンドを使った操作**  
・マウスを使ったり指でタッチする操作を**GUI**、キーボードから文字を入力して操作する方法を**CUI**といい、文字のことを**コマンド**  
・Linuxシステムを管理する権限を持ったユーザーを**管理者ユーザー**または**rootユーザー**という  
・システムプログラムやサーバーソフトウェアなどを実行するために用意されるユーザーを**システムユーザー**  
・あまり権限を持たない通常の利用者を**一般ユーザー**  
・Linuxにログインしコマンドを入力できる場所を**プロンプト**  
**2-2ファイルとディレクトリ**  
・Linuxではファイルのことを**ディレクトリ**という  
・すべてのファイルやディレクトリが収められているディレクトリを**ルートディレクトリ**  
・ユーザーが今いるディレクトリを**カレントディレクトリ**　　
・ファイルやディレクトリの場所はパスを使って表し、ルートディレクトリを起点としてファイルやディレクトリの場所を表す方法を**絶対パス**という  
・カレントディレクトリを起点としてファイルやディレクトリの場所を表す方法を**相対パス**  
・ユーザーがログインした時点のカレントディレクトリを**ホームディレクトリ**  
####CHAPTER3Linuxサーバー構築の基礎知識**  
**3-1サーバーを公開する**  
・サーバー、OS、ドメイン名、IPアドレスを準備する  
・ポート、ネットワーク、Webサーバー、ファイアウォール、ドキュメントルートなどを設定する  
**3-2サーバーを用意する手段**  
・見かけや性能がパソコンに近いサーバーを**PCサーバー**  
・サーバーを用意する手段として自前で用意する方法と、事業者から借りる**レンタルサーバー**がある  
・レンタルサーバーには事業者がサーバーもネットワーク機器もネットワーク回線も用意しそれらを借りる**ホスティング**という形態と、サーバー等のハードウェアは自前で用意し、業者には設置場所とネットワーク回線だけを借りる**ハウジング**という形態がある  
・1台のサーバーをソフトウェア的に分割し、外部からは複数のサーバーであるかのように見せかけるものを**VPS**という  
・VPSを更に進化させ、必要に応じてメモリやディスク容量、CPUパワー、仮想サーバーの台数を増やせられる**クラウドサーバー**がある  
####CHAPTER4Linuxサーバー運用の基礎知識**  
**4-1サーバー運用のポイント**  
・サーバーの運用とは主に、**リソース管理**、**リモート管理**、**バックアップ**、**ソフトウェアのアップデート**  
・サーバーのメモリやハードディスク、CPUの処理能力などを**リソース**といい、それらを管理することを**リソース管理**という  
・データセンターなどに置かれているサーバーをネットワーク経由で管理することを**リモート管理**  
**4-2ソフトウェアの管理**  
・Linuxではソフトウェアは**パッケージ**という単位でインストールする  
・パッケージには大きく分けて**RPM形式**と**Debian形式**がある  
・それぞれの拡張子はRPM形式が**.rpm**、Debian形式が**.deb**  
**4-3現場で役立つLinuxサーバー管理の知識**  
・システムに常駐して利用者や管理者に対して情報を提供するプログラムを**サービス**という  
・サービスの管理には**service**コマンドを使う  
・ユーザーの追加には**useradd**コマンドを使う  
・パスワードの変更には**passwd**コマンドを使う  
・ファイルのダウンロードには**wget**コマンド、sshを使ったファイルの転送には**scp**を使う  
・処理の自動化には**crontab**  
####CHAPTER5セキュリティの基礎知識
**5-1Linuxにとっての危険**  
・正規のユーザーではないのにシステムに侵入する行為を**不正侵入**  
・ネットワーク経由で大量のデータを送りつけるで、本来の利用者が正常なサービスを受けられなくなる攻撃を**DoS攻撃**  
・何千ものコンピューターにウイルスを仕掛けて目標を同時に攻撃することを**DDoS攻撃(分散型サービス妨害攻撃)**  
・ソフトウェアのバージョンは常に最新にしておく  
・侵入したサーバーを土台にし、別の攻撃対象に攻撃を仕掛ける行為を**踏み台**  
**5-2知っておきたいセキュリティ技術**  
・ファイルやディレクトリには**パーミッション(アクセス権限)**を設定する  
・WebブラウザとWebサーバーとの通信を暗号化するために使われているのが**SSL/TLS**  
・SSHを使ってFTP通信をするのを**SFTP/FTPS**  
・システムを行き来するパケットを調べ、通信を許可か否か判断する仕組みを**ファイアウォール**  
・サーバーに侵入されたかどうかをチェックするセキュリティ・ソフトウェアを**IDS**  
・万が一侵入された場合被害をできるだけ抑えるLinuxのセキュリティ機構を**SELinux**  
**5-3サーバーに求められるセキュリティ対策**  
・最新のソフトウェアを使い、適宜アップデート  
・不要なソフトを削除  
・ログインを制限  
・ポートの確認  
####CHAPTER6実践的Linuxサーバー管理とトラブル対応  
**6-1管理者が知っておきたい基本操作**  
・**/etc/passwd**でユーザー情報を確認する  
・**locate**でファイルを検索  
・**man**コマンドに調べたいコマンドを付け加えることでそのコマンドのマニュアルを見ることができる  
**6-2トラブルに対処する**  
・コマンドが実行できない場合、スペルミス、ファイルの有無、アクセス権限の設定等の可能性を考える  
・ネットワークに繋がらない場合、ケーブル類、ネットワーク機能の設定、ファイアウォールの設定等の可能性を考える  
・SSH接続ができない場合、サーバーの起動確認、ポート、ファイアウォール、ネットワークを確認してみる  
**6-3viエディタを使ってみる**  
・**vi**コマンドで起動し、ファイル名を指定するとそのファイルを開く、ファイルが存在しない場合、新規ファイルとして作成される  
・**i**でカーソルの位置から文字の挿入を開始  
・**:w**で保存、**:q**でviエディタの終了  
・**x**でカーソル位置の文字を削除  
・**/**のあとに検索したい文字列を入力することで、開いているエディタ内でマッチする文字列を探すことができる  
・**u**で直前の操作を取り消す  
####付録・Linux基本用語集・INDEX  
サーバー用ディストリビューション、サーバー設定Tips、サーバー管理コマンド逆引き集など  
  
以上  
##感想
コマンドを使った操作やviエディタの使い方から、サーバー構築の基礎知識（用語・導入）が主な内容で、「基礎」というよりは「基本中の基本」という印象があった。
##ツッコミどころ
この本を読むような人はコマンド操作やviエディタの知識はすでにあると思うので、この部分の内容は不要に感じた。
