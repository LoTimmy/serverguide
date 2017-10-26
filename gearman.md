<a name="getstarted"></a>

![](http://gearman.org/img/logo.png)

![](http://gearman.org/img/stack.png)


---

安裝作業系統及`gearman`相關套件。
因本文主要介紹`gearman`如何安裝及設定，作業系統方面就不再詳述。

``` 
shell> lsb_release -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 14.04.1 LTS
Release:	14.04
Codename:	trusty
```

``` 
shell> gearmand -V
gearmand 1.0.6 - https://bugs.launchpad.net/gearmand
```

Worker
```
shell> gearman -w -f wc -- wc -l
```

Client
```
$ gearman -f wc < /etc/passwd
```


```
gearman - Distributed job queue
gearman-job-server - Job server for the Gearman distributed job queue
gearman-server - Gearman distributed job server and Perl interface
gearman-tools - Tools for the Gearman distributed job queue
```

``` 
shell> mysql -u gearman -p -e 'CREATE DATABASE gearman;'
```

/etc/default/gearman-job-server

```
PARAMS="-q libdrizzle --libdrizzle-host=10.0.0.1 --libdrizzle-user=gearman \
                      --libdrizzle-password=secret --libdrizzle-db=gearman \
                      --libdrizzle-table=gearman_queue --libdrizzle-mysql"

```

```
PARAMS="-q 


PARAMS="-q mysql --mysql-host=localhost --mysql-user=xxxx --mysql-password=xxxxx--mysql-db=gearman --mysql-table=gearman_queue"
```

```
MySQL:
  --mysql-host arg (=localhost)      MySQL host.
  --mysql-port arg (=3306)           Port of server. (by default 3306)
  --mysql-user arg                   MySQL user.
  --mysql-password arg               MySQL user password.
  --mysql-db arg                     MySQL database.
  --mysql-table arg (=gearman_queue) MySQL table name.
```





- [job_server](http://gearman.org/manual/job_server/)

---

```
extension="gearman.so"
```

```php
<?php
print gearman_version() . "\n";
?>
```

``` 
shell> php gearman_version.php
```

```php
<?php
$client = new GearmanClient();
$client->addServer();

print $client->doBackground("reverse", "Hello World!");
```

```php
<?php

// Create Worker Object
$worker= new GearmanWorker();

// Add Gearman Job Servers to Worker
$worker->addServer(); 

$worker->addFunction("reverse", function ($job) {
  file_put_contents('people.txt', $job->workload() . "\n", FILE_APPEND | LOCK_EX);  
});
while ($worker->work());
```

- [gearmanclient.dobackground](http://php.net/manual/en/gearmanclient.dobackground.php)
- [getting-started](http://gearman.org/getting-started/)
- [reverse](http://gearman.org/examples/reverse/)

---

```js
//  Created by Timmy on 2015/2/24.
//  Copyright (c) 2015年 Timmy. All rights reserved.

// 导入所需模块
var Gearman = require("node-gearman");
var gearman = new Gearman(); // by default host/port will be "localhost" & 4730 
var gearman = new Gearman(hostname, port);

gearman.connect();

gearman.on("connect", function() {
  console.log("Connected to the server!");
});

var job = gearman.submitJob("reverse", "test string");

job.on("data", function(data) {
  console.log(data.toString("utf-8")); // gnirts tset
});

job.on("end", function() {
  console.log("Job completed!");
  gearman.close();
});

job.on("error", function(error) {
  console.log(error.message);
});

gearman.on("close", function() {
  console.log("Connection closed");
  console.log("Bye");
});
```

```js
//  Created by Timmy on 2015/2/24.
//  Copyright (c) 2015年 Timmy. All rights reserved.

// 导入所需模块
var Gearman = require("node-gearman");
var gearman = new Gearman('localhost', '4730');

gearman.on("connect", function() {
  console.log("Connected to the server!");
});

gearman.connect();

gearman.registerWorker("reverse", function(payload, worker) {
  if (!payload) {
    worker.error();
    return;
  }
  var reversed = payload.toString("utf-8").split("").reverse().join("");
  console.log(reversed);
  worker.end(reversed);
});
```

- [node-gearman](https://www.npmjs.com/package/node-gearman)

---

PARAMS="-q mysql --mysql-host=localhost --mysql-user=user --mysql-password=password --mysql-db=gearman --mysql-table=gearman_queue"


- [gearman](http://gearman.org/)
