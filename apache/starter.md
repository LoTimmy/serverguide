<img src="https://raw.githubusercontent.com/docker-library/docs/master/httpd/logo.png" width="200">

### 安裝 {#installing}

安裝作業系統及`Apache`相關套件。
因本文主要介紹`Apache`如何安裝及設定，作業系統方面就不再詳述。

```
shell> apt-get install libapache2-mod-php5 php5 php5-gd php5-gd php5-mysql php5-curl php-apc php-mdb2 php-mail php-date php-pear apache2
```

```
shell> sudo yum update -y
shell> sudo yum install -y httpd24 php70 mysql56-server php70-mysqlnd
shell> sudo service httpd start
shell> sudo chkconfig httpd on
shell> chkconfig --list httpd

shell> sudo yum-config-manager --enable epel
shell> sudo yum install -y phpMyAdmin
```


```
shell> cp /etc/apache2/sites-available/default /etc/apache2/sites-available/mynewsite
shell> a2ensite mynewsite
shell> service apache2 restart
shell> a2dissite mynewsite
shell> service apache2 restart
```

```
shell> vim /etc/apache2/conf.d/security
```

```apacheconf
ServerSignature Off
ServerTokens Prod
```

```
shell> a2enmod ssl
shell> a2ensite default-ssl
shell> service apache2 restart
```

```
shell> chgrp -R webmasters /var/www
shell> find /var/www -type d -exec chmod g=rwxs "{}" \;
shell> find /var/www -type f -exec chmod g=rws  "{}" \;
```

```
shell> httpd -M
shell> apachectl -M

Loaded Modules:
 core_module (static)
 so_module (static)
 http_module (static)
 access_compat_module (shared)
 actions_module (shared)
 alias_module (shared)
 allowmethods_module (shared)
 ...
 proxy_fcgi_module (shared)
 proxy_fdpass_module (shared)
 proxy_ftp_module (shared)
 proxy_http_module (shared)
 proxy_scgi_module (shared)
 proxy_wstunnel_module (shared)
 systemd_module (shared)
 cgi_module (shared)
 php5_module (shared)
```

### Disabling SSLv3 on Apache
```
shell> vim /etc/apache2/mods-available/ssl.conf
```
```apacheconf
SSLProtocol all -SSLv3 -SSLv2
```

### Enabling Strict Transport Security

```apacheconf
LoadModule headers_module libexec/apache2/mod_headers.so

<VirtualHost 100.101.102.103:443>
  Header always set Strict-Transport-Security "max-age=31536000; includeSubDomains; preload";
</VirtualHost>

<VirtualHost *:80>
  # ...
  ServerName example.com
  Redirect permanent / https://example.com/
</VirtualHost>

<VirtualHost *:80>
  # ...
  <IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteCond %{HTTPS} off
    RewriteRule (.*) https://%{HTTP_HOST}%{REQUEST_URI}
  </IfModule>
</VirtualHost>
```

### mod_spdy {#mod_spdy}

`SPDY` (讀音為`SPeeDY`) 為一網路內容傳輸的應用程式層協定，該協定著重於開發更多可降低延遲的功能，諸如多工串流、判斷要求的優先等級，以及HTTP標頭的壓縮等。
`SPDY`是`Google`針對HTTP傳輸協定缺點所提出的改良版，具有加密、傳輸較快等優點。

```
shell> uname -m

shell> wget https://dl-ssl.google.com/dl/linux/direct/mod-spdy-beta_current_i386.deb
shell> wget https://dl-ssl.google.com/dl/linux/direct/mod-spdy-beta_current_amd64.deb

shell> sudo dpkg -i mod-spdy-*.deb
shell> sudo apt-get -f install

shell> vim /etc/apache2/mods-available/spdy.conf
``` 

```apacheconf
Turning ON mod_spdy
SpdyEnabled on
```

#### :books: 參考網站：
- [mod_spdy](https://developers.google.com/speed/spdy/mod_spdy/)

### mod_pagespeed {#mod_pagespeed}

`mod_pagespeed`可以自動為`Apache`網站上的網頁及資源使用做最佳化，以加快網站的速度。

```
shell> uname -m

shell> wget https://dl-ssl.google.com/dl/linux/direct/mod-pagespeed-stable_current_i386.deb
shell> wget https://dl-ssl.google.com/dl/linux/direct/mod-pagespeed-stable_current_amd64.deb

shell> sudo dpkg -i mod-pagespeed-*.deb
shell> sudo apt-get -f install

shell> sudo apt-get update
shell> sudo apt-get upgrade
shell> sudo /etc/init.d/apache2 restart
```

`/etc/apache2/mods-available/pagespeed.conf`


#### :books: 參考網站：
- [pagespeed](https://developers.google.com/speed/pagespeed/module/)
- https://modpagespeed.com/doc/download
- https://modpagespeed.com/doc/configuration
- https://www.modpagespeed.com/
- https://modpagespeed.com/doc/



