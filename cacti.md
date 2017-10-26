<img src="http://i.imgur.com/HiXoTOy.png" width="50">

```
cacti	cacti/app-password-confirm	password	
# MySQL application password for cacti:
cacti	cacti/mysql/app-pass	password	
cacti	cacti/mysql/admin-pass	password	
cacti	cacti/password-confirm	password	
# MySQL database name for cacti:
cacti	cacti/db/dbname	string	cacti
# Connection method for MySQL database of cacti:
cacti	cacti/mysql/method	select	Unix socket
# Host running the MySQL server for cacti:
cacti	cacti/remote/newhost	string	
# Configure database for cacti with dbconfig-common?
cacti	cacti/dbconfig-install	boolean	true
cacti	cacti/remote/port	string	
cacti	cacti/internal/skip-preseed	boolean	false
# Delete the database for cacti?
cacti	cacti/purge	boolean	false
cacti	cacti/webserver	select	apache2
# Back up the database for cacti before upgrading?
cacti	cacti/upgrade-backup	boolean	true
# Reinstall database for cacti?
cacti	cacti/dbconfig-reinstall	boolean	false
# MySQL username for cacti:
cacti	cacti/db/app-user	string	cacti
cacti	cacti/install-error	select	abort
# Perform upgrade on database for cacti with dbconfig-common?
cacti	cacti/dbconfig-upgrade	boolean	true
cacti	cacti/upgrade-error	select	abort
cacti	cacti/remove-error	select	abort
# Host name of the MySQL database server for cacti:
cacti	cacti/remote/host	select	localhost
# Database type to be used by cacti:
cacti	cacti/database-type	select	mysql
# Deconfigure database for cacti with dbconfig-common?
cacti	cacti/dbconfig-remove	boolean	true
cacti	cacti/mysql/admin-user	string	debian-sys-maint
cacti	cacti/internal/reconfiguring	boolean	false
cacti	cacti/missing-db-package-error	select	abort
cacti	cacti/passwords-do-not-match	error	
```

`/usr/share/cacti/site/lib/functions.php`

```php
<?php

setlocale(LC_ALL, 'zh_TW.UTF-8');

setlocale(LC_CTYPE,"zh_TW.UTF-8");
```

---

`Configuration` → `Settings` → `Poller`

`Poller Interval` → `Every Minute`
`Cron Interval` → `Every Minute`

---

`Templates` → `Data Templates` → `Interface - Traffic`
`Step` → `60`
`Associated RRA's` → `Hourly (1 Minute Average)`

`/etc/cron.d/cacti`

```
MAILTO=root
*/5 * * * * www-data php --define suhosin.memory_limit=512M /usr/share/cacti/site/poller.php 2>&1 >/dev/null | if [ -f /usr/bin/ts ] ; then ts ; else tee ; fi >> /var/log/cacti/poller-error.log
```

`Utilities` → `System Utilities` → `Rebuild Poller Cache`

```
shell> cd /usr/share/cacti/cli
shell> php -q rebuild_poller_cache.php -d
```

---

```
shell> dpkg --purge javascript-common
shell> apt-get install javascript-common
```

---
#### :books: 參考網站：
- http://www.cacti.net/index.php
- [unix_configure_cacti](http://www.cacti.net/downloads/docs/html/unix_configure_cacti.html)
- [輕鬆做好流量管理—Cacti（上）](http://www.netadmin.com.tw/article_content.aspx?sn=1212060003)
- [輕鬆做好流量管理—Cacti（下）](http://www.netadmin.com.tw/article_content.aspx?sn=1301020001)

---

```
shell> esxcli system snmp set -c public
shell> esxcli system snmp set -e yes
```

`Cacti` → `Console` → `Import Templates`

```
shell> wget http://docs.cacti.net/_media/usertemplate:host:cacti_esxi_template.02.tar.gz -O cacti_esxi_template.02.tar.gz
shell> tar zxvf cacti_esxi_template.02.tar.gz
shell> mkdir /var/lib/cacti/scripts
shell> cd cacte_esxi_template
shell> cp -r scripts /var/lib/cacti
shell> cp -r resource/snmp_queries /usr/share/cacti/resource
```

- `/var/lib/cacti`
- `/usr/share/cacti/resource/snmp_queries`

#### :books: 參考網站：
- http://docs.cacti.net/usertemplate:host:esxi
- http://www.cacti.net/downloads/docs/html/snmp_query_xml.html
- http://docs.cacti.net/plugins
- http://docs.cacti.net/templates

#### :books: 參考網站：
- http://docs.cacti.net/usertemplate:host:microsoft:sqlserver
- http://docs.cacti.net/usertemplate:data:informant:memory_stats

---

`Cacti` → `Console` → `Settings` → `Visual` → `Maximum Field Length` → ~`15`~ → `30` 
`Cacti` → `Console` → `Graph Templates` → `Interface - Traffic (bits/sec)` → `Title (--title)` → ~`|host_description| - Traffic`~ → `|host_description| - Traffic - |query_ifName|`




