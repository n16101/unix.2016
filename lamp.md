## サーバ(LAMP)環境構築

1. システムの更新  
$ sudo apt update  
$ sudo apt upgrade

1. Apache2インストール&設定  
	1. Apache2インストール  
	$ sudo aptitude install apache2  
	1. DocumentRootの所有者をログインユーザに変更  
	$ sudo chown ユーザ名.グループ名 /var/www/html  

1. MySQLインストール&設定  
	1. MySQLインストール(インストール途中でデータベース管理者(root)のパスワード設定)  
	$ sudo aptitude install mysql-server  
	1. MySQLサーバへの接続確認(切断は「exit」)  
	$ mysql -u root -p  
	mysql> exit  
	1. 設定ファイル(my.cnf)の編集  
		1. ディレクトリの移動  
		$ cd /etc/mysql  
		1. my.cnfのバックアップ  
		$ sudo cp my.cnf my.cnf.bak  
		1. エディタで開いて編集  
		$ sudo vi my.cnf  
		::: ここから :::  
		[mysqld]  
		character_set_server = utf8  
		skip-character-set-client-handshake  
		default-storage-engine = innoDB  
		innodb_file_per_table  
		[client]  
		default-character-set = utf8  
		[mysqldump]  
		default-character-set = utf8  
		::: ここまで :::  
		1. 設定ファイルの再読み込み  
		$ sudo service mysql restart
		
1. PHPイストール&設定
	1. phpパッケージのインストール  
	$ sudo aptitude install php5  
	1. MySQLとの連携のためのパッケージのインストール  
	$ sudo aptitude install php5-mysql  
	1. その他のパッケージのインストール  
	$ sudo aptitude install php-pear php5-gd  
	1. Apacheの再起動(phpを読み込むため)  
	$ sudo service apache2 restart  
	1. 動作確認  
		1. エディタでphpファイルを作成  
		$ vi /var/www/html/test.php  
		::: 以下を追加 :::  
		<?php  
			phpinfo(); // phpの設定情報表示  
		?>  
		::: 以上を追加　:::  
		1. ブラウザで以下にアクセスし、phpの設定情報が表示されること  
		但し、以下のIPアドレスをプロキシーから除外する必要あり  
		http://localhost:8080/test.php  
		http://192.168.56.10/test.php  
	1. 設定ファイル(php.ini)の編集
		1. ディレクトリの移動  
		$ cd /etc/php5/apache2  
		1. php.iniのバックアップ  
		$ sudo cp php.ini php.ini.bak  
		1. エディタで開いて編集  
		$ sudo vi php.ini  
		::: ここから :::  
		display_errors = On  
		error_log = /var/log/php.log  
		mbstring.language = Japanese  
		mbstring.internal_encoding = UTF-8  
		mbstring.http_input = auto  
		mbstring.detect_order = auto  
		expose_php = Off  
		date.timezone = Asia/Tokyo  
		::: ここまで :::  
		1. Apacheの再起動(設定ファイルの再読み込みのため)  
		$ sudo service apache2 restart  
