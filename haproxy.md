<img src="http://i.imgur.com/qfadnu3.png" alt="ansible" width=200>

```
shell> lsb_release -a
shell> aptitude install build-essential
```

```
shell> sudo yum grouplist
shell> sudo yum groupinstall "Development Tools"
```

- https://docs.oracle.com/cd/E37670_01/E37355/html/ol_about_yum_groups.html
- http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/compile-software.html


```
shell> wget http://www.haproxy.org/download/1.6/src/haproxy-1.6.6.tar.gz
shell> tar xvzf haproxy-1.6.6.tar.gz
shell> cd haproxy-1.6.6  
shell> make TARGET=generic
shell> make TARGET=custom CPU=native USE_PCRE=1 USE_LIBCRYPT=1 USE_LINUX_SPLICE=1 USE_LINUX_TPROXY=1

shell> sudo mkdir -p /srv/haproxy/bin
shell> sudo mkdir -p /srv/haproxy/etc

shell> sudo cp haproxy /srv/haproxy/bin
shell> sudo vi /srv/haproxy/etc/haproxy.cfg

shell> /srv/haproxy/bin/haproxy -D -f /srv/haproxy/etc/haproxy.cfg -p haproxy.pid

shell> kill -USR1 $(cat haproxy.pid)
shell> rm -f haproxy.pid
```

`start.sh`
```sh
#! /bin/sh

/srv/haproxy/bin/haproxy -D -f /srv/haproxy/etc/haproxy.cfg -p haproxy.pid
```

`shutdown.sh`
```sh
#! /bin/sh

kill -USR1 $(cat haproxy.pid)
rm -f haproxy.pid
```

`haproxy.cfg`
```
global
        daemon 
        user haproxy
        group haproxy
        chroot /var/lib/haproxy
        maxconn         20000

defaults
        mode    http
        timeout connect 5s
        timeout client 5s
        timeout server 5s

frontend http-in
        bind       :80
        default_backend www

backend www
        balance roundrobin
        server www1 192.168.12.2:80
        server back 192.168.11.2:80
```

`haproxy.cfg`
```
global
        daemon 
        user haproxy
        group haproxy
        chroot /var/lib/haproxy
        maxconn         20000

defaults
        mode    http
        timeout connect 5s
        timeout client 5s
        timeout server 5s

frontend http-in
        bind       :80
        default_backend www

backend www
        balance roundrobin
        server www1 192.168.12.2:80
        server www2 192.168.11.2:80
```

```
backend www
        balance roundrobin
        option httpchk
        server www1 192.168.12.2:80 check inter 1000
        server www2 192.168.11.2:80 check inter 1000
```

```
backend www
        balance roundrobin
        option httpchk
        server www1 192.168.12.2:80 check inter 1000
        server www2 192.168.11.2:80 check inter 1000
```

```
backend www
        balance roundrobin
        option httpchk HEAD /
        server www1 192.168.12.2:80 check inter 1000 weight 10
        server www2 192.168.11.2:80 check inter 1000 weight 20
```

```
backend www
        balance roundrobin
        option httpchk HEAD /
        cookie SRV insert indirect nocache
        server www1 192.168.12.2:80 check inter 1000 weight 10 cookie www1
        server www2 192.168.11.2:80 check inter 1000 weight 20 cookie www2 
```

`cookie`

```
defaults
        stats enable
        stats uri     /admin?stats
        stats refresh 5s
        stats auth    admin1:AdMiN123
        stats auth    admin2:AdMiN321
```

#### :books: 參考網站：
- http://cbonte.github.io/haproxy-dconv/configuration-1.7.html

---

`/etc/default/haproxy`

```
# Set ENABLED to 1 if you want the init script to start haproxy.
ENABLED=0
# Add extra flags here.
#EXTRAOPTS="-de -m 16"
```

`/etc/haproxy/haproxy.cfg`

```
global
	log /dev/log	local0
	log /dev/log	local1 notice
	chroot /var/lib/haproxy
	user haproxy
	group haproxy
	daemon

defaults
	log	global
	mode	http
	option	httplog
	option	dontlognull
        contimeout 5000
        clitimeout 50000
        srvtimeout 50000
	errorfile 400 /etc/haproxy/errors/400.http
	errorfile 403 /etc/haproxy/errors/403.http
	errorfile 408 /etc/haproxy/errors/408.http
	errorfile 500 /etc/haproxy/errors/500.http
	errorfile 502 /etc/haproxy/errors/502.http
	errorfile 503 /etc/haproxy/errors/503.http
	errorfile 504 /etc/haproxy/errors/504.http
```

```
balance roundrobin
balance	source
```

```
global
	log /dev/log	local0
	log /dev/log	local1 notice
    maxconn	4000
    ulimit-n 8000

	chroot /var/lib/haproxy
	user haproxy
	group haproxy
	daemon

listen proxy1 0.0.0.0:8000
    mode	tcp
	bind       :80
    balance roundrobin

    server srv1 127.0.0.1:8080
    server srv2 192.168.12.3:8080
```
#### :books: 參考網站：
- [haproxy](http://www.haproxy.org/)
