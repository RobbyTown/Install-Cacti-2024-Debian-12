# Install-Cacti-2024-Debian-12
Install Cacti 2024 Debian 12

sudo apt update && apt upgrade -y

sudo timedatectl set-timezone Asia/Jakarta

date

sudo apt install snmp php-snmp rrdtool librrds-perl unzip curl git gnupg2 -y

sudo apt install apache2 mariadb-server php php-mysql libapache2-mod-php php-xml php-ldap php-mbstring php-gd php-gmp php-intl -y


sudo nano /etc/php/8.2/apache2/php.ini

memory_limit = 1024M
max_execution_time = 60
date.timezone = Asia/Jakarta


sudo nano /etc/php/8.2/cli/php.ini

memory_limit = 1024M
max_execution_time = 60
date.timezone = Asia/Jakarta


sudo systemctl restart apache2

sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf


collation-server = utf8mb4_unicode_ci
max_heap_table_size = 128M
tmp_table_size = 64M
join_buffer_size = 1M
innodb_file_format = Barracuda
innodb_large_prefix = 1
innodb_buffer_pool_size = 512M
innodb_flush_log_at_timeout = 3
innodb_read_io_threads = 32
innodb_write_io_threads = 16
innodb_io_capacity = 5000
innodb_io_capacity_max = 10000
innodb_doublewrite = OFF
sort_buffer_size = 1M

sudo systemctl restart mariadb

systemctl status mariadb

mysql

create database cactidb;
GRANT ALL ON cactidb.* TO cactiuser@localhost IDENTIFIED BY 'passwordkamu';
flush privileges;
exit;

mysql mysql < /usr/share/mysql/mysql_test_data_timezone.sql

mysql
MariaDB [(none)]> GRANT SELECT ON mysql.time_zone_name TO cactiuser@localhost;
MariaDB [(none)]> flush privileges;
MariaDB [(none)]> exit;

rm -rf /var/www/html/index.html
wget https://www.cacti.net/downloads/cacti-latest.tar.gz --no-check-certificate

mv cacti-1* /var/www/html/cacti

nano /var/www/html/include/config.php

$database_default  = 'cactidb';
$database_hostname = 'localhost';
$database_username = 'passwordkamu';
$database_password = 'passwordkamu';

chown -R www-data:www-data /var/www/html/*
