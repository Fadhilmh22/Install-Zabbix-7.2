Step 1 : Mulai sesi shell baru dengan hak akses root.
   sudo -s

Step 2 : Instal repositori Zabbix
    wget https://repo.zabbix.com/zabbix/7.2/release/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_7.2+ubuntu22.04_all.deb
    dpkg -i zabbix-release_latest_7.2+ubuntu22.04_all.deb
    apt update

Step 3 : Instal server Zabbix, frontend, agen
    apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent

Step 4 : Install mysql server
    sudo apt-get install mysql-server -y

Step 5 : Membuat database awal
    mysql -uroot -p
    ENTER AJA
	create database zabbix character set utf8mb4 collate utf8mb4_bin;
	create user zabbix@localhost identified by 'zabbix';
	grant all privileges on zabbix.* to zabbix@localhost;
	set global log_bin_trust_function_creators = 1;
	quit;

Step 6 : Pada server Zabbix, host mengimpor skema dan data awal. Anda akan diminta memasukkan kata sandi yang baru Anda buat.
    zcat /usr/share/zabbix/sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -uzabbix -p zabbix

Step 7 : Nonaktifkan opsi log_bin_trust_function_creators setelah mengimpor skema basis data.
    mysql -uroot -p
    password yang dibuat
    set global log_bin_trust_function_creators = 0;
    quit;

Step 8: Setup the DB password in the server configuration file
    sudo vi /etc/zabbix/zabbix_server.conf ---- Set the "DBPassword=zabbix"

Step 9: Mulai proses server dan agen Zabbix
    systemctl restart zabbix-server zabbix-agent apache2
    systemctl enable zabbix-server zabbix-agent apache2

Step 10: Akses website dengan IP Public AWS
    IPPublic/zabbix
