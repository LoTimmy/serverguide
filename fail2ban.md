### 在 `Ubuntu` 14.04 LTS 上建置 `fail2ban`

`自動封鎖`


安裝作業系統及`fail2ban`相關套件。
因本文主要介紹`Apache`如何安裝及設定，作業系統方面就不再詳述。

```
shell> lsb_release -a
```
```
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 14.04 LTS
Release:	14.04
Codename:	trusty
```

### 安裝 fail2ban 

```
shell> aptitude install fail2ban
```

---

```
shell> fail2ban-client status
Status
|- Number of jail:	1
`- Jail list:		ssh
```

```
shell> fail2ban-client status ssh
Status for the jail: ssh
|- filter
|  |- File list:	/var/log/auth.log 
|  |- Currently failed:	3
|  `- Total failed:	130
`- action
   |- Currently banned:	0
   |  `- IP list:	
   `- Total banned:	4
```

```
shell> iptables -n -L INPUT
```

```
shell> fail2ban-regex /var/log/auth.log /etc/fail2ban/filter.d/sshd.conf
shell> fail2ban-regex /var/log/auth.log /etc/fail2ban/filter.d/sshd-ddos.conf
```

```
shell> fail2ban-client set ssh unbanip <IP> 
shell> fail2ban-client set postfix-sasl unbanip <IP> 
```

```
shell> fail2ban-client -i
fail2ban> status postfix-sasl
fail2ban> status set postfix-sasl unbanip <IP>
```


`/etc/fail2ban/jail.conf`

```
[DEFAULT]
ignoreip = 127.0.0.1/8 192.168.0.0/16
# bantime  = 600
bantime  = -1
# findtime  = 600
findtime  = 300
# maxretry = 3
maxretry = 10


# banaction = iptables-multiport
banaction = iptables-ipset-proto4
```

嘗試登入次數: `10`
幾分鐘內: `5`

啟動封鎖過期
當啟動封鎖過期功能時，在下列天數後，被封鎖的 IP 將會被解除封鎖。


```
[ssh]

enabled  = true
port     = ssh
filter   = sshd
logpath  = /var/log/auth.log
maxretry = 6

[sshd-ddos]

enabled  = true
port     = ssh
filter   = sshd-ddos
logpath  = /var/log/auth.log
maxretry = 6
```

```
shell> tar xvfj fail2ban-0.9.4.tar.bz2
shell> cd fail2ban-0.9.4
shell> python setup.py install
shell> cp files/debian-initd /etc/init.d/fail2ban
```
