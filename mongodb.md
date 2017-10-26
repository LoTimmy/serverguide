![](https://raw.githubusercontent.com/docker-library/docs/master/mongo/logo.png)

- `MongoDB`為一跨平台的文件導向開放源碼資料庫系統，是目前最受歡迎的`非關聯性資料庫` (`NoSQL`)。
- `MongoDB`兼具關聯式資料多鍵值查詢的方便性，又有`NoSQL`資料庫處理大量資料的速度。
- 可橫向擴展的`NoSQL`資料庫`MongoDB`。`MongoDB`橫向擴展的能力，不只能讓資料庫儲存大量資料，也能具備承載瞬間大量系統存取的能力。
- `MongoDB`對於`JSON`(JavaScript Object Notation)格式有良好的支援度，使用者能在資料表中嵌入資料檔案以支援一對多表格關係(One-to-Many Relationships)，且`MongoDB`支援2級索引，即使資料檔案的格式沒有事先定義，也能讓資料庫直接搜尋這個檔案內部的資料。`MongoDB`在資料表中儲存`JSON`格式的檔案，搭配二級索引功能，就能取代多子表的資料庫設計方式。關聯式資料庫雖然也能將檔案存進資料表中，但是僅能使用`Blob`(Binary Large Object)格式，這種格式對於資料庫來說是黑盒子，無法對檔案內容進行搜尋，使用者需要從資料庫讀取出來，再用程式解析成需要的資訊。
- `NoSQL`資料庫`MongoDB`釋出了`MongoDB` 2.6，全面強化核心伺服器，有全新的自動化工具與重要的企業功能，宣稱是`MongoDB`問世5年來最大的一次版本發表，主要改善操作、開發人員經驗，與大型企業的適用性。
- 根據`MongoDB`的設計，資料量成長到預設標準值，資料切割自動化的機制就會啟動，並且快速增加一臺伺服器，把資料平均分散在每一臺伺服器上，不僅可以達到橫向擴充目的，還能讓系統效能維持在最佳狀態。
- 相較於關聯式資料庫，`MongoDB`有幾個特色，除了能因應資料大量讀寫的需求，最重要的是，`MongoDB`還具有`Auto-sharding`（`資料自動切割`）的功能，可以自動切割資料來橫向資料庫的容量，而且具有`自動容錯移轉`(`Auto Failover`)機制。使用者只要事先設定標準，當資料量達到預設標準值，`MongoDB`的Auto-sharding功能就會自動進行資料切割，並且將資料平均分配到不同主機 (`Shard`)。
- 關聯式資料庫的資料庫越來越大後，存取效能會降低，一般作法上，會透過資料表切割的方式，將原有的資料分散到不同的資料表或資料庫上，來提高存取效能。但是一份資料切割成多份後，不但資料彙整的複雜性增加，應用程式也需要能夠辨認出不同資料的所在位置，往往需要修改現有應用程式或另外客製開發處理，這就是手動式的資料分割作法，相當麻煩也費時，才能讓資料庫做到`垂直擴充`（`Scale Up`）或是`橫向擴充`（`Scale Out`）。而`MongoDB`就是在資料庫中內建了資料分割機制，企業就不用再自行處理資料切割的工作。
- 由於`MongoDB`的資料儲存結構設計，沒有關聯式資料庫的概念，例如，關聯式資料庫最擅長的資料`合併` (`Join`)以及`交易` (`Transaction`)等，`MongoDB`通通沒有。
- `MongoDB`與`MySQL`不是全有全無，而是可以互補，依據資料庫本身的特性與資料屬性重新配置。例如：更新頻繁的資料如`Log`等就使用`MongoDB`，其他需要跨資料表合併的資料則儲存在`MySQL`。
- 關聯式資料庫因為需要事先定義資料欄位 (Schema)，往往需要很多時間才能更改資料欄位，`MongoDB`的資料儲存結構設計，因為不需要事先定義 (Schema-free)，所以，不論新增或調整部分服務功能，都不需要去調整原本的資料欄位就能完成。
- `MongoDB`是文件導向型資料庫，不需要明確定義資料欄位，資料表 (`Collection`)中可儲存長度不定、欄位數量不限的資料，而且每個欄位還可以再切分成多個子欄位，例如：HTML網頁裡有Head與Body結構，Body元素中可能會有多個段落，段落中會有文字、圖片、連結等，MongoDB的資料結構是比較鬆散的樹狀結構。
- `MongoDB`連續兩年(2013/2014)拿下DB-Engines.COM公司所頒發的DBMS of the year獎項。這個`文件式資料庫` (`Document-based Database`) 從2007開始，到目前已經是`NoSQL`資料庫領域的領先者。在整個資料庫產業裡，也以僅次於`Oracle`、`MySQL`、`Microsoft SQL Server`的姿態，成為目前資料庫領域的第四名 (DB-Engines發佈)。
- `MongoDB`以開放原始碼、免費、架構簡單、易學易用、效能佳等優點成為許多新創公司建構系統的首選。傳統上`LAMP` (Linux/Apache/MySQL/Perl,Python)的架構，有許多人倡議要將`MySQL`改為`MongoDB`，不過`MySQL`還是有其優勢，特別是`交易` (`transaction`) 控制方面，是目前NoSQL資料庫尚未克服的部分，但其他方面的應用，`MongoDB`就足以與任何資料庫匹敵。


---

**mongod - MongoDB Server**

```
shell> iptables -A INPUT -s <ip-address> -p tcp --destination-port 27017 -m state --state NEW,ESTABLISHED -j ACCEPT
shell> iptables -A OUTPUT -d <ip-address> -p tcp --source-port 27017 -m state --state ESTABLISHED -j ACCEPT
```

[employeeName.csv](https://dl.dropboxusercontent.com/u/4276183/docs/employeeName.csv)

---

### 安裝 {#installing}

**Install MongoDB on Ubuntu**

```
shell> sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6

shell> echo "deb [ arch=amd64 ] http://repo.mongodb.org/apt/ubuntu precise/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list
shell> echo "deb [ arch=amd64 ] http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list
shell> echo "deb [ arch=amd64,arm64 ] http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list

shell> sudo apt-get update
shell> sudo apt-get install -y mongodb-org
```

**Install Percona Server for MongoDB on Ubuntu**
```
shell> sudo apt-key adv --keyserver keys.gnupg.net --recv-keys 1C4CBDCDCD2EFD2A
shell> echo "deb http://repo.percona.com/apt "$(lsb_release -sc)" main" | sudo tee /etc/apt/sources.list.d/percona.list
shell> apt-get update
shell> sudo apt-get install percona-server-mongodb
```

- [Installing Percona Server for MongoDB on Debian and Ubuntu](https://www.percona.com/doc/percona-server-for-mongodb/installation/apt_repo.html)
- https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/


**Start MongoDB**
```
shell> sudo service mongod start
```

**Stop MongoDB**
```
shell> sudo service mongod stop
```

**Restart MongoDB**
```
shell> sudo service mongod restart
```

```
shell> sudo systemctl --system daemon-reload
shell> sudo systemctl enable mongod.service
shell> sudo systemctl start mongod.service
```

`/etc/mongod.conf`
```
storage:
  dbPath: /var/lib/mongodb
  journal:
    enabled: true

processManagement:
  fork: true
  pidFilePath: /var/run/mongod.pid

# network interfaces
net:
  port: 27017
  bindIp: 172.16.7.103

security:
   authorization: enabled
net:
   bindIp: 127.0.0.1,10.8.0.10,192.168.4.24
```

```
replSet = rs0
```

#### :books: 參考網站：
- https://docs.mongodb.com/manual/reference/configuration-options/
- https://docs.mongodb.com/manual/administration/configuration/

---

<img src="http://i.imgur.com/enCNaiB.png" width="400">

```
shell> mongo
shell> mongo --eval 'db.collection.find().forEach(printjson)'
```

```js
rs.initiate()
rs.conf()
rs.add(""mongodb1.example.net"")
rs.add(""mongodb2.example.net"")
```

---

```js
show dbs
use mydb
db
```

```js
show dbs, show databases
show collections
show users
show roles
show logs
```

```js
var person1 = {name:"John Doe", age:25};
var person2 = {name:"Jane Doe", age:26, dept: 115};
db.employees.save(person1);
db.employees.save(person2);
db.employees.find();
```

#### :books: 參考網站：
- https://docs.mongodb.com/v3.2/tutorial/write-scripts-for-the-mongo-shell/

---

**新增 (Insert)**

```js
use mydb
db.testData.insert( { name : "mongo" } )
db.testData.insert( { x : 3 } )
db.testData.insert( { CompanyName : "Microsoft Corporation" } )
db.testData.insert( { CompanyName : "CompanyName"} )

db.testData.insert( {"_id":"conf","CompanyName":"CompanyName"} )

db.friends.insert( {name:'John', age:25, gender:'boy'} )
db.friends.insert( {name:'Jessie', age:30, gender:'girl'} )
db.friends.insert( {name:'Johanna', age:28, gender:'girl'} )
db.friends.insert( {name:'Joy', age:15, gender:'girl'} )
db.friends.insert( {name:'Mary', age:28, gender:'girl'} )
db.friends.insert( {name:'Peter', age:95, gender:'boy'} )
db.friends.insert( {name:'Sebastian', age:50, gender:'boy'} )
db.friends.insert( {name:'Erika', age:27, gender:'girl'} )
db.friends.insert( {name:'Patrick', age:40, gender:'boy'} )
db.friends.insert( {name:'Samantha', age:60, gender:'girl'} )

for(var i = 1; i < 10; i++) db.testData.insert( { x : i } );

db.contacts.insert( { name: "Amanda", status: "Updated" } )

j = { name : "mongo" }
k = { x : 3 }
db.testData.insert( j )
db.testData.insert( k )
```

```js
name = { first: "Yukihiro",  last: "Matsumoto" }
db.testData.insert(name)

name = {}
name.first = 'Yukihiro'
name.last = 'Matsumoto'
db.testData.insert(name)
```

```js
db.createCollection("myColl")
db.createCollection("people", { size: 2147483648 } )
```

#### :books: 參考網站：
- [db.createCollection](https://docs.mongodb.org/manual/reference/method/db.createCollection/#db.createCollection)
- [db.collection.insert](https://docs.mongodb.com/manual/reference/method/db.collection.insert/)

---

**查詢 (Find)**

**Find All Documents in a Collection**

```js
db.getCollection('testData').find({})
db.testData.find()
```
```js
db.testData.find( { x: 18 } )
db.testData.find( { name: "mongo" } )
db.inventory.find( { qty: { $gt: 4 } } )
db.inventory.find( { qty: { $lt: 4 } } )
db.inventory.find( { qty: { $gte: 20 } } )
db.inventory.find( { qty: { $lte: 20 } } )
db.inventory.find( { qty: { $in: [ 5, 15 ] } } )
db.inventory.find( { qty: { $nin: [ 5, 15 ] } } )
db.inventory.find( { $or: [ { quantity: { $lt: 20 } }, { price: 10 } ] } )

db.testData.find( { "name": /m+/ } )

db.orders.find().sort( { amount: -1 } )

db.books.find().pretty()
```


```js
db.testData.find(ObjectId("507f191e810c19729de860ea"));
ObjectId("507f191e810c19729de860ea").str
```

#### :books: 參考網站：
- [db.collection.find](https://docs.mongodb.com/manual/reference/method/db.collection.find/)
- [gte](https://docs.mongodb.com/manual/reference/operator/query/gte/)
- [lte](https://docs.mongodb.com/manual/reference/operator/query/lte/)
- [in](https://docs.mongodb.com/manual/reference/operator/query/in/)
- [nin](https://docs.mongodb.com/manual/reference/operator/query/nin/)
- [or](https://docs.mongodb.com/manual/reference/operator/query/or/)
- [sort](https://docs.mongodb.com/v3.2/reference/method/cursor.sort/)
- https://docs.mongodb.com/manual/reference/method/ObjectId/

---

**更新 (Update)**

```js
db.testData.update( { x : 3 }, { x : 18 } )
```

#### :books: 參考網站：
- [db.collection.update](https://docs.mongodb.com/manual/reference/method/db.collection.update/)

---

**移除 (Remove)**

**Remove All Documents from a Collection**

```js
db.testData.remove( { } )
```

```js
db.products.remove( { qty: { $gt: 20 } } )
```

#### :books: 參考網站：
- https://docs.mongodb.com/manual/reference/method/db.collection.remove/

---

```js
db.orders.count()
db.orders.count( { ord_dt: { $gt: new Date('01/01/2012') } } )
```

#### :books: 參考網站：
- https://docs.mongodb.com/manual/reference/method/db.collection.count/

---

```js
db.myCollection.find( { $where: "this.credits == this.debits" } );
db.myCollection.find( { $where: "obj.credits == obj.debits" } );

db.myCollection.find( { $where: function() { return (this.credits == this.debits) } } );
db.myCollection.find( { $where: function() { return obj.credits == obj.debits; } } );
```

#### :books: 參考網站：
- [where](https://docs.mongodb.com/manual/reference/operator/query/where/)

---

```json
{ "_id": 1, "dept": "A", "item": { "sku": "111", "color": "red" }, "sizes": [ "S", "M" ] }
{ "_id": 2, "dept": "A", "item": { "sku": "111", "color": "blue" }, "sizes": [ "M", "L" ] }
{ "_id": 3, "dept": "B", "item": { "sku": "222", "color": "blue" }, "sizes": "S" }
{ "_id": 4, "dept": "A", "item": { "sku": "333", "color": "black" }, "sizes": [ "S" ] }
```

```js
db.inventory.distinct( "dept" )
```

#### :books: 參考網站：
- [db.collection.distinct](https://docs.mongodb.com/manual/reference/method/db.collection.distinct/)

---

```js
db.orders.group(
   {
     key: { ord_dt: 1, 'item.sku': 1 },
     cond: { ord_dt: { $gt: new Date( '01/01/2012' ) } },
     reduce: function ( curr, result ) { },
     initial: { }
   }
)
```

```
SELECT ord_dt, item_sku
FROM orders
WHERE ord_dt > '01/01/2012'
GROUP BY ord_dt, item_sku
```

#### :books: 參考網站：
- [db.collection.group](https://docs.mongodb.com/manual/reference/method/db.collection.group/)

---

#### :books: 參考網站：
- [db.collection.mapReduce](https://docs.mongodb.com/manual/reference/method/db.collection.mapReduce/)

---

`匯入 CSV檔`
`CSV`檔編碼使用`UTF-8`

```
shell> mongoimport --collection <collection> --db <database> --type csv --file <filename> --headerline     
```
---

```
shell> mongo --eval 'db.collection.find().forEach(printjson)'
shell> mongo --username <user> --password <pass> --host <host> --port 28015
shell> mongo script-file.js -u <user> -p

```

`.mongorc.js`

```js
host = db.serverStatus().host;

prompt = function() {
             return db+"@"+host+"$ ";
         }

DBQuery.shellBatchSize = 10;
```

#### :books: 參考網站：
- https://docs.mongodb.com/manual/mongo/

---

```
shell> mongodump --collection collection --db test
shell> mongodump --out /opt/backup/mongodump-2011-10-24
shell> mongodump --host mongodb.example.net --port 27017
shell> mongodump --out /data/backup/
shell> mongodump --host mongodb1.example.net --port 3017 --username user --password pass --out /opt/backup/mongodump-2013-10-24
```

```
shell> mongorestore --port <port number> <path to the backup>
shell> mongorestore dump-2013-10-25/
shell> mongorestore --host mongodb1.example.net --port 3017 --username user --password pass /opt/backup/mongodump-2013-10-24
```

```
shell> mongodump -h 127.0.0.1 -d test -o /opt/backup/mongodump-2017-01-01
shell> mongorestore -h 127.0.0.1 --port 27017 /opt/backup/mongodump-2017-01-01
shell> mongorestore -h 127.0.0.1 --port 27017 --drop /opt/backup/mongodump-2017-01-01      
```

```sh
ssh-keygen
ssh-copy-id 155.94.159.51
```

```sh
ssh -NCf 155.94.159.51 -L 3017:localhost:27017

today=`date "+%Y-%m-%d"`
mongodump --host 127.0.0.1 --port 3017 --out /opt/backup/mongodump-${today}
mongorestore -h 127.0.0.1 --port 27017 --drop /opt/backup/mongodump-${today}
```

```js
use admin
db.runCommand( {fsync: 1, lock: true} )
db.currentOp()
db.fsyncUnlock();
```

#### :books: 參考網站：
- [db.runCommand](https://docs.mongodb.com/v3.2/reference/method/db.runCommand/)
- [use-database-commands](https://docs.mongodb.com/manual/tutorial/use-database-commands/)
- [fsync](https://docs.mongodb.com/manual/reference/command/fsync/)
- [db.currentOp](https://docs.mongodb.com/manual/reference/method/db.currentOp/)
- https://docs.mongodb.com/manual/reference/program/mongorestore/
- https://docs.mongodb.com/v3.2/reference/program/mongorestore/

---

#### :books: 參考網站：
- [install-mongodb-on-ubuntu](http://docs.mongodb.org/manual/tutorial/install-mongodb-on-ubuntu/)
- [mongo](http://docs.mongodb.org/manual/reference/program/mongo/)
- [mongoimport/](http://docs.mongodb.org/manual/reference/program/mongoimport/)
- [mongodump](http://docs.mongodb.org/manual/reference/program/mongodump/)
- [NoSQL資料庫MongoDB 2.6出爐：史上最大版本發表](http://www.ithome.com.tw/news/86559)
- [趨勢科技導入MongoDB 追蹤管理全球10萬個行動裝置](http://www.ithome.com.tw/tech/87418)
- [Getting Started](http://mongoosejs.com/docs/)
- [http://mongoosejs.com/docs/api.html](http://mongoosejs.com/docs/api.html)
- [東方航空用MongoDB挑戰1天10億次網站查詢](http://www.ithome.com.tw/news/98087)
- https://docs.mongodb.com/v3.2/tutorial/configure-linux-iptables-firewall/

---

```js
db.runCommand( { serverStatus: 1 } )
db.serverStatus()
```

**mongostat - MongoDB Use Statistics**
**mongotop - MongoDB Activity Monitor**

```
shell> mongostat
shell> mongostat --port 27017
shell> mongotop   
```

#### :books: 參考網站：
- [serverStatus](https://docs.mongodb.com/manual/reference/command/serverStatus/)

---

```js
use admin
db.createUser(
  {
    user: "myUserAdmin",
    pwd: "abc123",
    roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
  }
)

use admin
db.auth("myUserAdmin", "abc123" )

use test
db.createUser(
  {
    user: "myTester",
    pwd: "xyz123",
    roles: [ { role: "readWrite", db: "test" },
             { role: "read", db: "reporting" } ]
  }
)

db.createUser({
  user: "myTester",
  pwd: "xyz123",
  roles: ["readWrite", "dbAdmin"]
})

use test
db.auth("myTester", "xyz123" )

use products
db.changeUserPassword("accountUser", "SOh3TbYhx8ypJPxmt1oOfL")
```

```
Successfully added user: {
	"user" : "myUserAdmin",
	"roles" : [
		{
			"role" : "userAdminAnyDatabase",
			"db" : "admin"
		}
	]
}
```

```
readAnyDatabase
readWriteAnyDatabase
userAdminAnyDatabase
dbAdminAnyDatabase
root

dbAdmin
dbOwner
userAdmin

readWrite

clusterAdmin
clusterManager
clusterMonitor
hostManager
```

```
shell> mongo --port 27017 -u "myUserAdmin" -p "abc123" --authenticationDatabase "admin"
shell> mongo --port 27017 -u "myTester" -p "xyz123" --authenticationDatabase "test"
```

```js
use products
db.dropAllUsers()
db.dropUser("myTester")
```

```
db.getUsers()
```

#### :books: 參考網站：

- [Enable Auth](https://docs.mongodb.com/manual/tutorial/enable-authentication/)
- https://docs.mongodb.com/manual/reference/built-in-roles/
- [db.createUser](https://docs.mongodb.com/manual/reference/method/db.createUser/)
- [db.dropUser](https://docs.mongodb.com/manual/reference/method/db.dropUser/)
- [db.dropAllUsers](https://docs.mongodb.com/manual/reference/method/db.dropAllUsers/)
- [db.changeUserPassword](https://docs.mongodb.com/manual/reference/method/db.changeUserPassword/)
- [db.grantRolesToUser](https://docs.mongodb.com/manual/reference/method/db.grantRolesToUser/)

---

#### :books: 參考網站：
- https://www.mongodb.org/dl/win32

---

```
sku
description
subject
author
results
product
score
name
phone
city
status
```

```json
{
  "name": "Anne",
  "phone": "+1 555 123 456",
  "city": "London",
  "status": "Complete"
}

{
  "name": "Ivan",
  "city": "Vancouver"
}

{
  "user": "myTester",
  "pwd": "xyz123"
}
```

---

mongoexport

- --host <hostname><:port>, -h <hostname><:port>
- --port <port>
- --username <username>, -u <username>
- --password <password>, -p <password>
- --db <database>, -d <database>
- --collection <collection>, -c <collection>
- --fields <field1[,field2]>, -f <field1[,field2]>
- --query <JSON>, -q <JSON>
- --out <file>, -o <file>
- --pretty


```
use test
db.traffic.insert( { _id: 1, volume: NumberLong('2980000'), date: new Date() } )
```

Export in JSON Format

```
shell> mongoexport --db test --collection traffic --out traffic.json
shell> mongoexport -d test -c records -q '{ a: { $gte: 3 } }' --out exportdir/myRecords.json
```

Export in CSV Format

```
shell> mongoexport --db users --collection contacts --type=csv --fields name,address --out /opt/backups/contacts.csv
```

```json
{ "_id" : 1, "volume" : { "$numberLong" : "2980000" }, "date" : { "$date" : "2014-03-13T13:47:42.483-0400" } }
```

#### :books: 參考網站：
- https://docs.mongodb.com/manual/reference/program/mongoexport/

---


```
shell> mongoimport --db users --collection contacts --file contacts.json
shell> mongoimport -c people -d example --mode upsert --file people-20160927.json
mongoimport --host mongodb1.example.net --port 37017 --username user --password "pass" --collection contacts --db marketing --file /opt/backups/mdb1-examplenet.json
```

CSV Import

General CSV Import

```
shell> mongoimport --db users --collection contacts --type csv --headerline --file /opt/backups/contacts.csv
shell> mongoimport --db users --type csv --headerline --file /opt/backups/contacts.csv
```

#### :books: 參考網站：
- https://docs.mongodb.com/manual/reference/program/mongoimport/












