> `遠程驗證撥接使用服務` (`Remote Authentication Dial-In User Service`, `RADIUS`)
> `RADIUS` (`Remote Authentication Dial In User Service`)
> `AAA`
> `Authentication` `驗證`
> `Authorization` `授權`
> `Accounting` `計量`
> `NAS`
> `802.1 x`
> `可延伸驗證通訊協定` (`EAP`)

### 在 `Ubuntu` 16.04.2 LTS 上建置 `freeradius`

``` 
shell> lsb_release -a
```
```
Distributor ID:	Ubuntu
Description:	Ubuntu 16.04.2 LTS
Release:	16.04
Codename:	xenial
```

```
freeradius - high-performance and highly configurable RADIUS server
freeradius-mysql - MySQL module for FreeRADIUS server
```

### 安裝 `freeradius` 
```
shell> sudo apt-get install -y mysql-server-5.7
shell> sudo apt-get install -y freeradius freeradius-mysql 
shell> sudo service freeradius stop
```

`/etc/freeradius`

```
shell> sudo cp /etc/freeradius/users /etc/freeradius/users.original
shell> sudo chmod a-w /etc/freeradius/users.original
```

`users`
```
steve	Cleartext-Password := "testing"
	Service-Type = Framed-User,
	Framed-Protocol = PPP,
	Framed-IP-Address = 172.16.3.33,
	Framed-IP-Netmask = 255.255.255.0,
	Framed-Routing = Broadcast-Listen,
	Framed-Filter-Id = "std.ppp",
	Framed-MTU = 1500,
	Framed-Compression = Van-Jacobson-TCP-IP
```
```
janedoe	User-Password := "6KqrYDRq"
janedoe	Crypt-Password := "Rqz3xDrdrkLAk"
janedoe	MD5-Password := "cdac0771bac5c2d1f4f0399af81fe317"
```

```
shell> freeradius -X
```
```
shell> radtest steve testing localhost 1812 testing123
shell> radtest janedoe 6KqrYDRq localhost 1812 testing123
```

```
Sending Access-Request of id 38 to 127.0.0.1 port 1812
	User-Name = "steve"
	User-Password = "testing"
	NAS-IP-Address = 127.0.1.1
	NAS-Port = 1812
	Message-Authenticator = 0x00000000000000000000000000000000
rad_recv: Access-Accept packet from host 127.0.0.1 port 1812, id=38, length=71
	Service-Type = Framed-User
	Framed-Protocol = PPP
	Framed-IP-Address = 172.16.3.33
	Framed-IP-Netmask = 255.255.255.0
	Framed-Routing = Broadcast-Listen
	Filter-Id = "std.ppp"
	Framed-MTU = 1500
	Framed-Compression = Van-Jacobson-TCP-IP
```

- `Access-Request`
- `Access-Accept`
- `Access-Reject`
- `Access-Challenge`



`clients.conf`
```
client localhost {
	secret		= testing123
}
```

```
client 192.168.88.120 {
	ipaddr = 192.168.88.120
	secret		= testing123
	require_message_authenticator = no
	nastype     = other
}
```


```
shell> lsof -n -i -P | grep -E '^COMMAND|freeradiu'
```

```
COMMAND     PID    USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
freeradiu 12276 freerad    5u  IPv4 195719      0t0  UDP *:1812 
freeradiu 12276 freerad    6u  IPv4 195720      0t0  UDP *:1813 
freeradiu 12276 freerad    7u  IPv4 195721      0t0  UDP 127.0.0.1:18120 
freeradiu 12276 freerad    8u  IPv4 195722      0t0  UDP *:1814 
freeradiu 12276 freerad    9u  IPv4 195723      0t0  UDP *:54563 
```

#### :books: 參考網站：
- http://www.cisco.com/c/en/us/td/docs/net_mgmt/access_registrar/1-7/concepts/guide/radius.html
- https://www.ibm.com/support/knowledgecenter/zh-tw/ssw_aix_61/com.ibm.aix.security/radius_config_dictionary.htm
- https://www.intel.com.tw/content/www/tw/zh/support/network-and-i-o/wireless-networking/000006999.html
- https://technet.microsoft.com/zh-tw/library/dd197481(v=ws.10).aspx

---

`/etc/freeradius/sql/mysql`

```
shell> mysql -u root -p -e "CREATE DATABASE radius;"
shell> mysql -u root -p radius < admin.sql
shell> mysql -u root -p radius < schema.sql
shell> mysql -u radius -p radius
```

`radpass`

```
shell> sudo cp /etc/freeradius/radiusd.conf /etc/freeradius/radiusd.conf.original
shell> sudo chmod a-w /etc/freeradius/radiusd.conf.original

shell> sudo cp /etc/freeradius/sql.conf /etc/freeradius/sql.conf.original
shell> sudo chmod a-w /etc/freeradius/sql.conf.original
```

`radiusd.conf`
```
	$INCLUDE sql.conf
```

`sql.conf`
```
sql {
        database = "mysql"
        driver = "rlm_sql_${database}"
        server = "localhost"
        #port = 3306
        login = "radius"
        password = "radpass"
        radius_db = "radius"
        [...]
}

```

`/etc/freeradius/sites-available/default`
```
authorize {
        [...]
#       files
        sql
        [...]
}

[...]

accounting {
        [...]
        sql
        [...]
}

session {
        radutmp
        sql
}

post-auth {
        [...]
        sql
        [...]
}
```
`/etc/freeradius/sql/mysql/dialup.conf`

```
        # Uncomment simul_count_query to enable simultaneous use checking
        simul_count_query = "SELECT COUNT(*) \
                             FROM ${acct_table1} \
                             WHERE username = '%{SQL-User-Name}' \
                             AND acctstoptime IS NULL"
```

```
+------------------+
| Tables_in_radius |
+------------------+
| radacct          |
| radcheck         |
| radgroupcheck    |
| radgroupreply    |
| radpostauth      |
| radreply         |
| radusergroup     |
+------------------+
```

`radcheck`
```
+-----------+------------------+------+-----+---------+----------------+
| Field     | Type             | Null | Key | Default | Extra          |
+-----------+------------------+------+-----+---------+----------------+
| id        | int(11) unsigned | NO   | PRI | NULL    | auto_increment |
| username  | varchar(64)      | NO   | MUL |         |                |
| attribute | varchar(64)      | NO   |     |         |                |
| op        | char(2)          | NO   |     | ==      |                |
| value     | varchar(253)     | NO   |     |         |                |
+-----------+------------------+------+-----+---------+----------------+
```

`radusergroup`

```
+-----------+-------------+------+-----+---------+-------+
| Field     | Type        | Null | Key | Default | Extra |
+-----------+-------------+------+-----+---------+-------+
| username  | varchar(64) | NO   | MUL |         |       |
| groupname | varchar(64) | NO   |     |         |       |
| priority  | int(11)     | NO   |     | 1       |       |
+-----------+-------------+------+-----+---------+-------+
```

`radreply`
```
+-----------+------------------+------+-----+---------+----------------+
| Field     | Type             | Null | Key | Default | Extra          |
+-----------+------------------+------+-----+---------+----------------+
| id        | int(11) unsigned | NO   | PRI | NULL    | auto_increment |
| username  | varchar(64)      | NO   | MUL |         |                |
| attribute | varchar(64)      | NO   |     |         |                |
| op        | char(2)          | NO   |     | =       |                |
| value     | varchar(253)     | NO   |     |         |                |
+-----------+------------------+------+-----+---------+----------------+
```

`radgroupreply`
```
+-----------+------------------+------+-----+---------+----------------+
| Field     | Type             | Null | Key | Default | Extra          |
+-----------+------------------+------+-----+---------+----------------+
| id        | int(11) unsigned | NO   | PRI | NULL    | auto_increment |
| groupname | varchar(64)      | NO   | MUL |         |                |
| attribute | varchar(64)      | NO   |     |         |                |
| op        | char(2)          | NO   |     | =       |                |
| value     | varchar(253)     | NO   |     |         |                |
+-----------+------------------+------+-----+---------+----------------+
```

```
INSERT INTO radcheck (username, attribute, op, value) VALUES ('janedoe', 'User-Password', ':=' ,'6KqrYDRq');
INSERT INTO radcheck (username, attribute, op, value) VALUES ('janedoe', 'Crypt-Password', ':=' ,'Rqz3xDrdrkLAk');
INSERT INTO radcheck (username, attribute, op, value) VALUES ('janedoe', 'MD5-Password', ':=' ,'cdac0771bac5c2d1f4f0399af81fe317');

INSERT INTO radreply (username, attribute, op, value) VALUES ('janedoe', 'Framed-IP-Address', ':=', '172.16.3.33'); 
```

```
INSERT INTO radgroupreply (groupname, attribute, op, value) VALUES ('user', 'Auth-Type', ':=','LOCAL');
INSERT INTO radgroupreply (groupname, attribute, op, value) VALUES ('user', 'Service-Type', ':=', 'Framed-User');
INSERT INTO radgroupreply (groupname, attribute, op, value) VALUES ('user', 'Framed-IP-Address', ':=', '172.16.3.33');
INSERT INTO radusergroup (username, groupname) VALUES ('janedoe', 'user');
```

```
shell> radtest janedoe 6KqrYDRq localhost 0 testing123
```

```
Sending Access-Request of id 37 to 127.0.0.1 port 1812
	User-Name = "test"
	User-Password = "test"
	NAS-IP-Address = 127.0.1.1
	NAS-Port = 0
	Message-Authenticator = 0x00000000000000000000000000000000
rad_recv: Access-Accept packet from host 127.0.0.1 port 1812, id=37, length=32
	Service-Type = Framed-User
	Framed-IP-Address = 172.16.3.33
```

#### :books: 參考網站：
- https://opensource.apple.com/source/freeradius/freeradius-32/freeradius/raddb/sql/mssql/schema.sql


---

- `radwho`
- `radlast`
- `radwatch`


---

`daloradius`

```
shell> sudo apt install apache2 libapache2-mod-php php7.0-gd php-pear php-db php-mysql
shell> wget --content-disposition https://sourceforge.net/projects/daloradius/files/latest/download?source=files
shell> tar zxvf daloradius-0.9-9.tar.gz
shell> mv daloradius-0.9-9 /var/www/daloradius 

shell> chown www-data:www-data /var/www/daloradius -R
shell> chmod 644 /var/www/daloradius/library/daloradius.conf.php

shell> /var/www/daloradius/contrib/db
shell> mysql -u root -p radius < mysql-daloradius.sql

/var/www/daloradius/library
daloradius.conf.php

```

```php
$configValues['FREERADIUS_VERSION'] = '2';
$configValues['CONFIG_DB_ENGINE'] = 'mysqli';
$configValues['CONFIG_DB_NAME'] = 'radius';
$configValues['CONFIG_DB_USER'] = 'radius';
$configValues['CONFIG_DB_PASS'] = 'radpass';
$configValues['CONFIG_DB_TBL_RADUSERGROUP'] = 'radusergroup';
```

`opendb.php`

```php
[...]
$dbSocket->query("SET GLOBAL sql_mode = '';");
```


http://192.168.88.120/daloradius/
`administrator`
`radius`

```
Database connection error
Error Message: DB Error: extension not found
```

