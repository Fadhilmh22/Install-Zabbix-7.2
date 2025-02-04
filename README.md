# Panduan Instalasi Zabbix di Ubuntu

## 1️ Step 1: Mulai sesi shell baru dengan hak akses root
sudo -s

## 2️⃣ Step 2: Instal repositori Zabbix
* wget https://repo.zabbix.com/zabbix/7.2/release/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_7.2+ubuntu22.04_all.deb
* dpkg -i zabbix-release_latest_7.2+ubuntu22.04_all.deb
* apt update
    
## 3️⃣ Step 3: Instal server Zabbix, frontend, dan agent
apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent

## 4️⃣ Step 4: Install MySQL Server

sudo apt-get install mysql-server -y

## 5️⃣ Step 5: Membuat database awal

* mysql -uroot -p
* ENTER AJA
* create database zabbix character set utf8mb4 collate utf8mb4_bin;
  create user zabbix@localhost identified by 'zabbix';
	grant all privileges on zabbix.* to zabbix@localhost;
	set global log_bin_trust_function_creators = 1;
	quit;
 
## 6️⃣ Step 6: Impor skema dan data awal

zcat /usr/share/zabbix/sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -uzabbix -p zabbix

## 7️⃣ Step 7: Nonaktifkan opsi log_bin_trust_function_creators

* mysql -uroot -p
* password yang dibuat
* set global log_bin_trust_function_creators = 0;
  quit;
    
## 8️⃣ Step 8: Konfigurasi database pada server Zabbix

sudo vi /etc/zabbix/zabbix_server.conf ---- Set the "DBPassword=zabbix"

## 9️⃣ Step 9: Mulai server dan agen Zabbix

* systemctl restart zabbix-server zabbix-agent apache2
* systemctl enable zabbix-server zabbix-agent apache2

## 🔟 Step 10: Akses website Zabbix

http://IPPublic/zabbix

