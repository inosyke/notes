#MediaWiki
##Встановлення wiki cms на влесному сервері
- - -
На даний момент існує купа цікавих рішень:
* WikiJS
* DocuWiki 
* MediaWiki

Але зупинився на MediaWiki, з попередніми виникли труднощі
у знаходженні документації та відео-інструкцій.
![MEdiaWiki](https://upload.wikimedia.org/wikipedia/commons/thumb/5/56/MediaWiki_logo_2018.svg/1200px-MediaWiki_logo_2018.svg.png)

[Гайд MediaWiki](https://www.mediawiki.org/wiki/Manual:Installation_guide/uk)

[Гайд MediaWiki установка на Ubuntu](https://www.mediawiki.org/wiki/Manual:Running_MediaWiki_on_Debian_or_Ubuntu/ru)

```bash
sudo apt-get install apache2 mysql-server php php-mysql libapache2-mod-php php-xml php-mbstring

sudo apt install tasksel -y
sudo tasksel install lamp-server

cd /tmp/
wget https://releases.wikimedia.org/mediawiki/1.34/mediawiki-1.34.2.tar.gz
tar -xvzf /tmp/mediawiki-*.tar.gz
sudo mkdir /var/lib/mediawiki
sudo mv mediawiki-*/* /var/lib/mediawiki

sudo mysql -u root -p
mysql> CREATE USER 'new_mysql_user'@'localhost' IDENTIFIED BY 'THISpasswordSHOULDbeCHANGED';
mysql> quit;

mysql -u root
mysql> CREATE DATABASE my_wiki;
mysql> use my_wiki;

mysql> GRANT ALL ON my_wiki.* TO 'new_mysql_user'@'localhost';
Query OK, 0 rows affected (0.01 sec)
mysql>quit;

/etc/init.d/mysql stop
            или
service mysql stop
sudo mysql -u root
use mysql;
update user set password=PASSWORD("новый_root_пароль") where User='root';
flush privileges;
quit;
/etc/init.d/mysql stop
/etc/init.d/mysql start

sudo ln -s /var/lib/mediawiki /var/www/html/mediawiki
```
Далі заходимо у браузер `ip/mediawiki`

Конфігуруємо сервер та ***LocalSettings.php*** переміщуємо у `/var/www/html/mediawiki`