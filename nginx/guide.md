![](https://raw.githubusercontent.com/docker-library/docs/master/nginx/logo.png)

![](https://d33np9n32j53g7.cloudfront.net/assets/stacks/nginxstack/img/nginxstack-stack-110x117-5c92eb04fa128dcd0e9259e63b1dba01.png)

### 在 `Raspbian` 8.0 上建置 `nginx`

安裝作業系統及`nginx`相關套件。
因本文主要介紹`nginx`如何安裝及設定，作業系統方面就不再詳述。

``` 
shell> lsb_release -a
No LSB modules are available.
Distributor ID:	Raspbian
Description:	Raspbian GNU/Linux 8.0 (jessie)
Release:	8.0
Codename:	jessie
```

### 安裝 {#installing}

```
nginx - small, powerful, scalable web/proxy server
nginx-extras - nginx web/proxy server (extended version)
nginx-full - nginx web/proxy server (standard version)
nginx-light - nginx web/proxy server (basic version)
```

```
shell> curl -sSL http://nginx.org/keys/nginx_signing.key | sudo apt-key add - 
shell> echo deb http://nginx.org/packages/ubuntu/ "$(lsb_release -sc)" nginx | sudo tee /etc/apt/sources.list.d/nginx.list
shell> echo deb-src http://nginx.org/packages/ubuntu/ "$(lsb_release -sc)" nginx | sudo tee -a /etc/apt/sources.list.d/nginx.list
shell> sudo apt-get update
shell> sudo apt-get install nginx
```

```
shell> nginx -v
nginx version: nginx/1.6.2
nginx version: nginx/1.10.2
nginx version: nginx/1.12.0
```

```
shell> nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

```
shell> nginx -V
nginx version: nginx/1.6.2
TLS SNI support enabled
configure arguments: --with-cc-opt='-g -O2 -fstack-protector-strong -Wformat -Werror=format-security -D_FORTIFY_SOURCE=2' --with-ld-opt=-Wl,-z,relro --prefix=/usr/share/nginx --conf-path=/etc/nginx/nginx.conf --http-log-path=/var/log/nginx/access.log --error-log-path=/var/log/nginx/error.log --lock-path=/var/lock/nginx.lock --pid-path=/run/nginx.pid --http-client-body-temp-path=/var/lib/nginx/body --http-fastcgi-temp-path=/var/lib/nginx/fastcgi --http-proxy-temp-path=/var/lib/nginx/proxy --http-scgi-temp-path=/var/lib/nginx/scgi --http-uwsgi-temp-path=/var/lib/nginx/uwsgi --with-debug --with-pcre-jit --with-ipv6 --with-http_ssl_module --with-http_stub_status_module --with-http_realip_module --with-http_auth_request_module --with-http_addition_module --with-http_dav_module --with-http_geoip_module --with-http_gzip_static_module --with-http_image_filter_module --with-http_spdy_module --with-http_sub_module --with-http_xslt_module --with-mail --with-mail_ssl_module --add-module=/build/nginx-Kaumns/nginx-1.6.2/debian/modules/nginx-auth-pam --add-module=/build/nginx-Kaumns/nginx-1.6.2/debian/modules/nginx-dav-ext-module --add-module=/build/nginx-Kaumns/nginx-1.6.2/debian/modules/nginx-echo --add-module=/build/nginx-Kaumns/nginx-1.6.2/debian/modules/nginx-upstream-fair --add-module=/build/nginx-Kaumns/nginx-1.6.2/debian/modules/ngx_http_substitutions_filter_module

nginx version: nginx/1.12.0
built by gcc 5.4.0 20160609 (Ubuntu 5.4.0-6ubuntu1~16.04.2) 
built with OpenSSL 1.0.2g-fips  1 Mar 2016 (running with OpenSSL 1.0.2g  1 Mar 2016)
TLS SNI support enabled
configure arguments: --prefix=/etc/nginx --sbin-path=/usr/sbin/nginx --modules-path=/usr/lib/nginx/modules --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --pid-path=/var/run/nginx.pid --lock-path=/var/run/nginx.lock --http-client-body-temp-path=/var/cache/nginx/client_temp --http-proxy-temp-path=/var/cache/nginx/proxy_temp --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp --http-scgi-temp-path=/var/cache/nginx/scgi_temp --user=nginx --group=nginx --with-compat --with-file-aio --with-threads --with-http_addition_module --with-http_auth_request_module --with-http_dav_module --with-http_flv_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_mp4_module --with-http_random_index_module --with-http_realip_module --with-http_secure_link_module --with-http_slice_module --with-http_ssl_module --with-http_stub_status_module --with-http_sub_module --with-http_v2_module --with-mail --with-mail_ssl_module --with-stream --with-stream_realip_module --with-stream_ssl_module --with-stream_ssl_preread_module --with-cc-opt='-g -O2 -fstack-protector-strong -Wformat -Werror=format-security -Wp,-D_FORTIFY_SOURCE=2 -fPIC' --with-ld-opt='-Wl,-Bsymbolic-functions -Wl,-z,relro -Wl,-z,now -Wl,--as-needed -pie'
```

```
shell> hostname -I
```

```
shell> service nginx restart
```
```
shell> systemctl status nginx.service
```
---

**反向 Proxy**
![](http://i.imgur.com/9QNvICY.gif)
![](https://upload.wikimedia.org/wikipedia/commons/6/67/Reverse_proxy_h2g2bob.svg)

```
shell> cd /etc/nginx
shell> sudo vim sites-enabled/default
shell> sudo vim conf.d/default.conf 
```

```
upstream backend {
  server backend1.example.com:12345;
  server backend2.example.com:12345;
  server backend3.example.com:12345;
  keepalive 32;
}

server {
  listen 80 default_server;

  location / {
    add_header Alternate-Protocol: 443:npn-spdy/2;
    proxy_pass http://backend;
    proxy_buffering off;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
  }
}
```

#### :books: 參考網站：
- [reverse-proxy](https://www.nginx.com/resources/admin-guide/reverse-proxy/)
- [load-balancer](https://www.nginx.com/resources/admin-guide/load-balancer/)
- [Alternate-Protocol](https://www.chromium.org/spdy/spdy-protocol/spdy-protocol-draft2#TOC-Server-Advertisement-of-SPDY-through-the-HTTP-Alternate-Protocol-header)

---

###### 建立憑證要求 (Generates a CSR)

```
shell> openssl req -newkey rsa:2048 -sha256 -nodes -keyout www_yourdomain_com.key -out server-req.pem -subj "/C=TW/ST=Taiwan/L=TPE/O=MyCompany Ltd/OU=IT/CN=server"

shell> openssl pkcs12 -export -in server.crt -inkey server.key -out server.pfx
shell> openssl pkcs12 -export -in server.crt -inkey server.key -out server.pfx -certfile CACert.crt
```

#### :books: 參考網站：
- [easy-csr](https://www.digicert.com/easy-csr/openssl.htm)
- [Converting a PFX file to PEM, SPC, and PVK files](https://support.comodo.com/index.php?/Default/Knowledgebase/Article/View/548/7/converting-a-pfx-file-to-pem-spc-and-pvk-files)
- [How do I export, as a pfx file, my Certificate and Private Key from Apache?](https://support.comodo.com/index.php?/Default/Knowledgebase/Article/View/297/17/how-do-i-export-as-a-pfx-file-my-certificate-and-private-key-from-apache)
- [Convert a Certificate File to PKCS#12 Format](https://pubs.vmware.com/view-51/index.jsp?topic=%2Fcom.vmware.view.certificates.doc%2FGUID-17AD1631-E6D6-4853-8D9B-8E481BE2CC68.html)
- [Various Types of SSL Commands and Keytool](https://cheapsslsecurity.com/blog/various-types-ssl-commands-keytool/)

######  提交憑證要求

`server-req.pem`
`www_yourdomain_com.key`

###### 安裝憑證，並設定 SSL 的網站

```
shell> cat www_yourdomain_com.crt COMODORSADomainValidationSecureServerCA.crt COMODORSAAddTrustCA.crt > ssl-bundle.crt

shell> cat COMODORSADomainValidationSecureServerCA.crt COMODORSAAddTrustCA.crt AddTrustExternalCARoot.crt > full_chain.pem  

shell> cp ssl-bundle.crt /etc/ssl/certs
shell> cp www_yourdomain_com.key /etc/ssl/private/mysite.key
shell> cp full_chain.pem /etc/nginx/ssl
```

```
shell> openssl dhparam -out dhparam.pem 2048
```

<img src="http://i.imgur.com/50BIcHD.png" width="300">

```
shell> cd /etc/nginx
shell> sudo vim sites-enabled/default 
```

```
server {
    listen       80;
    server_name  _;
    return       301 https://$host$request_uri;
}

server {
	listen 443 ssl default_server spdy;
    listen 443 ssl http2;

    server_tokens off;


        ssl on;
        ssl_certificate /etc/ssl/certs/ssl-bundle.crt;
        ssl_certificate_key /etc/ssl/private/mysite.key;
         
        ssl_dhparam /etc/ssl/private/dhparam.pem;

        #enables all versions of TLS, but not SSLv2 or 3 which are weak and now deprecated.
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;

        #Disables all weak ciphers
        ssl_ciphers "ECDHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA:ECDHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA256:DHE-RSA-AES128-SHA256:DHE-RSA
-AES256-SHA:DHE-RSA-AES128-SHA:ECDHE-RSA-DES-CBC3-SHA:EDH-RSA-DES-CBC3-SHA:AES256-GCM-SHA384:AES128-GCM-SHA256:AES256-SHA256:AES128-SHA256:AES256-SHA:AES128-SHA:DES-CBC3-SHA:HIGH:!aNULL:!eNULL:!EXPORT:!DES:!MD5:!PSK:!RC4";

        ssl_prefer_server_ciphers on;

        resolver 8.8.8.8;
        ssl_stapling on;
        ssl_stapling_verify on;
        ssl_trusted_certificate /etc/nginx/ssl/full_chain.pem;
        
        #Enabling Strict Transport Security
        add_header Strict-Transport-Security "max-age=31536000; includeSubDomains; preload";
        add_header X-Frame-Options SAMEORIGIN;
        add_header Alternate-Protocol: 443:npn-spdy/3;

}
```

```
shell> openssl s_client -connect www.godaddy.com:443
```

#### :books: 參考網站：
- [enable-ocsp-stapling-on-nginx](https://support.comodo.com/index.php?/Default/Knowledgebase/Article/View/1015/38/enable-ocsp-stapling-on-nginx)
- [ngx_http_v2_module](http://nginx.org/en/docs/http/ngx_http_v2_module.html)
- [enable-ocsp-stapling-on-nginx](https://support.comodo.com/index.php?/Default/Knowledgebase/Article/View/1015/0/enable-ocsp-stapling-on-nginx)
- [X-Frame-Options](https://developer.mozilla.org/en-US/docs/Web/HTTP/X-Frame-Options)
- [linux_packages](http://nginx.org/en/linux_packages.html)
- [ngx_http_headers_module](http://nginx.org/en/docs/http/ngx_http_headers_module.html)
- [csr-generation-using-openssl-apache-wmod_ssl-nginx-os-x](https://support.comodo.com/index.php?/Default/Knowledgebase/Article/View/1/0/csr-generation-using-openssl-apache-wmod_ssl-nginx-os-x)
- [certificate-installation-nginx](https://support.comodo.com/index.php?/Default/Knowledgebase/Article/View/789/37/certificate-installation-nginx)
- [HTTP_strict_transport_security](https://developer.mozilla.org/en-US/docs/Web/Security/HTTP_strict_transport_security)
- [最佳化效能](https://developers.google.com/web/fundamentals/performance/?hl=zh-tw)
- [configuring_https_servers](http://nginx.org/en/docs/http/configuring_https_servers.html)
- [sslanalyzer](https://sslanalyzer.comodoca.com/)
- [root-comodo-rsa-certification-authority-sha-2](https://support.comodo.com/index.php?/Knowledgebase/Article/View/969/0/root-comodo-rsa-certification-authority-sha-2)
- [intermediate-2-sha-2-comodo-rsa-domain-validation-secure-server-ca](https://support.comodo.com/index.php?/Knowledgebase/Article/View/970/0/intermediate-2-sha-2-comodo-rsa-domain-validation-secure-server-ca)


---

```
location / {
    deny  192.168.1.1;
    allow 192.168.1.0/24;
    allow 10.1.1.0/16;
    allow 2001:0db8::/32;
    deny  all;
}
```

#### :books: 參考網站：
- [ngx_http_access_module](http://nginx.org/en/docs/http/ngx_http_access_module.html)

---
```
server_tokens off;
```

```
HTTP/1.1 200 OK
Server: nginx/1.10.2
Date: Wed, 09 Nov 2016 04:11:31 GMT
Content-Type: text/html
Content-Length: 612
Last-Modified: Tue, 18 Oct 2016 15:03:13 GMT
Connection: keep-alive
ETag: "580639b1-264"
Accept-Ranges: bytes
```

```
HTTP/1.1 200 OK
Server: nginx
Date: Wed, 09 Nov 2016 04:13:01 GMT
Content-Type: text/html
Content-Length: 612
Last-Modified: Tue, 18 Oct 2016 15:03:13 GMT
Connection: keep-alive
ETag: "580639b1-264"
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
X-Frame-Options: SAMEORIGIN
Alternate-Protocol:: 443:npn-spdy/3
Accept-Ranges: bytes
```

---

```
location /favicon.ico {
  log_not_found off;
  access_log off;
}
```
#### :books: 參考網站：
- [log_not_found](http://nginx.org/en/docs/http/ngx_http_core_module.html#log_not_found)

---
```
shell> nginx -V
```

```
--with-http_spdy_module
```

```
listen 443 ssl default_server spdy;
```

#### :books: 參考網站：
- [ngx_http_spdy_module](http://nginx.org/en/docs/http/ngx_http_spdy_module.html)

---
### 安裝 PHP

```
shell> sudo apt-get install php5-fpm
```

```
shell> cd /etc/nginx
shell> sudo vim sites-enabled/default 
```

```
index index.html index.htm;
```

```
index index.php index.html index.htm;
```

---

**/etc/nginx/sites-enabled/default**
```
server {
        listen 80 default_server;
        listen [::]:80 default_server;
        root /var/www/html;
        index index.html index.htm index.nginx-debian.html;
        server_name _;
}
```

---

**/etc/nginx/sites-enabled/default**
```
location / {
       # First attempt to serve request as file, then
       # as directory, then fall back to displaying a 404.
       try_files $uri $uri/ =404;
}
```
---

**/etc/nginx/nginx.conf**

```
worker_processes auto;

http {
        server_tokens off;
        ssl_session_cache   shared:SSL:10m;
        ssl_session_timeout 10m;
        
        ssl_stapling on;
        resolver 192.0.2.1;
        ssl_trusted_certificate file;
}

events {
        worker_connections 768;
        use epoll; 
        multi_accept on;
}

http {
        gzip on;
        gzip_disable "msie6";
        gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;


}

- [configuring_https_servers](http://nginx.org/en/docs/http/configuring_https_servers.html)
- [ngx_http_ssl_module](http://nginx.org/en/docs/http/ngx_http_ssl_module.html)

```

#### :books: 參考網站：
- [worker_processes](http://nginx.org/en/docs/ngx_core_module.html#worker_processes)
- [use](http://nginx.org/en/docs/ngx_core_module.html#use)
- [ngx_http_gzip_module](http://nginx.org/en/docs/http/ngx_http_gzip_module.html)

---

```
net.ipv4.ip_local_port_range = 1024	65000
net.ipv4.tcp_fin_timeout = 15
```

#### :books: 參考網站：
-  [Changing Network Kernel Settings](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/5/html/Tuning_and_Optimizing_Red_Hat_Enterprise_Linux_for_Oracle_9i_and_10g_Databases/sect-Oracle_9i_and_10g_Tuning_Guide-Adjusting_Network_Settings-Changing_Network_Kernel_Settings.html)

---

```
shell> sudo apt-get install nginx-extras
```

```
# set the Server output header
more_set_headers    "Server: my_server";

more_clear_headers 'Server';
more_clear_headers 'X-Powered-By';

expires    24h;
add_header Cache-Control private;

```

#### :books: 參考網站：
- [headers-more-nginx-module](https://github.com/openresty/headers-more-nginx-module#more_set_headers)
- [ngx_http_headers_module](http://nginx.org/en/docs/http/ngx_http_headers_module.html#add_header)

---

```
server {
        client_max_body_size 100m;
}
```

#### :books: 參考網站：
- [client_max_body_size](http://nginx.org/en/docs/http/ngx_http_core_module.html#client_max_body_size)

---

```
server {
    listen       80;
    server_name www.example.com;
    rewrite ^ https://$server_name$request_uri? permanent;  
}
```

`$server_name`

`$request_uri`

`permanent`

#### :books: 參考網站：
- [ngx_http_rewrite_module](http://nginx.org/en/docs/http/ngx_http_rewrite_module.html)

---

How to test Logjam via command line?

```
shell> openssl s_client -connect www.example.com:443 -cipher 'EXP'
shell> nmap --script ssl-enum-ciphers -p 443 www.example.com
```

---

```
if ($http_user_agent ~* "Baiduspider") {
        return 403;
}
```
**robots.txt**
```
User-agent: Googlebot-Image
User-agent: Googlebot
User-agent: Baiduspider
Disallow: /
```
#### :books: 參考網站：
- [建立 robots.txt 檔案](https://support.google.com/webmasters/answer/6062596?hl=zh-Hant)
- [robots.txt](https://support.google.com/webmasters/answer/6062608?hl=zh-Hant)
- [透過 robots.txt 測試工具來測試 robots.txt](https://support.google.com/webmasters/answer/6062598)

---

```
nginx - high performance web server
nginx-dbg - nginx debug symbols
nginx-module-geoip - nginx GeoIP dynamic modules
nginx-module-geoip-dbg - debug symbols for the nginx-module-geoip
nginx-module-image-filter - nginx image filter dynamic module
nginx-module-image-filter-dbg - debug symbols for the nginx-module-image-filter
nginx-module-njs - nginx nginScript dynamic modules
nginx-module-njs-dbg - debug symbols for the nginx-module-njs
nginx-module-perl - nginx Perl dynamic module
nginx-module-perl-dbg - debug symbols for the nginx-module-perl
nginx-module-xslt - nginx xslt dynamic module
nginx-module-xslt-dbg - debug symbols for the nginx-module-xslt
```

```
--prefix=/etc/nginx
--sbin-path=/usr/sbin/nginx
--conf-path=/etc/nginx/nginx.conf
--error-log-path=/var/log/nginx/error.log
--http-log-path=/var/log/nginx/access.log
--pid-path=/var/run/nginx.pid
--lock-path=/var/run/nginx.lock
--http-client-body-temp-path=/var/cache/nginx/client_temp
--http-proxy-temp-path=/var/cache/nginx/proxy_temp
--http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp
--http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp
--http-scgi-temp-path=/var/cache/nginx/scgi_temp
--user=nginx
--group=nginx
--with-http_ssl_module
--with-http_realip_module
--with-http_addition_module
--with-http_sub_module
--with-http_dav_module
--with-http_flv_module
--with-http_mp4_module
--with-http_gunzip_module
--with-http_gzip_static_module
--with-http_random_index_module
--with-http_secure_link_module
--with-http_stub_status_module
--with-http_auth_request_module
--with-threads
--with-stream
--with-stream_ssl_module
--with-http_slice_module
--with-mail
--with-mail_ssl_module
--with-file-aio
--with-http_v2_module
--with-ipv6
```

#### :books: 參考網站：
- [linux_packages](http://nginx.org/en/linux_packages.html)

---

### with-rtmp-module {#with-rtmp-module}

> 最常見的串流技術，包括：`HTTP Adaptive Bitrate Streaming`（簡稱`ABS`）、`RTSP`、`RTMP`和`HLS`等四種。
> 其中，**`RTMP`來自於`Adobe`，也是`YouTube`正在使用的串流技術，最普遍也最穩定**；另外一個`HLS`串流技術，則是蘋果公司進行線上轉播所使用的技術，同樣也兼具新技術和穩定的特性；至於，`RSTP`目前則是比較少見的技術，但也具有技術穩定的特色。
> `即時傳訊協定` (`Real-Time Messaging Protocol`，`RTMP`) ，這是`Adobe`的串流技術規格，提供有關音訊、視訊，及資料等各種`Adobe Flash`平台技術的高效能傳輸。
> 蘋果制定的`HTML5`串流媒體協定`HTTP Live Streaming` (`HLS`)
> `HLS`能將`H.264`影片轉換為多個時間長度約10秒的`MPEG2`片段，透過`HTTP`通信協定傳輸。
> `HTTP`傳輸速度雖然遜於`Adobe`先前制定的`即時訊息協定` (`Real Time Message Protocol`, `RTMP`)，但大部分的`內容傳輸網路`(`CDN`，如`Akamai`)均支援`HTTP`協定，因此網站內容可以藉由`CDN`代為傳送。另外`HTTP`對於網路設備的通透性也比其他通訊協定好，除非管理人員刻意阻擋，`HTTP`可以通過大部分的分享器或防火牆。

```
shell> brew install nginx-full --with-rtmp-module
```

```
shell> sudo apt-get update
shell> sudo apt-get install curl build-essential libpcre3-dev libpcre++-dev zlib1g-dev libcurl4-openssl-dev libssl-dev  git supervisor

shell> cd /usr/local/src
shell> wget http://nginx.org/download/nginx-1.10.2.tar.gz
shell> git clone https://github.com/arut/nginx-rtmp-module.git
shell> tar zxvf nginx-1.10.2.tar.gz

shell> cd nginx-1.10.2
shell> ./configure --add-module=../nginx-rtmp-module
shell> ./configure --add-module=../nginx-rtmp-module --with-http_ssl_module --with-http_secure_link_module --with-stream --with-stream_ssl_module
shell> make
shell> make install

shell> /usr/local/nginx/sbin/nginx -v
shell> vim /usr/local/nginx/conf/nginx.conf
shell> /usr/local/nginx/sbin/nginx -v

shell> /usr/local/nginx/sbin/nginx -s stop
shell> /usr/local/nginx/sbin/nginx -s reload
```

- `notify_method`
- `publish_time_fix`
- `buflen`

```
log_format new '$remote_addr';
access_log logs/rtmp_access.log new;
```

```
rtmp {
  server {
    listen 1935;
    chunk_size 4000;
    notify_method get;
    publish_time_fix off;
    buflen 30s;

    application live {
      live on;
      wait_key on;
      wait_video on;
      idle_streams off;
      drop_idle_publisher 10s;
    }

    application push {
      live on;
      drop_idle_publisher 10s;
    }
  }
}
```

```
rtmp {
  server {
    listen 1935;
    chunk_size 4000;

    application live {
      # enable live streaming
      live on;

      # append current timestamp to each flv
      record_unique on;

      # publish only from localhost
      allow publish 127.0.0.1;
      deny publish all;
    }

    application hls {
      live on;
      hls on;
      hls_path /tmp/hls;
    }

  }
}

http {
  server {
    location /control {
      rtmp_control all;
    }
  }
}

http {
  include       mime.types;
  default_type  application/octet-stream;
  sendfile        on;
  keepalive_timeout  65;

  server {
    listen       80;
    server_name  localhost;
    add_header X-Frame-Options SAMEORIGIN;

    location / {
      root   html;
      index  index.html index.htm;
    }

    location /stat {
      rtmp_stat all;
      rtmp_stat_stylesheet stat.xsl;
    }

    location /stat.xsl {
      root /path/to/stat/xsl/file;
    }

    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
      root   html;
    }
  }

  server {
    listen       8080;
    server_name  localhost;

    location /on_play {
    }

    location /on_publish {
    }

    location /control {
      rtmp_control all;
    }
  }
}
```

```
shell> ffmpeg -re -i input.mp4 -c copy -f flv rtmp://192.168.1.55:1935/live
shell> ffmpeg -re -i input.mp4 -c copy -vcodec libx264 -f flv rtmp://192.168.1.55:1935/live  
```

```
shell> ffplay 'rtmp://192.168.1.55:1935/live'
```

```
nginx path prefix: "/usr/local/nginx"
nginx binary file: "/usr/local/nginx/sbin/nginx"
nginx modules path: "/usr/local/nginx/modules"
nginx configuration prefix: "/usr/local/nginx/conf"
nginx configuration file: "/usr/local/nginx/conf/nginx.conf"
nginx pid file: "/usr/local/nginx/logs/nginx.pid"
nginx error log file: "/usr/local/nginx/logs/error.log"
nginx http access log file: "/usr/local/nginx/logs/access.log"
nginx http client request body temporary files: "client_body_temp"
nginx http proxy temporary files: "proxy_temp"
nginx http fastcgi temporary files: "fastcgi_temp"
nginx http uwsgi temporary files: "uwsgi_temp"
nginx http scgi temporary files: "scgi_temp"
```

```
nginx version: nginx/1.10.2
Usage: nginx [-?hvVtTq] [-s signal] [-c filename] [-p prefix] [-g directives]

Options:
  -?,-h         : this help
  -v            : show version and exit
  -V            : show version and configure options then exit
  -t            : test configuration and exit
  -T            : test configuration, dump it and exit
  -q            : suppress non-error messages during configuration testing
  -s signal     : send signal to a master process: stop, quit, reopen, reload
  -p prefix     : set prefix path (default: /usr/local/nginx/)
  -c filename   : set configuration file (default: conf/nginx.conf)
  -g directives : set global directives out of configuration file
```

#### :books: 參考網站：
- https://github.com/arut/nginx-rtmp-module
- http://www.ithome.com.tw/node/53142
- http://brew.sh/homebrew-nginx/
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options
- http://nginx.org/en/docs/http/ngx_http_secure_link_module.html

---

`ssl_handshake_timeout`
`5m`

```
stream {
  upstream backend {
    server 127.0.0.1:1935;
  }

  server {
    listen 443 ssl;
    proxy_pass backend;
    ssl_certificate     /usr/local/nginx/conf/cert.pem;
    ssl_certificate_key /usr/local/nginx/conf/cert.key;
    ssl_ciphers HIGH:!aNULL:!MD5;
    ssl_protocols TLSv1.1 TLSv1.2;
    ssl_session_timeout 4h;
    ssl_handshake_timeout 30s;
  }
}
```

```
proxy_ssl_certificate     /etc/ssl/certs/backend.crt;
proxy_ssl_certificate_key /etc/ssl/certs/backend.key;
proxy_ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
proxy_ssl_ciphers   HIGH:!aNULL:!MD5;
proxy_pass backend;
proxy_ssl  on;
```

```
ms	milliseconds
s	seconds
m	minutes
h	hours
d	days
w	weeks
M	months, 30 days
y	years, 365 days
```



#### :books: 參考網站：
- https://nginx.org/en/docs/stream/ngx_stream_core_module.html
- http://nginx.org/en/docs/http/ngx_http_ssl_module.html
- http://nginx.org/en/docs/syntax.html
- https://www.nginx.com/resources/admin-guide/nginx-tcp-ssl-upstreams/ 


---

```
error_page 404             /404.html;
error_page 500 502 503 504 /50x.html;

location = /50x.html {
    root /usr/share/nginx/html;
}

location ~ \.php$ {
    try_files $uri =404;            
    fastcgi_split_path_info       ^(.+\.php)(.*)$;
    fastcgi_pass unix/var/run/php/php7.0-fpm.sock;
    fastcgi_index index.php;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    include fastcgi_params;
}
```

#### :books: 參考網站：
- http://nginx.org/en/docs/http/ngx_http_fastcgi_module.html

---

```
location /mod_pagespeed_example {
  location ~* \.(jpg|jpeg|gif|png|js|css)$ {
    add_header Cache-Control "public, max-age=600";
  }
}
```

#### :books: 參考網站：
- https://modpagespeed.com/doc/filter-cache-extend
---

#### :books: 參考網站：
- [ngx_http_log_module](http://nginx.org/en/docs/http/ngx_http_log_module.html)
- [ssl_checker](https://www.geocerts.com/ssl_checker)
- [SSL Certificate Checker](https://www.digicert.com/help/)
- [csr-creation](https://www.digicert.com/csr-creation.htm)
- [easy-csr](https://www.digicert.com/easy-csr/openssl.htm)
- [checker](https://cryptoreport.websecurity.symantec.com/checker/)
- [ssltest](https://www.ssllabs.com/ssltest/index.html)
- [SSL/TLS Capabilities of Your Browser](https://www.ssllabs.com/ssltest/viewMyClient.html)
- [nginx](https://www.raspberrypi.org/documentation/remote-access/web-server/nginx.md)
- [Install Nginx + PHP FPM + Caching + MySQL on Ubuntu 12.04](https://www.vultr.com/docs/install-nginx-php-fpm-caching-mysql-on-ubuntu-12-04)
- [Disabling SSLv3](https://www.vultr.com/docs/disabling-sslv3)
- [Setup Nginx as Reverse Proxy Over Apache on Debian/Ubuntu](https://www.vultr.com/docs/setup-nginx-as-reverse-proxy-over-apache-on-debian-ubuntu)

---

#### :books: 參考網站：
- http://nginx.org/en/docs/http/ngx_http_index_module.html
- http://nginx.org/en/docs/http/ngx_http_autoindex_module.html


MacGyver
馬蓋先
