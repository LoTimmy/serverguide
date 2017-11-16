### 在 `CentOS-5` 上建置 `Apache`

```
shell> yum install httpd

shell> chkconfig --list
shell> chkconfig httpd on

shell> service httpd start
```

```
shell> sestatus
```
```
SELinux status:                 enabled
SELinuxfs mount:                /selinux
Current mode:                   enforcing
Mode from config file:          enforcing
Policy version:                 24
Policy from config file:        targeted
```
```
shell> vim /etc/selinux/config
```
```
# This file controls the state of SELinux on the system.
# SELINUX= can take one of these three values:
#       enforcing - SELinux security policy is enforced.
#       permissive - SELinux prints warnings instead of enforcing.
#       disabled - No SELinux policy is loaded.
SELINUX=disabled
# SELINUXTYPE= can take one of these two values:
#       targeted - Targeted processes are protected,
#       mls - Multi Level Security protection.
SELINUXTYPE=targeted
```

```
shell> getenforce
Disabled
```
---

`mod_pagespeed`可以自動為`Apache`網站上的網頁及資源使用做最佳化，以加快網站的速度。

---

` `是一種網頁防火牆軟體，可與網站伺服器結合（依照官方網站的說明，`ModSecurity`可安裝在`Apache`或`IIS`等知名的網站伺服器），提供針對Web攻擊如`資料庫注入攻擊` (`SQL injection Attacks`)、`跨網站Script攻擊` (`Cross-site Scripting`)、`目錄路徑外洩` (`Path Traversal Attacks`)等等相關攻擊的防衛能力。

```
libapache2-mod-security2 - Tighten web applications security for Apache
libapache2-modsecurity - Dummy transitional package
modsecurity-crs - modsecurity's Core Rule Set
```

```
shell> apt-get install libapache2-mod-security2
shell> dpkg -L libapache2-mod-security2
shell> /etc/modsecurity/modsecurity.conf
```

**413 Payload Too Large**

```apacheconf
SecRequestBodyAccess On
```

```
shell> a2dismod security2
shell> service apache2 restart
```

```
spring.http.multipart.max-file-size=128KB
spring.http.multipart.max-request-size=128KB
```

#### :books: 參考網站：
- [uploading-files](https://spring.io/guides/gs/uploading-files/)
- [413 Payload Too Large](https://tools.ietf.org/html/rfc7231#section-6.5.11)

---

```
shell> apt-get install libapache2-mod-rpaf
shell> vim /etc/apache2/mods-available/rpaf.conf
```
```apacheconf
RPAFproxy_ips 192.168.42.102
```

---

`Apache`在全球網站伺服器的領導地位早已著毋庸議，而其內建的`Apache Bench`也是相當值得參考的效能指標。
進行網站伺服器的效能測試，同步連線數設為10，對伺服器發出10000次request

```
http://192.168.42.104
shell> ab -n 10000 -c 10 http://192.168.42.104/
shell> ab -c 500 -n 20000 http://192.168.42.104/
shell> ab -c 500 -n 20000 -H "Accept-Encoding: gzip" http://192.168.42.104/
```
---

```
shell> apache2ctl -M

Loaded Modules:
 core_module (static)
 log_config_module (static)
 logio_module (static)
 version_module (static)
 mpm_worker_module (static)
 http_module (static)
 so_module (static)
 alias_module (shared)
 auth_basic_module (shared)
 authn_file_module (shared)
 authz_default_module (shared)
 authz_groupfile_module (shared)
 authz_host_module (shared)
 authz_user_module (shared)
 autoindex_module (shared)
 cgid_module (shared)
 deflate_module (shared)
 dir_module (shared)
 env_module (shared)
 mime_module (shared)
 security2_module (shared)
 negotiation_module (shared)
 pagespeed_module (shared)
 proxy_module (shared)
 proxy_balancer_module (shared)
 proxy_http_module (shared)
 reqtimeout_module (shared)
 setenvif_module (shared)
 spdy_module (shared)
 status_module (shared)
 unique_id_module (shared)
Syntax OK
```

---

```
shell> apt-get install liblwp-online-perl
shell> HEAD http://127.0.0.1/
shell> GET -Used http://127.0.0.1/
```

---

`htpasswd - Manage user files for basic authentication`

```
shell> htpasswd -c /home/doe/public_html/.htpasswd jane
shell> htpasswd /usr/local/etc/apache/.htpasswd-users jsmith
shell> htpasswd -db /usr/web/.htpasswd-all jones Pwd4Steve
```

---
##### 疑難排解 (Troubleshooting)

出現錯誤訊息

```
apache2: Could not reliably determine the server's fully qualified domain name, using 192.168.42.102 for ServerName
```

```
shell> vim /etc/hosts
```
```
192.168.42.102       ip-192-168-42-102.myserver.com ip-192-168-42-102
```

---

```apacheconf
# Ensure that Apache listens on port 80
Listen 80

# Listen for virtual host requests on all IP addresses
NameVirtualHost *:80

# etc ...
# ...
```

```
Complete!
```

#### :books: 參考網站：

- [letsencrypt](https://github.com/letsencrypt/letsencrypt)
- [httpd.conf](http://www.opensource.apple.com/source/apache/apache-769/httpd.conf)
- [Apache Core Features](http://httpd.apache.org/docs/2.2/mod/core.html)
- [开源 Apache 服务器安全防护技术精要及实战](http://www.ibm.com/developerworks/cn/linux/l-cn-apache-secure/index.html)
- [HTTPD - Apache2 Web Server](https://help.ubuntu.com/12.04/serverguide/httpd.html)
- [Certificates](https://help.ubuntu.com/12.04/serverguide/certificates-and-security.html)
- [Enabling and Disabling SELinux](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Security-Enhanced_Linux/sect-Security-Enhanced_Linux-Working_with_SELinux-Enabling_and_Disabling_SELinux.html)
- [http://zh.wikipedia.org/wiki/SPDY](http://zh.wikipedia.org/wiki/SPDY)
- [Apache釋出HTTP伺服器2.4版](http://www.ithome.com.tw/node/72287)
- [IETF規劃下一代HTTP標準　將採SPDY協定](http://www.ithome.com.tw/node/76619)
- [Google揭露SPDY計畫　要讓網路瀏覽速度加倍](http://www.ithome.com.tw/node/58126)
- [Google釋出正式版Apache自動優化模組　加快網站速度](http://www.ithome.com.tw/node/76796)
- [How to Install Apache, MySQL, and PHP on Ubuntu](https://www.vultr.com/docs/how-to-install-apache-mysql-and-php-on-ubuntu)
- [Disabling SSLv3](https://www.vultr.com/docs/disabling-sslv3)
- [以ModSecurity監控上傳檔案 結合開源ClamAV防毒](http://www.netadmin.com.tw/article_content.aspx?sn=1412020006)
- [spdy](https://developers.google.com/speed/spdy/)
- [spdy](https://developers.google.com/speed/spdy/mod_spdy/)
- [Apache內建基本驗證機制 快速啟用mod_auth網頁認證](http://www.netadmin.com.tw/article_content.aspx?sn=1107080004&ns=1107190008)
- [用MySQL資料庫 提供Apache控管帳號權限](http://www.netadmin.com.tw/article_content.aspx?sn=1107150002)
- [ab - Apache HTTP server benchmarking tool](http://httpd.apache.org/docs/2.2/programs/ab.html)
- [htpasswd](http://httpd.apache.org/docs/2.2/programs/htpasswd.html)
