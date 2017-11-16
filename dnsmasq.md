`dnsmasq - Small caching DNS proxy and DHCP/TFTP server`

`dnsmasq-utils - Utilities for manipulating DHCP leases`

> 为`DHCP`和`DNS`使用`dnsmasq`是一个不错的主意，因为`dnsmasq`的配置过程很容易。

---

```
shell> apt-get install dnsmasq 

shell> brew install dnsmasq
shell> cp /usr/local/opt/dnsmasq/dnsmasq.conf.example /usr/local/etc/dnsmasq.conf
shell> sudo brew services start dnsmasq
```

`/usr/local/etc/dnsmasq.conf`

`/etc/default/dnsmasq`

`/etc/dnsmasq.conf`


```
interface=
bind-interfaces

dhcp-range=192.168.0.50,192.168.0.150,12h
dhcp-range=192.168.0.50,192.168.0.150,255.255.255.0,12h

dhcp-host=11:22:33:44:55:66,192.168.0.60
dhcp-host=11:22:33:44:55:66,192.168.0.70,infinite
dhcp-host=11:22:33:44:55:66,ignore

dhcp-option=3,1.2.3.4

dhcp-lease-max=150
dhcp-leasefile=/var/lib/misc/dnsmasq.leases
```

```
no-resolv
no-poll
server=8.8.8.8
server=8.8.4.4

server=/example.com/example.org/192.168.1.88
```

```
listen-address=127.0.0.1
listen-address=127.0.0.1,192.168.0.1

cache-size=1000
log-queries
log-facility=/var/log/dnsmasq.log

server=/localnet/192.168.0.1
server=/localnet/127.0.0.1#10053
server=/in-addr.arpa/127.0.0.1#10053
server=/ip6.arpa/127.0.0.1#10053

address=/local/127.0.0.1
address=/double-click.net/127.0.0.1
local-ttl=
```

```
shell> dnsmasq --test
dnsmasq: syntax check OK.

shell> scutil --dns
```

```
*.lan
*.local
*.localdomain
*.workgroup
```

```
listen-address=127.0.0.1

no-resolv
no-poll

address=/local/127.0.0.1

server=127.0.0.1#10053
server=/google.com/8.8.8.8
server=/google.com/8.8.4.4

server=/taobao.com/223.5.5.5
server=/taobao.com/223.6.6.6
server=/tmall.com/223.5.5.5
server=/tmall.com/223.6.6.6
```

```
dig google.com | grep "Query time"
```

---

`dnscrypt-proxy - Tool for securing communications between a client and a DNS resolver`

```
shell> brew install dnscrypt-proxy
shell> sudo brew services start dnscrypt-proxy

shell> dnscrypt-update-resolvers
```

`dnscrypt-proxy.conf`

```
# ResolverName random
ResolverName cisco

ResolversList /usr/local/opt/dnscrypt-proxy/share/dnscrypt-proxy/dnscrypt-resolvers.csv

LocalAddress 127.0.0.1:10053
```

#### :books: 參考網站：
- http://www.thekelleys.org.uk/dnsmasq/docs/dnsmasq-man.html
- https://dnscrypt.org/
- https://simplednscrypt.org/
- https://www.opendns.com/about/innovations/dnscrypt/
- https://github.com/alterstep/dnscrypt-osxclient
- http://manpages.ubuntu.com/manpages/xenial/man8/dnsmasq.8.html
- http://manpages.ubuntu.com/manpages/trusty/man8/dnsmasq.8.html
- http://www.cisco.com/c/en/us/td/docs/net_mgmt/network_registrar/7-1/user/guide/cnr71book/UGB_Opts.html
- https://technet.microsoft.com/en-us/library/cc958941.aspx


---

`dhcping - DHCP Daemon Ping Program`

```
shell> apt-get install dhcping
shell> dhcping -s 255.255.255.255 -r -v  
```

#### :books: 參考網站：
- [dhcping](http://manpages.ubuntu.com/manpages/precise/man8/dhcping.8.html)

---

`dhcpdump - DHCP packet dumper`

```
shell> sudo dhcpdump -i en0
shell> dhcpdump -i eth0
```

---

`DNS spoofing` (`DNS 詐騙`)
因採用另一系統的`網域名稱系統` (`DNS`) 而導致破壞信任關係的行為。其實施模式通常是破壞受侵害系統的名稱服務快取，或者破壞有效網域的網域名稱伺服器。


```
--ipset=/<domain>/<ipset>[,<ipset>..Specify ipsets to which matching domains should be added
-q, --log-queries                       Log DNS queries.
-R, --no-resolv                         Do NOT read resolv.conf.
-S, --server=/<domain>/<ipaddr>         Specify address(es) of upstream servers with optional domains.
--all-servers                       Always perform DNS queries to all servers.
--test                              Check configuration syntax.
```

sudo ipset create 列表名 hash:ip

```
shell> ipset create facebook hash:ip
shell> ipset create facebook hash:ip timeout 300
```

```
ipset=/.facebook.com/facebook
```

