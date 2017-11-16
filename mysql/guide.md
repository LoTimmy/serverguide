<img src="https://raw.githubusercontent.com/docker-library/docs/master/mysql/logo.png" alt="ansible" width=100>

### 在 `Ubuntu` 14.04 LTS 上建置 `MySQL`

### 安裝 {#installing}

安裝作業系統及`MySQL`相關套件。
因本文主要介紹`MySQL`如何安裝及設定，作業系統方面就不再詳述。

``` 
shell> lsb_release -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 14.04 LTS
Release:	14.04
Codename:	trusty
```

```
shell> MYSQL_PASSWORD=MYSQL_PASSWORD
shell> echo "mysql-server-5.5 mysql-server/root_password password ${MYSQL_PASSWORD}
mysql-server-5.5 mysql-server/root_password seen true
mysql-server-5.5 mysql-server/root_password_again password ${MYSQL_PASSWORD}
mysql-server-5.5 mysql-server/root_password_again seen true
" | debconf-set-selections
shell> DEBIAN_FRONTEND=noninteractive apt-get install -y --force-yes mysql-server
```

---

<img src="http://i.imgur.com/HS5iAz1.png" alt="ansible" width=40>

### 安裝 Percona Server 5.7 {#percona-server-server-5.7}
```
shell> sudo apt-key adv --keyserver keys.gnupg.net --recv-keys 8507EFA5
shell> echo "deb http://repo.percona.com/apt "$(lsb_release -sc)" main" | sudo tee /etc/apt/sources.list.d/percona.list
shell> sudo apt-get update
shell> sudo apt-get install percona-server-server-5.6
shell> sudo apt-get install percona-server-server-5.7

shell> mysql -e "CREATE FUNCTION fnv1a_64 RETURNS INTEGER SONAME 'libfnv1a_udf.so'"
shell> mysql -e "CREATE FUNCTION fnv_64 RETURNS INTEGER SONAME 'libfnv_udf.so'"
shell> mysql -e "CREATE FUNCTION murmur_hash RETURNS INTEGER SONAME 'libmurmur_udf.so'"
```

`/etc/mysql/percona-server.conf.d/mysqld.cnf`
```
bind-address = 127.0.0.1
```

#### :books: 參考網站： 
- [percona-server-server-5.7](https://www.percona.com/doc/percona-server/5.7/installation/apt_repo.html)
- [udf_percona_toolkit](http://www.percona.com/doc/percona-server/5.7/management/udf_percona_toolkit.html)

<img src="http://i.imgur.com/wtp11Uj.png" alt="ansible" width=40>

```
shell> sudo apt-get install software-properties-common
shell> sudo apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0xcbcb082a1bb943db
shell> sudo add-apt-repository 'deb [arch=amd64,i386] http://nyc2.mirrors.digitalocean.com/mariadb/repo/10.1/ubuntu trusty main'

shell> sudo apt-get update
shell> sudo apt-get install mariadb-server
shell> sudo apt-get install mariadb-connect-engine-10.1
```
#### :books: 參考網站： 
- [Installing Percona Server on Debian and Ubuntu](https://www.percona.com/doc/percona-server/5.6/installation/apt_repo.html)
- [mariadb](http://downloads.mariadb.org/mariadb/repositories/)
- [Adding the MariaDB YUM Repository](https://mariadb.com/kb/en/mariadb/yum/)
- [installing-and-using-mariadb-via-docker](https://mariadb.com/kb/en/mariadb/installing-and-using-mariadb-via-docker/)
- [installing-mariadb-deb-files](https://mariadb.com/kb/en/mariadb/installing-mariadb-deb-files/)



### 安裝 MySQL 5.6

```
shell> apt-get install mysql-server-5.7
```


### 安裝 MySQL 5.6

```
shell> apt-get install mysql-server-5.6
shell> netstat -tap | grep mysql

shell> sudo service mysql status
shell> sudo service mysql stop
shell> sudo service mysql start

shell> vim /etc/mysql/my.cnf
```
註解掉「`bind-address`」
```ini
[mysqld]
...
bind-address            = 127.0.0.1
#bind-address            = 127.0.0.1
bind-address = 10.0.0.11
```

```ini
[mysqld]
...
default-storage-engine = innodb
innodb_file_per_table
collation-server = utf8_general_ci
init-connect = 'SET NAMES utf8'
character-set-server = utf8
```

```
shell> dpkg-reconfigure mysql-server-5.6
shell> mysql -p
```

```sql
SELECT VERSION();
```

```
+-------------------------+
| version()               |
+-------------------------+
| 5.6.17-0ubuntu0.14.04.1 |
+-------------------------+
1 row in set (0.00 sec)
```

```sql
SHOW VARIABLES LIKE "%version%";
SHOW STATUS;
```

---
`create-user`

```sql
CREATE USER 'jeffrey'@'localhost' IDENTIFIED BY 'mypass';
SET PASSWORD FOR 'jeffrey'@'localhost' = PASSWORD('mypass');
GRANT ALL ON db1.* TO 'jeffrey'@'localhost';
GRANT SELECT ON db2.invoice TO 'jeffrey'@'localhost';
GRANT USAGE ON *.* TO 'jeffrey'@'localhost' WITH MAX_QUERIES_PER_HOUR 90;

CREATE USER 'root'@'%' IDENTIFIED BY 'mypass';
GRANT ALL ON *.* TO 'root'@'%';
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;

```

```sql
SHOW GRANTS FOR 'root'@'localhost';
SHOW GRANTS;
SHOW GRANTS FOR CURRENT_USER;
SHOW GRANTS FOR CURRENT_USER();

```

#### :books: 參考網站：
- [create-user](https://dev.mysql.com/doc/refman/5.7/en/create-user.html)
- [show-grants](http://dev.mysql.com/doc/refman/5.7/en/show-grants.html)

---
<a name="storage-engines"></a>

### 認識MySQL資料庫引擎 {#storage-engines}

- `MySQL`資料庫軟體有一個與其他資料庫軟體不同的特色，在於它提供了不同類型的資料庫引擎，以供使用者根據不同的需求來選擇，藉此提高資料庫應用的效能。
- 如果想知道`MySQL`資料庫支援那些資料庫引擎，可在登入至`MySQL`資料庫後，以`SHOW ENGINES;`指令來查詢所使用的`MySQL`資料庫能夠支援那些資料庫引擎，其中最常見的預設資料庫引擎是`MyISAM`。 
- 在`MySQL 5.5`之前的版本，當使用者新建資料庫時，預設使用的資料庫引擎為`MyISAM`，而之後則更改為`InnoDB`。

`MyISAM` 
`MyISAM`是早期`MySQL`所支援的資料庫引擎之一，其優勢在於連結執行速度快，但此種資料庫引擎並不支援`交易`(`Transaction`)功能，所謂的交易指的是資料庫確保一筆交易可完整執行的機制。 
假如一筆交易可能需要多次的資料庫動作（如新增或更新）才能完成，交易機制就需要確保當所有的資料庫動作均成功執行時才以`提交`(`Commit`)的方式來完成此項交易。 
反之，如果交易在執行途中發生問題，以致於無法完成交易，即可回復到交易之前的狀態(`Rollback`)用來回復為執行此筆交易之前所執行的動作，如更新(`Update`)、刪除(`Delete`)、新增(`Insert`)等資料庫動作。 
此種資料庫引擎主要是應用在對於無需交易要求或以查詢(`Select`)、新增(`Insert`)為主的應用，這時就可以選用此種類型。 
`MyISAM`資料表在建立後會在磁碟上新增三種類型的檔案，相關類型說明如下所述：
`.frm`：儲存資料庫表格的定義
`.MYD`：儲存資料庫表格內的記錄資料
`.MYI`：儲存資料庫表格內`索引`(`Index`)的資料 

`InnoDB`
`InnoDB`資料庫引擎提供`交易`(`Transaction`)安全的機制，可在交易無法完全執行成功時，提供`回復`(`Rollback`)功能來回復之前所執行的資料庫動作。此種的資料庫類型通常運用在對於交易的完整性有嚴格要求的應用上。 

`Memory` 
就純粹以速度來比較，記憶體的運算速度是遠超過以磁碟來運算的速度，因此`MySQL`資料庫特別提供利用`記憶體`(`Memory`)的容量來儲存資料庫表格(`Table`)的相關資料。 
使用系統記憶體來當成資料庫表格的儲存空間，最大的好處是運算速度快，但由於記憶體揮發的特性，一旦系統關機，所有儲存在記憶體的資料庫表格紀錄也會隨之被抹除。而使用記憶體來儲存，另外還有一個很大的缺點，為其可儲存的容量遠小於以磁碟來儲存。 

> `TokuDB`是一種**支援交易功能且具有高效能的插入(`Insert`)功能及高資料壓縮比的資料庫引擎**，可與`MySQL`資料庫結合，提供特定的應用，通常**適用於需要經常大量寫入的應用上**。 

> 「工欲善其事，必先利其器」，對於資料庫的運用也是如此。以`MySQL`為例，它提供了不同的資料庫引擎來滿足不同的應用，如果**需要嚴謹之保證交易成功的機制，即可選用`InnoDB`型態**。但如果**要求的是快速的運算能力且預期資料量並不大的話，也可考慮使用`Memory`型態**。而`TokuDB`型態，就適用於有大量的插入要求及有效縮小資料庫容量的需求。 

<img src="http://i.imgur.com/GKHGeWI.png" alt="ansible" width=400>

```sql
INSTALL SONAME 'ha_connect';
SHOW ENGINES;
```

```
+--------------------+---------+-----------------------+--------------+------+------------+
| Engine             | Support | Comment                                                                                          | Transactions | XA   | Savepoints |
+--------------------+---------+-----------------------+--------------+------+------------+
| CSV                | YES     | CSV storage engine                                                                               | NO           | NO   | NO         |
| MRG_MyISAM         | YES     | Collection of identical MyISAM tables                                                            | NO           | NO   | NO         |
| MyISAM             | YES     | MyISAM storage engine                                                                            | NO           | NO   | NO         |
| SEQUENCE           | YES     | Generated tables filled with sequential values                                                   | YES          | NO   | YES        |
| CONNECT            | YES     | Management of External Data (SQL/MED), including many file formats                               | NO           | NO   | NO         |
| MEMORY             | YES     | Hash based, stored in memory, useful for temporary tables                                        | NO           | NO   | NO         |
| PERFORMANCE_SCHEMA | YES     | Performance Schema                                                                               | NO           | NO   | NO         |
| Aria               | YES     | Crash-safe tables with MyISAM heritage                                                           | NO           | NO   | NO         |
| InnoDB             | DEFAULT | Percona-XtraDB, Supports transactions, row-level locking, foreign keys and encryption for tables | YES          | YES  | YES        |
+--------------------+---------+-----------------------+--------------+------+------------+
9 rows in set (0.00 sec)
```

```sql
CREATE TABLE t (
) ENGINE=CONNECT DEFAULT CHARSET big5 tabname='Customers' table_type=ODBC block_size=10 
CONNECTION='Driver={SQL Server Native Client 11.0};Server=192.168.31.33;Database=database;UID=UID;PWD=PWD'; 

CREATE TABLE Customer (
  CustomerID VARCHAR(5),
  CompanyName VARCHAR(40),
  ContactName VARCHAR(30),
  ContactTitle VARCHAR(30),
  Address VARCHAR(60),
  City VARCHAR(15),
  Region VARCHAR(15),
  PostalCode VARCHAR(10),
  Country VARCHAR(15),
  Phone VARCHAR(24),
  Fax VARCHAR(24))
ENGINE=connect table_type=ODBC block_size=10
tabname='Customers'
CONNECTION='DSN=MS Access Database;DBQ=C:/ProgramFiles/Microsoft Office/Office/1033/FPNWIND.MDB;';

CREATE TABLE XLCONT
ENGINE=CONNECT table_type=ODBC tabname='CONTACT'
CONNECTION='DSN=Excel Files;DBQ=D:/Ber/Doc/Contact_BP.xls;';

CREATE TABLE essai (
  num INTEGER(4) NOT NULL,
  line CHAR(15) NOT NULL)
ENGINE=CONNECT table_type=MYSQL
CONNECTION='mysql://root@localhost/test/people';

CREATE TABLE xyz (
id int(10) NOT NULL ,
name VARCHAR(50)
) ENGINE=CONNECT DEFAULT CHARSET big5 CONNECTION='Driver={SQL Server Native Client 11.0};Server=localhost;Database=MS_ABC;Uid=sa;Pwd=1234;' table_type=ODBC block_size=10 tabname='TB_XYZ'
```

```sql
CREATE TABLE t (i INT) ENGINE = INNODB;
CREATE TABLE t (i INT) TYPE = MEMORY;
CREATE TABLE t (i INT) ENGINE = MYISAM;
```

```sql
ALTER TABLE t ENGINE = MYISAM;
ALTER TABLE t TYPE = BDB;
```

#### :books: 參考網站：
- [storage-engines](https://dev.mysql.com/doc/refman/5.0/en/storage-engines.html)
- [innodb-configuration](https://dev.mysql.com/doc/refman/5.0/en/innodb-configuration.html)

---

#### The MERGE Storage Engine {#merge-storage-engine}
```sql
CREATE DATABASE test;
USE test;

CREATE TABLE t1 (
   a INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
   message CHAR(20)) ENGINE=MyISAM;
CREATE TABLE t2 (
   a INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
   message CHAR(20)) ENGINE=MyISAM;
INSERT INTO t1 (message) VALUES ('Testing'),('table'),('t1');
INSERT INTO t2 (message) VALUES ('Testing'),('table'),('t2');
CREATE TABLE total (
   a INT NOT NULL AUTO_INCREMENT,
   message CHAR(20), INDEX(a))
   ENGINE=MERGE UNION=(t1,t2) INSERT_METHOD=LAST;

SELECT * FROM total;
```
```
+---+---------+
| a | message |
+---+---------+
| 1 | Testing |
| 2 | table   |
| 3 | t1      |
| 1 | Testing |
| 2 | table   |
| 3 | t2      |
+---+---------+
```

#### :books: 參考網站：
- [merge-storage-engine](https://dev.mysql.com/doc/refman/5.0/en/merge-storage-engine.html)

---

### The FEDERATED Storage Engine {#federated}

![FEDERATED Table Structure](http://dev.mysql.com/doc/refman/5.7/en/images/se-federated-structure.png)

`表格統合` (`FEDERATED`)

```sql
SHOW PLUGINS;
```
```
+---+----------+--------------------+---------+---------+
| Name                       | Status   | Type               | Library | License |
+---+----------+--------------------+---------+---------+
| binlog                     | ACTIVE   | STORAGE ENGINE     | NULL    | GPL     |
| mysql_native_password      | ACTIVE   | AUTHENTICATION     | NULL    | GPL     |
| mysql_old_password         | ACTIVE   | AUTHENTICATION     | NULL    | GPL     |
| sha256_password            | ACTIVE   | AUTHENTICATION     | NULL    | GPL     |
| MEMORY                     | ACTIVE   | STORAGE ENGINE     | NULL    | GPL     |
| MyISAM                     | ACTIVE   | STORAGE ENGINE     | NULL    | GPL     |
| MRG_MYISAM                 | ACTIVE   | STORAGE ENGINE     | NULL    | GPL     |
| CSV                        | ACTIVE   | STORAGE ENGINE     | NULL    | GPL     |
| FEDERATED                  | DISABLED | STORAGE ENGINE     | NULL    | GPL     |
| ARCHIVE                    | ACTIVE   | STORAGE ENGINE     | NULL    | GPL     |
| BLACKHOLE                  | ACTIVE   | STORAGE ENGINE     | NULL    | GPL     |
| InnoDB                     | ACTIVE   | STORAGE ENGINE     | NULL    | GPL     |
| INNODB_TRX                 | ACTIVE   | INFORMATION SCHEMA | NULL    | GPL     |
| INNODB_LOCKS               | ACTIVE   | INFORMATION SCHEMA | NULL    | GPL     |
| INNODB_LOCK_WAITS          | ACTIVE   | INFORMATION SCHEMA | NULL    | GPL     |
| INNODB_CMP                 | ACTIVE   | INFORMATION SCHEMA | NULL    | GPL     |
| INNODB_CMP_RESET           | ACTIVE   | INFORMATION SCHEMA | NULL    | GPL     |
| INNODB_CMPMEM              | ACTIVE   | INFORMATION SCHEMA | NULL    | GPL     |
| INNODB_CMPMEM_RESET        | ACTIVE   | INFORMATION SCHEMA | NULL    | GPL     |
| INNODB_CMP_PER_INDEX       | ACTIVE   | INFORMATION SCHEMA | NULL    | GPL     |
| INNODB_CMP_PER_INDEX_RESET | ACTIVE   | INFORMATION SCHEMA | NULL    | GPL     |
| INNODB_BUFFER_PAGE         | ACTIVE   | INFORMATION SCHEMA | NULL    | GPL     |
| INNODB_BUFFER_PAGE_LRU     | ACTIVE   | INFORMATION SCHEMA | NULL    | GPL     |
| INNODB_BUFFER_POOL_STATS   | ACTIVE   | INFORMATION SCHEMA | NULL    | GPL     |
| INNODB_METRICS             | ACTIVE   | INFORMATION SCHEMA | NULL    | GPL     |
| INNODB_FT_DEFAULT_STOPWORD | ACTIVE   | INFORMATION SCHEMA | NULL    | GPL     |
| INNODB_FT_DELETED          | ACTIVE   | INFORMATION SCHEMA | NULL    | GPL     |
| INNODB_FT_BEING_DELETED    | ACTIVE   | INFORMATION SCHEMA | NULL    | GPL     |
| INNODB_FT_CONFIG           | ACTIVE   | INFORMATION SCHEMA | NULL    | GPL     |
| INNODB_FT_INDEX_CACHE      | ACTIVE   | INFORMATION SCHEMA | NULL    | GPL     |
| INNODB_FT_INDEX_TABLE      | ACTIVE   | INFORMATION SCHEMA | NULL    | GPL     |
| INNODB_SYS_TABLES          | ACTIVE   | INFORMATION SCHEMA | NULL    | GPL     |
| INNODB_SYS_TABLESTATS      | ACTIVE   | INFORMATION SCHEMA | NULL    | GPL     |
| INNODB_SYS_INDEXES         | ACTIVE   | INFORMATION SCHEMA | NULL    | GPL     |
| INNODB_SYS_COLUMNS         | ACTIVE   | INFORMATION SCHEMA | NULL    | GPL     |
| INNODB_SYS_FIELDS          | ACTIVE   | INFORMATION SCHEMA | NULL    | GPL     |
| INNODB_SYS_FOREIGN         | ACTIVE   | INFORMATION SCHEMA | NULL    | GPL     |
| INNODB_SYS_FOREIGN_COLS    | ACTIVE   | INFORMATION SCHEMA | NULL    | GPL     |
| INNODB_SYS_TABLESPACES     | ACTIVE   | INFORMATION SCHEMA | NULL    | GPL     |
| INNODB_SYS_DATAFILES       | ACTIVE   | INFORMATION SCHEMA | NULL    | GPL     |
| PERFORMANCE_SCHEMA         | ACTIVE   | STORAGE ENGINE     | NULL    | GPL     |
| partition                  | ACTIVE   | STORAGE ENGINE     | NULL    | GPL     |
+---+----------+--------------------+---------+---------+
```

```
shell> vim /etc/mysql/my.cnf
```
```ini
[mysqld]
federated=ON
archive=OFF
```

```sql
CREATE TABLE test_table (
    id     INT(20) NOT NULL AUTO_INCREMENT,
    name   VARCHAR(32) NOT NULL DEFAULT '',
    other  INT(20) NOT NULL DEFAULT '0',
    PRIMARY KEY  (id),
    INDEX name (name),
    INDEX other_key (other)
)
ENGINE=MyISAM
DEFAULT CHARSET=latin1;
```

```sql
CREATE TABLE federated_table (
    id     INT(20) NOT NULL AUTO_INCREMENT,
    name   VARCHAR(32) NOT NULL DEFAULT '',
    other  INT(20) NOT NULL DEFAULT '0',
    PRIMARY KEY  (id),
    INDEX name (name),
    INDEX other_key (other)
)
ENGINE=FEDERATED
DEFAULT CHARSET=latin1
CONNECTION='mysql://fed_user@remote_host:9306/federated/test_table';
```

```sql
CONNECTION='mysql://username:password@hostname:port/database/tablename'
CONNECTION='mysql://username@hostname/database/tablename'
CONNECTION='mysql://username:password@hostname/database/tablename'
```

```sql
CREATE SERVER fedlink
FOREIGN DATA WRAPPER mysql
OPTIONS (USER 'fed_user', HOST 'remote_host', PORT 9306, DATABASE 'federated');
```
```sql
CREATE TABLE test_table (
    id     INT(20) NOT NULL AUTO_INCREMENT,
    name   VARCHAR(32) NOT NULL DEFAULT '',
    other  INT(20) NOT NULL DEFAULT '0',
    PRIMARY KEY  (id),
    INDEX name (name),
    INDEX other_key (other)
)
ENGINE=FEDERATED
DEFAULT CHARSET=latin1
CONNECTION='fedlink/test_table';
```

----------

`create-table`

```sql
CREATE TABLE new_tbl LIKE orig_tbl;
```

```sql
CREATE TABLE new_tbl SELECT * FROM orig_tbl;
```

```sql
CREATE TABLE t1 (t TIME(3), dt DATETIME(6));
INSERT INTO t1 VALUES (NOW(), CURRENT_TIMESTAMP);
SELECT * FROM  t1;
```

```sql
CREATE TABLE t
(
    c1 VARCHAR(20) CHARACTER SET utf8,
    c2 TEXT CHARACTER SET latin1 COLLATE latin1_general_cs
);

CREATE TABLE t
(
  c1 VARBINARY(10),
  c2 BLOB,
  c3 ENUM('a','b','c') CHARACTER SET binary
);

salary DECIMAL(5,2)

CREATE TABLE t (qty INT, price INT);

CREATE TABLE t1 (
  ts TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  dt DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

```sql
CREATE TEMPORARY TABLE IF NOT EXISTS t (i INT) ENGINE = INNODB;
```

#### :books: 參考網站：
- [create-table](http://dev.mysql.com/doc/refman/5.1/en/create-table.html)
- [integer-types](http://dev.mysql.com/doc/refman/5.7/en/integer-types.html)

---

```sql
CREATE TABLE t1 (jdoc JSON);
INSERT INTO t1 VALUES('{"key1": "value1", "key2": "value2"}');

SELECT JSON_TYPE('["a", "b", 1]');
SELECT JSON_TYPE('"hello"');

SELECT JSON_ARRAY('a', 1, NOW());

SELECT JSON_OBJECT('key1', 1, 'key2', 'abc');

SELECT JSON_MERGE('["a", 1]', '{"key": "value"}');

SET @j = JSON_OBJECT('key', 'value');
SELECT @j;
SELECT CHARSET(@j), COLLATION(@j);

SELECT JSON_MERGE('[1, 2]', '["a", "b"]', '[true, false]');
SELECT JSON_MERGE('{"a": 1, "b": 2}', '{"c": 3, "a": 4}');

SELECT JSON_EXTRACT('{"id": 14, "name": "Aztalan"}', '$.name');
SELECT JSON_EXTRACT('{"a": 1, "b": 2, "c": [3, 4, 5]}', '$.*');
SELECT JSON_EXTRACT('{"a": 1, "b": 2, "c": [3, 4, 5]}', '$.c[*]');
SELECT JSON_EXTRACT('{"a": {"b": 1}, "c": {"b": 2}}', '$**.b');

SET @j = '"abc"';
SELECT @j, JSON_UNQUOTE(@j);
```

```sql
CREATE TABLE jemp (
     c JSON,
     g INT GENERATED ALWAYS AS (JSON_EXTRACT(c, '$.id')),
     INDEX i (g)
);     

INSERT INTO jemp (c) VALUES
  ('{"id": "1", "name": "Fred"}'), ('{"id": "2", "name": "Wilma"}'),
  ('{"id": "3", "name": "Barney"}'), ('{"id": "4", "name": "Betty"}');

ALTER TABLE jemp ADD COLUMN `name` VARCHAR(30) GENERATED ALWAYS AS (JSON_UNQUOTE(JSON_EXTRACT(c, '$.name'))) VIRTUAL;
ALTER TABLE jemp ADD KEY `name` (`name`);

SELECT JSON_UNQUOTE(JSON_EXTRACT(c, '$.name')) AS name
     FROM jemp WHERE g > 2;
```

#### :books: 參考網站：
- [json](https://dev.mysql.com/doc/refman/5.7/en/json.html)
- [json-modification-functions](https://dev.mysql.com/doc/refman/5.7/en/json-modification-functions.html)

---

```sql
USE TEST;

CREATE TABLE table1 (
     a INT NOT NULL,
     b VARCHAR(32),
     c INT AS (a mod 10) VIRTUAL,
     d VARCHAR(5) AS (left(b,5)) VIRTUAL);

INSERT INTO table1 VALUES (1, 'some text',DEFAULT,DEFAULT);
```

#### :books: 參考網站：
- [virtual-computed-columns](https://mariadb.com/kb/en/mariadb/virtual-computed-columns/)
- [data-type-defaults](https://dev.mysql.com/doc/refman/5.7/en/data-type-defaults.html)

---

`trigger-syntax`

```sql
CREATE TABLE account (acct_num INT, amount DECIMAL(10,2));
CREATE TABLE account_log (ts TIMESTAMP DEFAULT CURRENT_TIMESTAMP, old_acct_num INT, old_amount DECIMAL(10,2), new_acct_num INT, new_amount DECIMAL(10,2));

DELIMITER //
CREATE TRIGGER ins_account_log AFTER INSERT ON account FOR EACH ROW
BEGIN
  INSERT INTO account_log SET new_acct_num=NEW.acct_num, new_amount=NEW.amount ;
END//
DELIMITER ;

DELIMITER //
CREATE TRIGGER upd_account_log AFTER UPDATE ON account FOR EACH ROW
BEGIN
  INSERT INTO account_log SET old_acct_num=OLD.acct_num, old_amount=OLD.amount, new_acct_num=NEW.acct_num, new_amount=NEW.amount;
END//
DELIMITER ;

DELIMITER //
CREATE TRIGGER del_account_log AFTER DELETE ON account FOR EACH ROW
BEGIN
  INSERT INTO account_log SET old_acct_num=OLD.acct_num, old_amount=OLD.amount;
END//
DELIMITER ;

INSERT INTO account VALUES(137,14.98),(141,1937.50),(97,-100.00);

UPDATE account SET amount=amount*2  WHERE acct_num=137;
UPDATE account SET amount=amount*3  WHERE acct_num=141;

DELETE  FROM account WHERE acct_num=97;

SELECT * FROM account;
SELECT * FROM account_log;
```

```sql
CREATE TABLE account (acct_num INT, amount DECIMAL(10,2));

CREATE TRIGGER ins_sum BEFORE INSERT ON account FOR EACH ROW SET @sum = @sum + NEW.amount;

SET @sum = 0;
INSERT INTO account VALUES(137,14.98),(141,1937.50),(97,-100.00);
SELECT @sum AS 'Total amount inserted';

DROP TRIGGER test.ins_sum;
```

```sql
DELIMITER //
CREATE TRIGGER upd_check BEFORE UPDATE ON account
FOR EACH ROW
BEGIN
    IF NEW.amount < 0 THEN
    SET NEW.amount = 0;
    ELSEIF NEW.amount > 100 THEN
    SET NEW.amount = 100;
    END IF;
END;//
DELIMITER ;
```

#### :books: 參考網站：
- [trigger-syntax](https://dev.mysql.com/doc/refman/5.7/en/trigger-syntax.html)

---

`InnoDB File-Per-Table Mode` 
```
shell> vim /etc/mysql/my.cnf
```
```ini
[mysqld]
innodb_file_per_table
```

```sql
CREATE TABLE customers (a INT, b CHAR (20), INDEX (a)) ENGINE=InnoDB;
```

`InnoDB Performance Tuning Tips`
```ini
[mysqld]
innodb_flush_log_at_trx_commit = 2
innodb_log_file_size
innodb_log_buffer_size
```
---

##### 疑難排解 (Troubleshooting)

- 出現錯誤訊息
```
[Warning] Using unique option prefix key_buffer instead of key_buffer_size is
 deprecated and will be removed in a future release. Please use the full name instead.
```
解決方法
```
shell> vim /etc/mysql/my.cnf
```
```ini
#key_buffer		= 16M
key_buffer_size = 16M
```

- 出現錯誤訊息
```
[Warning] TIMESTAMP with implicit DEFAULT value is deprecated. Please use 
--explicit_defaults_for_timestamp server option (see documentation for more details).
```
解決方法
```
shell> vim /etc/mysql/my.cnf
```
```ini
explicit_defaults_for_timestamp = 1
```

- 出現錯誤訊息
```
[Warning] Using unique option prefix myisam-recover instead of myisam-recover-options is deprecated and will be remo
ved in a future release. Please use the full name instead.
```
解決方法
```
shell> vim /etc/mysql/my.cnf
```
```ini
#myisam-recover         = BACKUP
myisam-recover-options = BACKUP
```

- 出現錯誤訊息
```
[Warning] IP address '192.168.81.9' could not be resolved: Name or service not known
```

解決方法
```
shell> vim /etc/mysql/my.cnf
```
```ini
skip-name-resolve
```

---

`insert-on-duplicate`

```sql
INSERT INTO table (a,b,c) VALUES (1,2,3)
  ON DUPLICATE KEY UPDATE c=c+1;

UPDATE table SET c=c+1 WHERE a=1;
```

#### :books: 參考網站：
- [insert-on-duplicate](http://dev.mysql.com/doc/refman/5.0/en/insert-on-duplicate.html)

---

`create-view`

```sql
CREATE TABLE t (qty INT, price INT);
INSERT INTO t VALUES(3, 50);
CREATE VIEW v AS SELECT qty, price, qty*price AS value FROM t;
SELECT * FROM v;
```
```
+------+-------+-------+
| qty  | price | value |
+------+-------+-------+
|    3 |    50 |   150 |
+------+-------+-------+
```

#### :books: 參考網站：
- [create-view](https://dev.mysql.com/doc/refman/5.0/en/create-view.html)

---

### Configuring MySQL for SSL

```sql
SHOW VARIABLES LIKE 'have_ssl';
```
```
+---------------+------------+
| Variable_name | Value      |
+---------------+------------+
| have_ssl      | DISABLED   |
+---------------+------------+
```

###### Create CA certificate
```
shell> openssl genrsa -out ca-key.pem 2048
shell> openssl req -sha1 -new -x509 -nodes -days 7305 \
       -key ca-key.pem -out ca-cert.pem \
       -subj "/C=TW/ST=Taiwan/L=TPE/O=Example Company/OU=MYCA/CN=MYCA" 
shell> openssl x509 -in ca-cert.pem -text -noout
```

###### Create server certificate, remove passphrase, and sign it
```
shell> openssl req -sha1 -newkey rsa:2048 -nodes -days 3653 \
       -keyout mysql-server-key.pem -out mysql-server-req.pem \
       -subj "/C=TW/ST=Taiwan/L=TPE/O=Example Company/OU=MYCA/CN=mysql-server"
shell> openssl x509 -sha1 -req -set_serial 01 -in mysql-server-req.pem \
       -days 3653 -CA ca-cert.pem -CAkey ca-key.pem -out mysql-server-cert.pem
shell> openssl verify -verbose -CAfile ca-cert.pem mysql-server-cert.pem
```
出現錯誤訊息
```
SSL error: Unable to get certificate from '/etc/mysql/mysql-server-key.pem'
[Warning] Failed to setup SSL
[Warning] SSL error: Unable to get certificate
```
解決方法
```
shell> openssl rsa -in mysql-server-key.pem -out mysql-server-key.pem
```

###### Create client certificate, remove passphrase, and sign it
```
shell> openssl req -sha1 -newkey rsa:2048 -nodes -days 3653 \
       -keyout mysql-client-key.pem -out mysql-client-req.pem \
       -subj "/C=TW/ST=Taiwan/L=TPE/O=Example Company/OU=MYCA/CN=mysql-client"
shell> openssl x509 -sha1 -req -set_serial 01 -in mysql-client-req.pem \
       -days 3653 -CA ca-cert.pem -CAkey ca-key.pem -out mysql-client-cert.pem
shell> openssl verify -verbose -CAfile ca-cert.pem mysql-client-cert.pem
shell> openssl rsa -in mysql-client-key.pem -out mysql-client-key.pem
```

```
shell> openssl req -sha1 -newkey rsa:2048 -nodes -days 3653 \
       -keyout jeffrey-key.pem -out jeffrey-req.pem \
       -subj "/C=TW/ST=Taiwan/L=TPE/O=Example Company/OU=MYCA/CN=jeffrey"
shell> openssl x509 -sha1 -req -set_serial 01 -in jeffrey-req.pem \
       -days 3653 -CA ca-cert.pem -CAkey ca-key.pem -out jeffrey-cert.pem
shell> openssl verify -verbose -CAfile ca-cert.pem jeffrey-cert.pem
shell> openssl rsa -in jeffrey-key.pem -out jeffrey-key.pem
```

```
shell> vim /etc/mysql/my.cnf
```

```ini
ssl-ca=/etc/mysql/ca-cert.pem
ssl-cert=/etc/mysql/mysql-server-cert.pem
ssl-key=/etc/mysql/mysql-server-key.pem
```

```
shell> service mysql restart
```

```sql
SHOW VARIABLES LIKE 'have_ssl';
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| have_ssl      | YES   |
+---------------+-------+
```

##### Connecting using SSL Encryption

```
shell> mysql --ssl-ca=ca-cert.pem \
       --ssl-cert=mysql-client-cert.pem \
       --ssl-key=mysql-client-key.pem -p
```
```sql
SHOW STATUS LIKE 'Ssl_cipher';
```
```
+---------------+--------------------+
| Variable_name | Value              |
+---------------+--------------------+
| Ssl_cipher    | DHE-RSA-AES256-SHA |
+---------------+--------------------+
```

```sql
GRANT ALL ON *.* TO 'jeffrey'@'localhost'
  REQUIRE SUBJECT '/C=TW/ST=Taiwan/L=TPE/O=Example Company/OU=MYCA/CN=jeffrey';
```

```sql
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'goodsecret' REQUIRE X509;
GRANT ALL ON *.* TO 'root'@'%' REQUIRE SSL WITH GRANT OPTION;
SHOW GRANTS FOR 'root'@'%';
```

```
shell> vim ~/.my.cnf
```
```ini
[client]
port=3306
ssl-ca=ca-cert.pem
ssl-cert=mysql-client-cert.pem
ssl-key=mysql-client-key.pem
~~~~

![](http://i.imgur.com/kdFs8Bq.jpg)
![](http://i.imgur.com/MLyikpT.jpg)

```php
<?php
$mysqli = mysqli_init();
$mysqli->ssl_set ('mysql-client-key.pem', 'mysql-client-cert.pem',
                  'ca-cert.pem', NULL, NULL);
$link = mysqli_real_connect($mysqli, 'localhost', 'jeffrey',
                 'mypass', 'my_db', 3306, NULL, MYSQLI_CLIENT_SSL);
$query = "SHOW STATUS LIKE 'Ssl_cipher'";

if (!$link) {
  die('Connect Error (' . mysqli_connect_errno() . ') ' . mysqli_connect_error());
} else {
  $result = $mysqli->query($query);
  /* fetch object array */
  while ($row = $result->fetch_row()) {
    printf ("%s (%s)\n", $row[0], $row[1]);
  }

  /* free result set */
  $result->close();
}

/* close connection */
$mysqli->close();
?>
```

```php
<?php
$dsn = 'mysql:dbname=my_db;host=127.0.0.1';
$user = 'jeffrey';
$password = 'mypass';
$dbh = new PDO($dsn, $user, $password, array(
    PDO::MYSQL_ATTR_SSL_KEY => 'mysql-client-key.pem',
    PDO::MYSQL_ATTR_SSL_CERT => 'mysql-client-cert.pem',
    PDO::MYSQL_ATTR_SSL_CA => 'ca-cert.pem'
  )
);

$sql = "SHOW STATUS LIKE 'Ssl_cipher'";

$sth = $dbh->query($sql);
$result = $sth->fetch(PDO::FETCH_ASSOC);
print_r($result);

foreach ($dbh->query($sql) as $row) {
  print $row['Value'] . "\t";
  print $row['Variable_name'] . "\n";
}
?>
```
---

```sql
SELECT USER(), CURRENT_USER();
```
```
+--------------------------+---------------------+
| USER()                   | CURRENT_USER()      |
+--------------------------+---------------------+
| user2@remote.example.com | user2@%.example.com |
+--------------------------+---------------------+
```
---

```sql
SELECT @@innodb_fast_shutdown;
SET GLOBAL innodb_fast_shutdown=0;
```

---
### How to Reset the Root Password

```
shell> service mysql stop
shell> mysqld_safe --skip-grant-tables &
shell> mysql
```

```sql
UPDATE mysql.user SET Password=PASSWORD('MyNewPass') WHERE User='root';
FLUSH PRIVILEGES;
```

```
shell> service mysql restart
```

---

```sql
DELETE FROM tbl_name WHERE lastUpdatedTime < 1423706065 
```

```sql
DELETE FROM tbl_name WHERE lastUpdatedTime < 1423706065 ORDER BY uniqueId LIMIT 0,10000
```

---

<a name="partitioning"></a>

###  Partitioning

```sql
SHOW VARIABLES LIKE '%partition%';
```
```
+-------------------+-------+
| Variable_name     | Value |
+-------------------+-------+
| have_partitioning | YES   |
+-------------------+-------+
1 row in set (0.00 sec)
```

```sql
CREATE TABLE members (
    firstname VARCHAR(25) NOT NULL,
    lastname VARCHAR(25) NOT NULL,
    username VARCHAR(16) NOT NULL,
    email VARCHAR(35),
    joined DATE NOT NULL
)
PARTITION BY KEY(joined)
PARTITIONS 6;
```

```sql
CREATE TABLE members (
    firstname VARCHAR(25) NOT NULL,
    lastname VARCHAR(25) NOT NULL,
    username VARCHAR(16) NOT NULL,
    email VARCHAR(35),
    joined DATE NOT NULL
)
PARTITION BY RANGE( YEAR(joined) ) (
    PARTITION p0 VALUES LESS THAN (1960),
    PARTITION p1 VALUES LESS THAN (1970),
    PARTITION p2 VALUES LESS THAN (1980),
    PARTITION p3 VALUES LESS THAN (1990),
    PARTITION p4 VALUES LESS THAN MAXVALUE
);
```

#### :books: 參考網站：
- [partitioning](https://dev.mysql.com/doc/refman/5.1/en/partitioning.html)
- [partitioning-types](https://dev.mysql.com/doc/refman/5.1/en/partitioning-types.html)

---

```sql
SHOW PLUGINS;
```
```
+------------+----------+----------------+---------+---------+
| Name       | Status   | Type           | Library | License |
+------------+----------+----------------+---------+---------+
| binlog     | ACTIVE   | STORAGE ENGINE | NULL    | GPL     |
| partition  | ACTIVE   | STORAGE ENGINE | NULL    | GPL     |
| ARCHIVE    | ACTIVE   | STORAGE ENGINE | NULL    | GPL     |
| BLACKHOLE  | ACTIVE   | STORAGE ENGINE | NULL    | GPL     |
| CSV        | ACTIVE   | STORAGE ENGINE | NULL    | GPL     |
| FEDERATED  | DISABLED | STORAGE ENGINE | NULL    | GPL     |
| MEMORY     | ACTIVE   | STORAGE ENGINE | NULL    | GPL     |
| InnoDB     | ACTIVE   | STORAGE ENGINE | NULL    | GPL     |
| MRG_MYISAM | ACTIVE   | STORAGE ENGINE | NULL    | GPL     |
| MyISAM     | ACTIVE   | STORAGE ENGINE | NULL    | GPL     |
| ndbcluster | DISABLED | STORAGE ENGINE | NULL    | GPL     |
+------------+----------+----------------+---------+---------+
11 rows in set (0.00 sec)
```

```sql
SELECT 
    PLUGIN_NAME as Name, 
    PLUGIN_VERSION as Version, 
    PLUGIN_STATUS as Status 
    FROM INFORMATION_SCHEMA.PLUGINS 
    WHERE PLUGIN_TYPE='STORAGE ENGINE';
```

```
+--------------------+---------+--------+
| Name               | Version | Status |
+--------------------+---------+--------+
| binlog             | 1.0     | ACTIVE |
| CSV                | 1.0     | ACTIVE |
| MEMORY             | 1.0     | ACTIVE |
| MRG_MYISAM         | 1.0     | ACTIVE |
| MyISAM             | 1.0     | ACTIVE |
| PERFORMANCE_SCHEMA | 0.1     | ACTIVE |
| BLACKHOLE          | 1.0     | ACTIVE |
| ARCHIVE            | 3.0     | ACTIVE |
| InnoDB             | 5.6     | ACTIVE |
| partition          | 1.0     | ACTIVE |
+--------------------+---------+--------+
10 rows in set (0.00 sec)
```

---

```sql
-- 建立預存程序
DELIMITER //
CREATE PROCEDURE Hello (IN `name` CHAR(20))
BEGIN
  SELECT CONCAT('Hello ', `name`, '!');
END //
DELIMITER ;

-- 執行預存程序
CALL Hello('World');
```
```
Hello World!
```

```sql
-- 建立預存程序
DELIMITER //
CREATE PROCEDURE simpleproc (OUT param1 INT)
BEGIN
  SELECT COUNT(*) INTO param1 FROM t;
END//
DELIMITER ;

-- 執行預存程序
CALL simpleproc(@a);
SELECT @a;
```
```
+------+
| @a   |
+------+
| 3    |
+------+
```

```sql
-- 建立預存程序
DELIMITER //
DROP PROCEDURE IF EXISTS `Hello`//
CREATE PROCEDURE Hello ()
BEGIN
  DECLARE `name` VARCHAR(20);
  SELECT '中文' INTO `name`;
  SELECT `name`;
END//
DELIMITER ;

-- 執行預存程序
CALL Hello(); -- ??
```

```sql
-- 建立預存程序
DELIMITER //
DROP PROCEDURE IF EXISTS `Hello`//
CREATE PROCEDURE Hello ()
BEGIN
  DECLARE `name` VARCHAR(20) CHARACTER SET utf8;
  SELECT '中文' INTO `name`;
  SELECT `name`;
END//
DELIMITER ;

-- 執行預存程序
CALL Hello(); -- 中文
```

```sql
-- 建立預存程序
DELIMITER //
CREATE FUNCTION SimpleCompare(n INT, m INT)
  RETURNS VARCHAR(20)

  BEGIN
    DECLARE s VARCHAR(20);

    IF n > m THEN SET s = '>';
    ELSEIF n = m THEN SET s = '=';
    ELSE SET s = '<';
    END IF;

    SET s = CONCAT(n, ' ', s, ' ', m);

    RETURN s;
  END //
DELIMITER ;
```

#### :books: 參考網站：
- [if](https://dev.mysql.com/doc/refman/5.7/en/if.html)

---

```sql
DROP TABLE IF EXISTS tbl;
CREATE TABLE tbl (created_at DATE, id INT, num INT);

INSERT INTO tbl VALUES 
       ('2015-08-01', 1, 50718),
       ('2015-08-01', 2, 82662),
       ('2015-08-01', 3, 12666),
       ('2015-08-02', 1, 19234),
       ('2015-08-02', 2, 44641),
       ('2015-08-02', 3, 29753),
       ('2015-08-03', 1, 82385),
       ('2015-08-03', 2, 72123),
       ('2015-08-03', 3, 47293);
SELECT * FROM tbl;
```

```
created_at      id     num  
----------  ------  --------
2015-08-01       1     50718
2015-08-01       2     82662
2015-08-01       3     12666
2015-08-02       1     19234
2015-08-02       2     44641
2015-08-02       3     29753
2015-08-03       1     82385
2015-08-03       2     72123
2015-08-03       3     47293
```

```sql
SELECT created_at, 
COALESCE(GROUP_CONCAT(IF(id = 1, num, NULL)), 0) AS '1',
COALESCE(GROUP_CONCAT(IF(id = 2, num, NULL)), 0) AS '2',
COALESCE(GROUP_CONCAT(IF(id = 3, num, NULL)), 0) AS '3' 
FROM tbl GROUP BY created_at;
```

```
created_at  1       2       3       
----------  ------  ------  --------
2015-08-01  50718   82662   12666   
2015-08-02  19234   44641   29753   
2015-08-03  82385   72123   47293   
```
---

#### :books: 參考網站：
- [create-procedure](https://dev.mysql.com/doc/refman/5.0/en/create-procedure.html)

---

```sql
CREATE TABLE MyTable (
  MyDecimalColumn DECIMAL(5,2) COMMENT '',
  MyNumericColumn NUMERIC(10,5) COMMENT '',
  MyNCharColumn NCHAR(15) COMMENT '',
  MyNVarCharColumn NVARCHAR(20) COMMENT '',
  MyBigIntColumn BIGINT COMMENT '',
  MyIntColumn  INT COMMENT '',
  MySmallIntColumn SMALLINT COMMENT '',
  MyTinyIntColumn TINYINT COMMENT ''
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='I''m a table comment!';

INSERT INTO MyTable VALUES (123, 12345.12, N'Test data', N'More test data', 9223372036854775807, 214483647,32767,255);

```

#### :books: 參考網站：
- [資料類型](https://msdn.microsoft.com/zh-tw/library/ms187752.aspx)
- [decimal 和 numeric](https://msdn.microsoft.com/zh-tw/library/ms187746.aspx)
- [nchar 和 nvarchar](https://msdn.microsoft.com/zh-tw/library/ms186939.aspx)
- [int、bigint、smallint 和 tinyint](https://msdn.microsoft.com/zh-tw/library/ms187745.aspx)

---

```sql
LOAD DATA INFILE 'data.txt' INTO TABLE db2.my_table;
```

```
shell> mysql -h remote.example.com \
       -u remoteuser \
       -p password \
       -e "LOAD DATA LOCAL INFILE 'file_name' INTO TABLE tbl_name CHARACTER SET utf8 FIELDS ESCAPED BY '\\\\' TERMINATED BY '\t' LINES TERMINATED BY '\n';"
```

#### :books: 參考網站：

- [load-data](https://dev.mysql.com/doc/refman/5.0/en/load-data.html)
- [freebcp](https://dl.dropboxusercontent.com/u/4276183/docs/commands.html#freebcp)
- [bcp](https://dl.dropboxusercontent.com/u/4276183/docs/sqlserver.html#bcp)

---


```sql
CREATE PROCEDURE sp1 (x VARCHAR(5))
BEGIN
  DECLARE xname VARCHAR(5) DEFAULT 'bob';
  DECLARE newname VARCHAR(5);
  DECLARE xid INT;

  SELECT xname, id INTO newname, xid
    FROM table1 WHERE xname = xname;
  SELECT newname;
END;
```

```sql
CREATE PROCEDURE sp2 (x VARCHAR(5))
BEGIN
  DECLARE xname VARCHAR(5) DEFAULT 'bob';
  DECLARE newname VARCHAR(5);
  DECLARE xid INT;
  DECLARE done TINYINT DEFAULT 0;
  DECLARE cur1 CURSOR FOR SELECT xname, id FROM table1;
  DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = 1;

  OPEN cur1;
  read_loop: LOOP
    FETCH FROM cur1 INTO newname, xid;
    IF done THEN LEAVE read_loop; END IF;
    SELECT newname;
  END LOOP;
  CLOSE cur1;
END;
```

#### :books: 參考網站：
- [local-variable-scope](http://dev.mysql.com/doc/refman/5.6/en/local-variable-scope.html)

---

<img src="http://i.imgur.com/CXYPLS5.png" alt="ansible" width=250>

<img src="http://i.imgur.com/lSBPuR6.png" alt="ansible" width=250>

**`查詢日誌`**
啟用此選項時，由於會記錄大量的查詢紀錄動作，因此可能造成大量的磁碟存取，以至於拖慢系統效率。

```sql
SET GLOBAL log_output = 'FILE';
SET GLOBAL general_log_file = 'host_name.log';
SET GLOBAL general_log = 'ON';
```

```sql
SET GLOBAL general_log = 'OFF';
```

**`慢查詢日誌`**
很多時候，由於不適當的`SQL`指令往往會造成資料庫效能低下，因為大量的資源都用來處理這些不適當的`SQL`指令。 
在這種情況下，可利用啟動「`慢查詢日誌`」記錄功能，來追蹤是否有超出所設定執行時間的`SQL`指令，藉此找出執行時間過長的`SQL`指令。 

- `log_slow_queries`：設定儲存慢查詢日誌的檔案 位置。
- `long_query_time`：設定執行時間的門檻值，一旦超過此門檻值，就會記錄該`SQL`指令的資訊，單位為秒。 

```sql
SET @@GLOBAL.long_query_time = 1;
SET @@GLOBAL.slow_query_log_file = 'host_name.log';
SET @@GLOBAL.slow_query_log = 1;

SHOW GLOBAL VARIABLES LIKE 'long_query_time';
SHOW GLOBAL VARIABLES LIKE 'slow_query_log_file';
SHOW GLOBAL VARIABLES LIKE 'slow_query_log';
```

```sql
SET GLOBAL slow_query_log = 'OFF';
```


#### :books: 參考網站：
- [server-system-variables](https://dev.mysql.com/doc/refman/5.1/en/server-system-variables.html)
- [sysvar_slow_query_log](http://dev.mysql.com/doc/refman/5.6/en/server-system-variables.html#sysvar_slow_query_log)
- [sysvar_long_query_time](http://dev.mysql.com/doc/refman/5.6/en/server-system-variables.html#sysvar_long_query_time)
- [詳解MySQL四種Log機制 以日誌稽核解析追蹤事件](http://www.netadmin.com.tw/article_content.aspx?sn=1601120005)

---

`mysql-config-editor`

```
shell> mysql_config_editor set --login-path=client --host=localhost --user=localuser --password 
shell> mysql --login-path=remote
```

.mylogin.cnf

```
shell> mysql_config_editor print --all
```
```
[client]
user = localuser
password = *****
host = localhost
[remote]
user = remoteuser
password = *****
host = remote.example.com
```

#### :books: 參考網站：
- [mysql-config-editor](http://dev.mysql.com/doc/refman/5.6/en/mysql-config-editor.html)

---

```sql
SELECT * FROM performance_schema.events_statements_summary_by_digest;
```

---

`0~99999`
```sql
(SELECT (t5.i*10000 + t4.i*1000 + t3.i*100 + t2.i*10 + t1.i) mynumber FROM
 (SELECT 0 i UNION SELECT 1 UNION SELECT 2 UNION SELECT 3 UNION SELECT 4 UNION SELECT 5 UNION SELECT 6 UNION SELECT 7 UNION SELECT 8 UNION SELECT 9) t1,
 (SELECT 0 i UNION SELECT 1 UNION SELECT 2 UNION SELECT 3 UNION SELECT 4 UNION SELECT 5 UNION SELECT 6 UNION SELECT 7 UNION SELECT 8 UNION SELECT 9) t2,
 (SELECT 0 i UNION SELECT 1 UNION SELECT 2 UNION SELECT 3 UNION SELECT 4 UNION SELECT 5 UNION SELECT 6 UNION SELECT 7 UNION SELECT 8 UNION SELECT 9) t3,
 (SELECT 0 i UNION SELECT 1 UNION SELECT 2 UNION SELECT 3 UNION SELECT 4 UNION SELECT 5 UNION SELECT 6 UNION SELECT 7 UNION SELECT 8 UNION SELECT 9) t4,
 (SELECT 0 i UNION SELECT 1 UNION SELECT 2 UNION SELECT 3 UNION SELECT 4 UNION SELECT 5 UNION SELECT 6 UNION SELECT 7 UNION SELECT 8 UNION SELECT 9) t5) 
```

```sql
SELECT * FROM (
(SELECT ADDDATE('1970-01-01', t5.i*10000 + t4.i*1000 + t3.i*100 + t2.i*10 + t1.i) `Date` FROM
 (SELECT 0 i UNION SELECT 1 UNION SELECT 2 UNION SELECT 3 UNION SELECT 4 UNION SELECT 5 UNION SELECT 6 UNION SELECT 7 UNION SELECT 8 UNION SELECT 9) t1,
 (SELECT 0 i UNION SELECT 1 UNION SELECT 2 UNION SELECT 3 UNION SELECT 4 UNION SELECT 5 UNION SELECT 6 UNION SELECT 7 UNION SELECT 8 UNION SELECT 9) t2,
 (SELECT 0 i UNION SELECT 1 UNION SELECT 2 UNION SELECT 3 UNION SELECT 4 UNION SELECT 5 UNION SELECT 6 UNION SELECT 7 UNION SELECT 8 UNION SELECT 9) t3,
 (SELECT 0 i UNION SELECT 1 UNION SELECT 2 UNION SELECT 3 UNION SELECT 4 UNION SELECT 5 UNION SELECT 6 UNION SELECT 7 UNION SELECT 8 UNION SELECT 9) t4,
 (SELECT 0 i UNION SELECT 1 UNION SELECT 2 UNION SELECT 3 UNION SELECT 4 UNION SELECT 5 UNION SELECT 6 UNION SELECT 7 UNION SELECT 8 UNION SELECT 9) t5) v )
WHERE `Date` BETWEEN DATE_FORMAT('2015-10-01', '%Y-%m-%d') AND DATE_FORMAT('2015-10-31', '%Y-%m-%d')
```

```sql
DELIMITER //
DROP PROCEDURE IF EXISTS `DateRange`//

CREATE PROCEDURE `DateRange`(IN StartDate CHAR(10),IN EndDate CHAR(10))
BEGIN
  DECLARE v1 INT;
  SELECT DATEDIFF(EndDate, StartDate) INTO v1;

  --Delete the t table if it exists.
  DROP TABLE IF EXISTS t;
  --Create a new table called t.
  CREATE TEMPORARY TABLE IF NOT EXISTS t (d CHAR(10)) ;

  WHILE v1 >= 0 DO
    INSERT INTO t SET d=ADDDATE(StartDate, v1);
    SET v1 = v1 - 1;
  END WHILE;
  SELECT * FROM t ORDER BY d;

END//
DELIMITER ;

CALL DateRange('2015-10-01', '2015-10-31');
```

---
`pvt`

```sql
CREATE TABLE t1 (VendorID INT, Employee VARCHAR(32), Orders INT);

INSERT INTO t1 VALUES (1, 'Emp1', 4);
INSERT INTO t1 VALUES (1, 'Emp2', 3);
INSERT INTO t1 VALUES (1, 'Emp3', 5);
INSERT INTO t1 VALUES (1, 'Emp4', 4);
INSERT INTO t1 VALUES (1, 'Emp5', 4);

INSERT INTO t1 VALUES (2, 'Emp1', 4);
INSERT INTO t1 VALUES (2, 'Emp2', 1);
INSERT INTO t1 VALUES (2, 'Emp3', 5);
INSERT INTO t1 VALUES (2, 'Emp4', 5);
INSERT INTO t1 VALUES (2, 'Emp5', 5);

INSERT INTO t1 VALUES (3, 'Emp1', 4);
INSERT INTO t1 VALUES (3, 'Emp2', 3);
INSERT INTO t1 VALUES (3, 'Emp3', 5);
INSERT INTO t1 VALUES (3, 'Emp4', 4);
INSERT INTO t1 VALUES (3, 'Emp5', 4);

INSERT INTO t1 VALUES (4, 'Emp1', 4);
INSERT INTO t1 VALUES (4, 'Emp2', 2);
INSERT INTO t1 VALUES (4, 'Emp3', 5);
INSERT INTO t1 VALUES (4, 'Emp4', 5);
INSERT INTO t1 VALUES (4, 'Emp5', 4);

INSERT INTO t1 VALUES (5, 'Emp1', 5);
INSERT INTO t1 VALUES (5, 'Emp2', 1);
INSERT INTO t1 VALUES (5, 'Emp3', 5);
INSERT INTO t1 VALUES (5, 'Emp4', 5);
INSERT INTO t1 VALUES (5, 'Emp5', 5);

SELECT * FROM t1;
```

```
VendorID  Employee  Orders  
--------  --------  --------
       1  Emp1             4
       1  Emp2             3
       1  Emp3             5
       1  Emp4             4
       1  Emp5             4
       2  Emp1             4
       2  Emp2             1
       2  Emp3             5
       2  Emp4             5
       2  Emp5             5
       3  Emp1             4
       3  Emp2             3
       3  Emp3             5
       3  Emp4             4
       3  Emp5             4
       4  Emp1             4
       4  Emp2             2
       4  Emp3             5
       4  Emp4             5
       4  Emp5             4
       5  Emp1             5
       5  Emp2             1
       5  Emp3             5
       5  Emp4             5
       5  Emp5             5
       1  Emp1             4
       1  Emp2             3
       1  Emp3             5
       1  Emp4             4
       1  Emp5             4
       2  Emp1             4
       2  Emp2             1
       2  Emp3             5
       2  Emp4             5
       2  Emp5             5
       3  Emp1             4
       3  Emp2             3
       3  Emp3             5
       3  Emp4             4
       3  Emp5             4
       4  Emp1             4
       4  Emp2             2
       4  Emp3             5
       4  Emp4             5
       4  Emp5             4
       5  Emp1             5
       5  Emp2             1
       5  Emp3             5
       5  Emp4             5
       5  Emp5             5
```

```sql
SELECT VendorID, 
COALESCE(GROUP_CONCAT(DISTINCT IF(Employee = 'Emp1', Orders, NULL)), 0) AS 'Emp1',
COALESCE(GROUP_CONCAT(DISTINCT IF(Employee = 'Emp2', Orders, NULL)), 0) AS 'Emp2',
COALESCE(GROUP_CONCAT(DISTINCT IF(Employee = 'Emp3', Orders, NULL)), 0) AS 'Emp3',
COALESCE(GROUP_CONCAT(DISTINCT IF(Employee = 'Emp4', Orders, NULL)), 0) AS 'Emp4',
COALESCE(GROUP_CONCAT(DISTINCT IF(Employee = 'Emp5', Orders, NULL)), 0) AS 'Emp5'
FROM t1 GROUP BY VendorID;

SELECT VendorID, 
MAX(IF(Employee='Emp1',Orders, NULL)) AS 'Emp1',
MAX(IF(Employee='Emp2',Orders, NULL)) AS 'Emp2',
MAX(IF(Employee='Emp3',Orders, NULL)) AS 'Emp3',
MAX(IF(Employee='Emp4',Orders, NULL)) AS 'Emp4',
MAX(IF(Employee='Emp5',Orders, NULL)) AS 'Emp5'
FROM t1 GROUP BY VendorID;
```

```
VendorID    Emp1    Emp2    Emp3    Emp4    Emp5  
--------  ------  ------  ------  ------  --------
       1       4       3       5       4         4
       2       4       1       5       5         5
       3       4       3       5       4         4
       4       4       2       5       5         4
       5       5       1       5       5         5
```

#### :books: 參考網站：
- [使用 PIVOT 和 UNPIVOT](https://technet.microsoft.com/zh-tw/library/ms177410(v=sql.105).aspx)

---

```sql
UPDATE table1
INNER JOIN table2
ON table1.id=table2.id
SET table1.col1 = 1,
    table1.col2 =2
WHERE table1.id > 100;
```

#### :books: 參考網站：
- [join](https://dev.mysql.com/doc/refman/5.7/en/join.html)
- [update](https://dev.mysql.com/doc/refman/5.7/en/update.html)

---

`sql-syntax-prepared-statements`

```sql
PREPARE stmt1 FROM 'SELECT SQRT(POW(?,2) + POW(?,2)) AS hypotenuse';
SET @a = 3;
SET @b = 4;
EXECUTE stmt1 USING @a, @b;
DEALLOCATE PREPARE stmt1;
```

```sql
SET @s = 'SELECT SQRT(POW(?,2) + POW(?,2)) AS hypotenuse';
PREPARE stmt2 FROM @s;
SET @a = 6;
SET @b = 8;
EXECUTE stmt2 USING @a, @b;
DEALLOCATE PREPARE stmt2;
```

```sql
USE test;
CREATE TABLE t1 (a INT NOT NULL);
INSERT INTO t1 VALUES (4), (8), (11), (32), (80);

SET @table = 't1';
SET @s = CONCAT('SELECT * FROM ', @table);

PREPARE stmt3 FROM @s;
EXECUTE stmt3;

DEALLOCATE PREPARE stmt3;
```

#### :books: 參考網站：
- [sql-syntax-prepared-statements](http://dev.mysql.com/doc/refman/5.7/en/sql-syntax-prepared-statements.html)

---

<a name="event_scheduler"></a>

```sql
SELECT @@event_scheduler;

SET GLOBAL event_scheduler = ON;
SET @@global.event_scheduler = ON;
SET GLOBAL event_scheduler = 1;
SET @@global.event_scheduler = 1;


SET GLOBAL event_scheduler = OFF;
SET @@global.event_scheduler = OFF;
SET GLOBAL event_scheduler = 0;
SET @@global.event_scheduler = 0;


SHOW PROCESSLIST;

GRANT EVENT ON myschema.* TO jon@ghidora;
GRANT EVENT ON *.* TO jon@ghidora;


CREATE TABLE mytable (dt DATETIME(6));

CREATE EVENT e_store_ts
    ON SCHEDULE
      EVERY 10 SECOND
    DO
      INSERT INTO myschema.mytable VALUES (NOW());

CREATE EVENT e_insert
    ON SCHEDULE
      EVERY 7 SECOND
    DO
      INSERT INTO myschema.mytable;

-- 每天 00:05 執行
DROP EVENT IF EXISTS myevent;

DELIMITER //
CREATE EVENT myevent
    ON SCHEDULE 
      EVERY 1 DAY STARTS DATE_ADD(DATE_ADD(CURDATE(), INTERVAL 1 DAY), INTERVAL 5 MINUTE) 
    ON  COMPLETION  PRESERVE  ENABLE
    COMMENT 'A sample comment.'
    DO
      BEGIN
        CALL MyProc();
      END//
DELIMITER ;
 
-- 每天 00:05 執行
-- EVERY 1 DAY STARTS DATE_ADD(DATE_ADD(CURDATE(), INTERVAL 1 DAY), INTERVAL 5 MINUTE)
-- 每天 03:00 執行 
-- EVERY 1 DAY STARTS DATE_ADD(DATE_ADD(CURDATE(), INTERVAL 1 DAY), INTERVAL 3 HOUR) 



ALTER EVENT myevent ON COMPLETION PRESERVE DISABLE;
ALTER EVENT myevent ON COMPLETION PRESERVE ENABLE;
DROP EVENT [IF EXISTS] myevent;


DELETE FROM mysql.event
    WHERE db = 'myschema'
      AND definer = 'jon@ghidora'
      AND name = 'e_insert';

```

#### :books: 參考網站：
- [events-configuration](https://dev.mysql.com/doc/refman/5.7/en/events-configuration.html)
- [events-privileges](https://dev.mysql.com/doc/refman/5.7/en/events-privileges.html)

---

<a name="sql-syntax-transactions"></a>

```sql
START TRANSACTION;
SELECT @A:=SUM(salary) FROM table1 WHERE type=1;
UPDATE table2 SET summary=@A WHERE type=1;
COMMIT;
```

#### :books: 參考網站：
- [sql-syntax-transactions](http://dev.mysql.com/doc/refman/5.7/en/sql-syntax-transactions.html)

---
<a name="find_in_set"></a>

#### :books: 參考網站：
- [find_in_set](https://mariadb.com/kb/en/mariadb/find_in_set/)

---

#### :books: 參考網站：
- [validate-password-plugin](http://dev.mysql.com/doc/refman/5.6/en/validate-password-plugin.html)
- [ha-memcached-install](http://dev.mysql.com/doc/refman/5.7/en/ha-memcached-install.html)

---

<a name="procedure-analyse"></a>

```sql
SELECT ... FROM ... WHERE ... PROCEDURE ANALYSE([max_elements,[max_memory]])
SELECT col1, col2 FROM table1 PROCEDURE ANALYSE(10, 2000);
```

#### :books: 參考網站：
- [procedure-analyse](http://dev.mysql.com/doc/refman/5.7/en/procedure-analyse.html)

---

<a name="cursors"></a>

```sql
CREATE PROCEDURE curdemo()
BEGIN
  DECLARE done INT DEFAULT FALSE;
  DECLARE a CHAR(16);
  DECLARE b, c INT;
  DECLARE cur1 CURSOR FOR SELECT id,data FROM test.t1;
  DECLARE cur2 CURSOR FOR SELECT i FROM test.t2;
  DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = TRUE;

  OPEN cur1;
  OPEN cur2;

  read_loop: LOOP
    FETCH cur1 INTO a, b;
    FETCH cur2 INTO c;
    IF done THEN
      LEAVE read_loop;
    END IF;
    IF b < c THEN
      INSERT INTO test.t3 VALUES (a,b);
    ELSE
      INSERT INTO test.t3 VALUES (a,c);
    END IF;
  END LOOP;

  CLOSE cur1;
  CLOSE cur2;
END;
```

#### :books: 參考網站：
- [cursors](https://dev.mysql.com/doc/refman/5.7/en/cursors.html)

---

```sql
SHOW  GLOBAL  STATUS LIKE 'Questions';
```

#### :books: 參考網站：
- [Queries_per_second](https://en.wikipedia.org/wiki/Queries_per_second)

```sql
SHOW VARIABLES LIKE 'long_query_time';
SET GLOBAL slow_query_log = ON;
```

---

```sql
SELECT REGEXP_SUBSTR('ab12cd','[0-9]+');
-> 12

SELECT REGEXP_SUBSTR(
  'See https://mariadb.org/en/foundation/ for details',
  'https?://[^/]*');
-> https://mariadb.org
SELECT REGEXP_SUBSTR('ABC','b');
-> B

SELECT REGEXP_SUBSTR('ABC' COLLATE utf8_bin,'b');
->

SELECT REGEXP_SUBSTR(BINARY'ABC','b');
->

SELECT REGEXP_SUBSTR('ABC','(?i)b');
-> B

SELECT REGEXP_SUBSTR('ABC' COLLATE utf8_bin,'(?+i)b');
-> B
```

#### :books: 參考網站：
- [regexp_substr](https://mariadb.com/kb/en/mariadb/regexp_substr/)

---

---
<a name="mysql-proxy"></a>

mysql-proxy - high availability, load balancing and query modification for mysql

```
shell> sudo apt-get update
shell> sudo apt-get install mysql-proxy
shell> dpkg -L mysql-proxy
```

/etc/default/mysql-proxy
```
ENABLED="true"
OPTIONS="--defaults-file=/etc/mysql/mysql-proxy.cnf"
```

/etc/mysql/mysql-proxy.cnf
```
[mysql-proxy]
daemon = true
user = mysql
proxy-skip-profiling = true
keepalive = true
max-open-files = 2048
event-threads = 50
pid-file = /var/run/mysql-proxy.pid
log-file = /var/log/mysql-proxy.log
log-level = debug

admin-address = :4041
admin-username = root
admin-password = password
admin-lua-script = /usr/lib/mysql-proxy/lua/admin.lua

proxy-address = :4040
proxy-backend-addresses = 192.168.0.1:3306
proxy-read-only-backend-addresses = 192.168.0.2:3306
proxy-lua-script = /usr/lib/mysql-proxy/lua/proxy/balance.lua

```

```
shell> sudo chmod 0660 /etc/mysql/mysql-proxy.cnf
shell> sudo service mysql-proxy start 
```

```
shell> mysql -P4040 -uroot -p 
```

#### :books: 參考網站：
- [mysql-proxy-en](http://downloads.mysql.com/docs/mysql-proxy-en.pdf)

---

<a name="mydumper"></a>

mydumper - High-performance MySQL backup tool

```
shell> sudo apt-get update
shell> sudo apt-get install mydumper
```

```
shell> mydumper -u root -p mypass -B mysql -o mysql
shell> myloader -u root -p mypass -B mysql -d mysql
```

---
<a name="replication"></a>

`my.cnf`
```ini
[mysqld]
server-id=1
log-bin=mysql-bin
binlog_format=MIXED
binlog-do-db=database_name
binlog-ignore-db=database_name
innodb_flush_log_at_trx_commit=1
sync_binlog=1
```

```
shell> service mysql restart
```

```sql
CREATE USER 'repl'@'%.mydomain.com' IDENTIFIED BY 'slavepass';
GRANT REPLICATION SLAVE ON *.* TO 'repl'@'%.mydomain.com';

FLUSH TABLES WITH READ LOCK;
SHOW MASTER STATUS;
```

```
shell> mysqldump --all-databases --master-data > dbdump.db
```

`my.cnf`
```ini
[mysqld]
server-id=2
replicate-do-db=db_name
report-host=host_name
replicate-ignore-db=db_name
replicate-do-table=name
replicate-wild-do-table=foo%.bar%
master-connect-retry=60
```

```
shell> service mysql restart
```

```sql
STOP SLAVE;
RESET SLAVE ALL;
```

```
shell> mysql < dbdump.db
```

```
CHANGE MASTER TO
     MASTER_HOST='master_host_name',
     MASTER_USER='replication_user_name',
     MASTER_PASSWORD='replication_password',
     MASTER_LOG_FILE='recorded_log_file_name',
     MASTER_LOG_POS=recorded_log_position;
```

```sql
START SLAVE; 
```

```sql
UNLOCK TABLES; 
```

```sql
SET GLOBAL binlog_format = 'STATEMENT';
SET GLOBAL binlog_format = 'ROW';
SET GLOBAL binlog_format = 'MIXED';

SET SESSION binlog_format = 'STATEMENT';
SET SESSION binlog_format = 'ROW';
SET SESSION binlog_format = 'MIXED';
```

```sql
PURGE BINARY LOGS TO 'mariadb-bin.000063';
PURGE BINARY LOGS BEFORE '2013-04-22 09:55:22';
```

#### :books: 參考網站：
- [replication](http://dev.mysql.com/doc/refman/5.7/en/replication.html)
- [using-and-maintaining-the-binary-log](https://mariadb.com/kb/en/mariadb/using-and-maintaining-the-binary-log/)
- [option_mysqld_report-host](http://dev.mysql.com/doc/refman/5.7/en/replication-options-slave.html#option_mysqld_report-host)

---
<a name="mysqlslap"></a>
**mysqlslap - load emulation client**

```
shell> mysqlslap
shell> mysqlslap --delimiter=";" --create="CREATE TABLE a (b int);INSERT INTO a VALUES (23)" --query="SELECT * FROM a" --concurrency=50 --iterations=200
shell> mysqlslap --concurrency=5 --iterations=5 --query=query.sql --create=create.sql --delimiter=";"
shell> mysqlslap --user=root --password=insecure --auto-generate-sql --verbose --host=localhost
shell> mysqlslap --user=root --password=insecure --auto-generate-sql --concurrency=50 --iterations=200 --verbose --host=localhost 
```

```
Benchmark
        Average number of seconds to run all queries: 0.358 seconds
        Minimum number of seconds to run all queries: 0.230 seconds
        Maximum number of seconds to run all queries: 0.938 seconds
        Number of clients running queries: 50
        Average number of queries per client: 0
```

---

#### :books: 參考網站：
- [percona-monitoring-plugins](https://www.percona.com/downloads/percona-monitoring-plugins/LATEST/)
- [installing-templates](https://www.percona.com/doc/percona-monitoring-plugins/1.0/cacti/installing-templates.html)

---

#### :books: 參考網站：
- [mysqldumpslow](http://dev.mysql.com/doc/refman/5.7/en/mysqldumpslow.html)

---

「`使用者自訂函數`」 (User-Defined Function, `UDF`)

使用`MySQL`時，只要撰寫程式的內容符合`UDF`架構的規範，即可撰寫自定義的函數來提升`MySQL`的功能。

![](http://i.imgur.com/igMbwjJ.png)

#### :books: 參考網站：
- [安裝UDF程式庫 現成MySQL功能輕鬆擴充](http://www.netadmin.com.tw/article_content.aspx?sn=1304290003)

---

#### :books: 參考網站：
- [`MariaDB 10`新版大躍進](http://www.ithome.com.tw/news/86622)
- [A Quick Guide to Using the MySQL APT Repository](http://dev.mysql.com/doc/mysql-apt-repo-quick-guide/en/)
- [甲骨文發表`MySQL 5.7`：效能比5.6版快三倍](http://www.ithome.com.tw/news/99416)
- [`percona-xtrabackup`](https://www.percona.com/software/mysql-database/percona-xtrabackup)
- [`percona-xtradb-cluster`](https://www.percona.com/software/mysql-database/percona-xtradb-cluster)
- [提高MySQL效能及壓縮率  以TokuDB優化Log資料庫](http://www.netadmin.com.tw/article_content.aspx?sn=1507020002)
- [資料庫管理系統｜MySQL 5.5 Community Edition 新增半同步複製機制](http://www.ithome.com.tw/node/69364)
- [威脅資料存取的SQL Injection](http://www.ithome.com.tw/node/66384)
- [甲骨文新版 MySQL帶來兩倍查詢速度](http://www.ithome.com.tw/news/86373)
- [Google棄甲骨文MySQL，將大規模導入MariaDB](http://www.ithome.com.tw/node/82854)
- [甲骨文發表可強化資料庫延展性的MySQL Fabric](http://www.ithome.com.tw/news/88179)
- [痞客邦導入MySQL 檢索1億張照片也沒問題](http://www.ithome.com.tw/tech/87416)
- [Oracle釋出MySQL 5.6　增加添NoSQL功能](http://www.ithome.com.tw/node/67003)
- [ALTER TABLE Partition Operations](http://docs.oracle.com/cd/E17952_01/refman-5.5-en/alter-table-partition-operations.html)
- [`MySQL`](https://help.ubuntu.com/12.04/serverguide/mysql.html)
- [Transportable Tablespace Examples](https://dev.mysql.com/doc/refman/5.7/en/innodb-transportable-tablespace-examples.html)
- [Assigning Account Passwords](http://dev.mysql.com/doc/refman/5.0/en/assigning-passwords.html)
- [Setting Up SSL Certificates and Keys for MySQL](http://dev.mysql.com/doc/refman/5.0/en/creating-ssl-certs.html)
- [http://dev.mysql.com/doc/refman/5.1/en/using-ssl-connections.html](http://dev.mysql.com/doc/refman/5.1/en/using-ssl-connections.html)
- [MySQL Documentation: Other MySQL Documentation](http://dev.mysql.com/doc/index-other.html)
- [innodb_flush_log_at_trx_commit](http://dev.mysql.com/doc/refman/5.1/en/innodb-parameters.html#sysvar_innodb_flush_log_at_trx_commit)
- [http://dev.mysql.com/doc/refman/5.1/en/innodb-tuning.html](http://dev.mysql.com/doc/refman/5.1/en/innodb-tuning.html)
- [http://dev.mysql.com/doc/refman/5.6/en/optimizing-innodb.html](http://dev.mysql.com/doc/refman/5.6/en/optimizing-innodb.html)
- [Setup MySQL Master-Slave Replication on Debian/Ubuntu](https://www.vultr.com/docs/setup-mysql-master-slave-replication-on-debian-ubuntu)
- [Reset MySQL Root Password on Debian/Ubuntu](https://www.vultr.com/docs/reset-mysql-root-password-on-debian-ubuntu)
- [DELETE Syntax](http://dev.mysql.com/doc/refman/5.0/en/delete.html)
