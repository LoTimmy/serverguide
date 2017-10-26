![](http://www.ntop.org/wp-content/uploads/2011/08/logo_new_m.png)

----------

##### 安裝`ntop`

安裝`ntop`套件。
``` 
shell> apt-get install ntop -y
shell> apt-get install ntopng -y
```

`/etc/ntopng.conf`
```
-e=
-i=eth0
-w=3000
```


```
shell> ntop --set-admin-password=<pass>
```

切換至「/var/lib/ntop」目錄下。
```
shell> cd /var/lib/ntop
```
```
shell> vi init.cfg
```
```ini
USER="ntop"
INTERFACES="eth1"
```
```
shell> service ntop restart
```

#### :books: 參考網站：
- http://www.ntop.org/products/traffic-analysis/ntop/
