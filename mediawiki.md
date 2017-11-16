
```
shell> apt-get install apache2 mysql-server php php-mysql libapache2-mod-php php-xml php-mbstring
shell> apt-get install php-apcu php-intl imagemagick inkscape php-gd php-cli php-curl
shell> apt-get install git
shell> apt-get install curl

shell> wget https://releases.wikimedia.org/mediawiki/1.29/mediawiki-1.29.1.tar.gz
shell> tar -zxvf mediawiki-*.tar.gz
shell> mkdir /var/www/html/mediawiki
shell> mv mediawiki-*/* /var/www/html/mediawiki
```
`/etc/php/7.0/apache2/php.ini`
```
upload_max_filesize = 2M
memory_limit = 128M
```

```
shell> phpenmod mbstring
shell> phpenmod xml
shell> systemctl restart apache2.service
```

`http://172.16.141.129/mediawiki`

`LocalSettings.php`
```
```

```
shell> mysqladmin -u root -p drop my_wiki
shell> mysqladmin -u root -p create my_wiki
shell> mysqldump -h -u root -p my_wiki > my_wiki.sql
shell> mysqldump --default-character-set=binary -h -u root -p my_wiki > my_wiki.sql
shell> mysql -u root -p --binary-mode my_wiki < my_wiki.sql

shell> cd /var/www/html/mediawiki
shell> php maintenance/update.php
```

```
shell> wget https://extdist.wmflabs.org/dist/extensions/VisualEditor-REL1_29-b655946.tar.gz
shell> tar -zxvf VisualEditor-REL1_29-b655946.tar.gz -C /var/www/html/mediawiki/extensions

shell> wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.6/install.sh | bash
shell> nvm install --lts=boron
shell> nvm alias default lts/boron
shell> nvm use --lts=boron

shell> cd /opt
shell> git clone https://github.com/wikimedia/parsoid.git
shell> cd /opt/parsoid
shell> npm install
shell> cp config.example.yaml config.yaml
shell> npm start
```

```
shell> sudo apt-key adv --keyserver keys.gnupg.net --recv-keys 90E9F83F22250DD7
shell> sudo apt install software-properties-common
shell> sudo add-apt-repository "deb https://releases.wikimedia.org/debian jessie-mediawiki main"
shell> apt-get update
shell> apt-get install apt-transport-https
shell> apt-get install parsoid
shell> systemctl restart parsoid.service
```
`LocalSettings.php`
```
wfLoadExtension( 'VisualEditor' );
$wgDefaultUserOptions['visualeditor-enable'] = 1;
```

---

```
shell> cd /var/www/html/mediawiki/maintenance
shell> php changePassword.php --user= --password= 
```

```
shell> apt-get update && apt-get install -y supervisor
parsoid.conf
supervisorctl reload
supervisorctl status
supervisorctl start
```

```
[supervisord]
nodaemon=true

[program:parsoid]
directory=/opt/parsoid
command=/root/.nvm/versions/node/v6.11.5/bin/node /opt/parsoid/bin/server.js
```

- https://releases.wikimedia.org/mediawiki/
- https://github.com/wikimedia/VisualEditor
- https://github.com/wikimedia/parsoid
