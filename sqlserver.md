![](http://i.imgur.com/D6krnAM.png)

```sql
USE master;
GO
-- 建立遠端資料來源的連結
-- Create a link to the remote data source. 
EXEC sp_addlinkedserver 
  @server = N'MyLinkServer',  -- here you can specify the name of the linked server
  @srvproduct = N'',
  @provider = N'SQLNCLI',     -- using SQL Server Native Client
  @datasrc = N'192.168.42.104',  
  @catalog = N'TestData';
GO

-- 利用登入 sa 和密碼 d89q3w4u，連接至連結伺服器 MyLinkServer。
EXEC sp_addlinkedsrvlogin 'MyLinkServer', 'false', NULL, 'sa', 'd89q3w4u';

EXEC sp_addlinkedsrvlogin
  @rmtsrvname = N'MyLinkServer',
  @useself = 'FALSE',
  @locallogin = NULL,
  @rmtuser = 'sa',
  @rmtpassword = 'd89q3w4u'

-- 
SELECT * FROM master.sys.sysservers;

--
SELECT * FROM MyLinkServer.TestData.dbo.Products;

-- 移除遠端伺服器 ACCOUNTS 和所有相關聯的遠端登入。
EXEC sp_dropserver 'MyLinkServer', 'droplogins';

EXEC sp_addlinkedserver 'rmtsrvname', '', 'SQLOLEDB', 'ip-addr'
EXEC sp_addlinkedsrvlogin 'rmtsrvname', 'false', NULL, 'sa', 'd89q3w4u';
EXEC sp_addlinkedsrvlogin 'rmtsrvname', 'false', NULL, 'SQLUser', 'Password'
```

```sql
SELECT name, physical_name AS CurrentLocation, state_desc
FROM sys.master_files
WHERE database_id = DB_ID(N'TestDB');
GO

ALTER DATABASE TestDB SET OFFLINE;
GO

ALTER DATABASE TestDB MODIFY FILE (NAME = TestDB, FILENAME = 'D:\SQLData\TestData.mdf');
ALTER DATABASE TestDB MODIFY FILE (NAME = TestDB_log, FILENAME = 'F:\SQLLog\TestData_log.ldf');
GO

ALTER DATABASE TestDB SET ONLINE;

ALTER DATABASE TestDB MODIFY FILE (NAME = TestDB, NEWNAME = 'TestData');
ALTER DATABASE TestDB MODIFY FILE (NAME = TestDB_log, NEWNAME = 'TestData_log');
GO

-- 驗證檔案變更
SELECT name, physical_name AS CurrentLocation, state_desc
FROM sys.master_files
WHERE database_id = DB_ID(N'TestDB');
GO
```

```sql
-- 變更資料庫的名稱
USE master;
GO
ALTER DATABASE TestDB Modify Name = TestData ;
GO
```


```sql
DECLARE @fileName AS sysname;
-- SET @filename = N'X:\SQLServerBackups\TestData' + '-' + CONVERT(nvarchar(30), GETDATE(), 112) + N'.bak';
SET @filename = N'X:\SQLServerBackups\TestData.bak'  

-- 備份
BACKUP DATABASE TestData
  TO DISK=@filename 
   WITH NOFORMAT;
GO
```

```sql
USE master
GO
RESTORE FILELISTONLY
   FROM DISK = 'X:\SQLServerBackups\TestData.bak'

-- 還原資料庫
RESTORE DATABASE TestData
   FROM DISK = 'X:\SQLServerBackups\TestData.bak'
   WITH RECOVERY,
   MOVE 'TestData' TO 'D:\MyData\TestData.mdf', 
   MOVE 'TestData_log' TO 'F:\MyLog\TestData_log.ldf'
GO
```

```sql
-- 備份交易記錄
BACKUP LOG TestData 
```

:star::star::star:
```sql
-- 切換到 master 資料庫
USE master;
GO

--Delete the TestData database if it exists.
IF EXISTS(SELECT * from sys.databases WHERE name='TestData')
BEGIN
    DROP DATABASE TestData;
END

-- 建立資料庫
--Create a new database called TestData.
CREATE DATABASE TestData;

-- 切換到 TestData 資料庫
USE TestData
GO

-- 建立資料表
CREATE TABLE dbo.Products
   (ProductID int PRIMARY KEY NOT NULL,
    ProductName varchar(25) NOT NULL,
    Price money NULL,
    ProductDescription text NULL)
GO

-- 將資料插入資料表
INSERT dbo.Products (ProductID, ProductName, Price, ProductDescription) VALUES (1, 'Clamp', 12.48, 'Workbench clamp')
INSERT dbo.Products (ProductName, ProductID, Price, ProductDescription) VALUES ('Screwdriver', 50, 3.17, 'Flat head')
INSERT dbo.Products VALUES (75, 'Tire Bar', NULL, 'Tool for changing tires.')
INSERT Products (ProductID, ProductName, Price) VALUES (3000, '3mm Bracket', .52)

-- 更新 Products 資料表
UPDATE dbo.Products SET ProductName = 'Flat Head Screwdriver' WHERE ProductID = 50

-- 讀取資料表的資料
SELECT ProductID, ProductName, Price, ProductDescription FROM dbo.Products
SELECT * FROM Products
SELECT ProductName, Price FROM dbo.Products
SELECT ProductID, ProductName, Price, ProductDescription FROM dbo.Products WHERE ProductID < 60
SELECT ProductName, Price * 1.07 AS CustomerPays FROM dbo.Products
```

:star::star::star:
```sql
--Delete the TestData database if it exists.
IF EXISTS(SELECT * from sys.databases WHERE name='TestData')
BEGIN
    DROP DATABASE TestData;
END

USE master ;
GO

-- 建立資料庫
--Create a new database called TestData.
CREATE DATABASE TestData
ON 
( NAME = TestData,
    FILENAME = 'E:\MyData\TestData.mdf',
    SIZE = 10,
    MAXSIZE = 50,
    FILEGROWTH = 5 )
LOG ON
( NAME = TestData_log,
    FILENAME = 'E:\MyLog\TestData_Log.ldf',
    SIZE = 5MB,
    MAXSIZE = 25MB,
    FILEGROWTH = 5MB ) ;
GO
```

:star::star::star:
```sql
--- 建立帳戶
CREATE LOGIN [computer_name\Mary] FROM WINDOWS WITH DEFAULT_DATABASE = [TestData];

-- 建立一個名為Mary的SQL Server帳戶
CREATE LOGIN Mary WITH PASSWORD = '340$Uuxwp7Mcxo7Khy';
CREATE LOGIN Mary WITH PASSWORD = '8fdKJl3$nlNv3049jsKK' DEFAULT_DATABASE = [TestData];
CREATE LOGIN Mary WITH PASSWORD = 'd89q3w4u', CHECK_POLICY = OFF, CHECK_EXPIRATION = OFF, DEFAULT_DATABASE = [TestData];

-- 
EXEC master..sp_addsrvrolemember @loginame = N'Mary', @rolename = N'sysadmin'

-- 在 TestData 建立對應的資料庫使用者Mary
USE TestData;

-- 授與資料庫的存取權
CREATE USER [Mary] FOR LOGIN [computer_name\Mary];
CREATE USER [Mary] FOR LOGIN [Mary];
EXEC sp_addrolemember 'db_owner', 'Mary';


-- 讓登入 Albert 成為目前資料庫的擁有者。
EXEC sp_changedbowner 'Albert';

-- 啟用帳戶Mary
ALTER LOGIN Mary ENABLE;

-- 停用帳戶Mary
ALTER LOGIN Mary DISABLE;

-- 變更帳戶Mary的密碼
ALTER LOGIN Mary WITH PASSWORD = 'd89q3w4u';

-- 
EXEC xp_instance_regwrite N'HKEY_LOCAL_MACHINE', N'SOFTWARE\Microsoft\MSSQLServer\MSSQLServer', 'LoginMode', N'REG_DWORD', 2

```

```sql
-- 建立檢視
CREATE VIEW vw_Names AS SELECT ProductName, Price FROM Products;
GO

-- 檢視的處理方式和資料表一樣。 使用 SELECT 陳述式存取檢視
SELECT * FROM vw_Names;
GO
```


```sql
-- 建立預存程序
CREATE PROCEDURE pr_Names @VarPrice money
   AS
   BEGIN
      -- The print statement returns text to the user
      PRINT 'Products less than ' + CAST(@VarPrice AS varchar(10));
      -- A second statement starts here
      SELECT ProductName, Price FROM vw_Names
            WHERE Price < @varPrice;
   END
GO

-- 執行預存程序
EXECUTE pr_Names 10.00;
GO

-- 執行下列陳述式，讓 Mary 具有 pr_Names 預存程序的 EXECUTE 權限。
GRANT EXECUTE ON pr_Names TO Mary;
GO
```

```sql
USE TestData;
GO

-- 使用 REVOKE 陳述式移除 Mary 對預存程序的執行權限
REVOKE EXECUTE ON pr_Names FROM Mary;
GO

-- 使用 DROP 陳述式移除 Mary 存取 TestData 資料庫的權限
DROP USER Mary;
GO

DROP LOGIN [<computer_name>\Mary];
GO

-- 使用 DROP 陳述式移除 pr_Names 預存程序
DROP PROC pr_Names;
GO

-- 使用 DROP 陳述式移除 vw_Names 檢視
DROP View vw_Names;
GO

-- 使用 DELETE 陳述式移除 Products 資料表中的所有資料列
DELETE FROM Products;
GO

-- 使用 DROP 陳述式移除 Products 資料表
DROP Table Products;
GO

-- 使用 DROP 陳述式移除 TestData 資料庫
USE MASTER;
GO
DROP DATABASE TestData;
GO
```

```sql
-- 在 TestData 資料庫上建立快照集
CREATE DATABASE TestData_snapshot_0600 ON
( NAME = TestData, FILENAME = 'D:\SQLData\TestData_snapshot_0600' )
AS SNAPSHOT OF TestData;
GO

SELECT name, database_id, source_database_id from sys.databases;

DROP DATABASE TestData_snapshot_0600;

_snapshot_0600
_snapshot_1200
_snapshot_1800

_snapshot_morning
_snapshot_noon
_snapshot_evening
```

```sql
SELECT ProductNumber, Category =
      CASE ProductLine
         WHEN 'R' THEN 'Road'
         WHEN 'M' THEN 'Mountain'
         WHEN 'T' THEN 'Touring'
         WHEN 'S' THEN 'Other sale items'
         ELSE 'Not for sale'
      END,
   Name
FROM Production.Product
ORDER BY ProductNumber;
```

```sql
-- 如何判斷 SQL Server 的版本及其元件
SELECT @@version
SELECT SERVERPROPERTY('productversion'), SERVERPROPERTY ('productlevel'),SERVERPROPERTY ('edition')
```

```sql
-- 顯示所有資料庫的記錄檔空間資訊
DBCC SQLPERF(LOGSPACE);

-- 傳回單一資料庫的相關資訊
EXEC sp_helpdb N'TestData';

-- 傳回所有資料庫的相關資訊
EXEC sp_helpdb;
```

---
<a name="bcp"></a>

:star::star::star:
```
bcp [database_name.] schema.{table_name | view_name | "query" {in data_file | out data_file | queryout data_file | format nul}

   [-a packet_size]
   [-b batch_size]
   [-c]
   [-C { ACP | OEM | RAW | code_page } ]
   [-d database_name]
   [-e err_file]
   [-E]
   [-f format_file]
   [-F first_row]
   [-h"hint [,...n]"] 
   [-i input_file]
   [-k]
   [-K application_intent]
   [-L last_row]
   [-m max_errors]
   [-n]
   [-N]
   [-o output_file]
   [-P password]
   [-q]
   [-r row_term]
   [-R]
   [-S [server_name[\instance_name]]
   [-t field_term]
   [-T]
   [-U login_id]
   [-v]
   [-V (80 | 90 | 100 | 110)]
   [-w]
   [-x]
   /?
```

```
shell> bcp database_name.dbo.table_name out data_file.bcp -q -c -T -r \n -t \,
```

```
shell> bcp "SELECT * FROM database_name.dbo.table_name WHERE " queryout data_file.bcp -q -c -T -r \n -t \,
```

```sql
LOAD DATA INFILE 'data_file.bcp' INTO TABLE tbl_name FIELDS TERMINATED BY ',' LINES TERMINATED BY '\n';
```

#### :books: 參考網站：
- [bcp](https://msdn.microsoft.com/zh-tw/library/ms162802(v=sql.120).aspx)
- [bcp](https://msdn.microsoft.com/en-us/library/ms191516.aspx)
- [LOAD DATA INFILE Syntax](https://dev.mysql.com/doc/refman/5.1/en/load-data.html)

---

#### :books: 參考網站：
- [queryexpress](http://www.albahari.com/queryexpress.aspx)
- [sp_addlinkedserver](http://msdn.microsoft.com/zh-tw/library/ms190479.aspx)
- [Creating a Table](http://msdn.microsoft.com/zh-tw/library/ms188264.aspx)
- [CREATE TABLE (Transact-SQL)](http://msdn.microsoft.com/zh-tw/library/ms174979.aspx)
- [在資料表中插入及更新資料](http://msdn.microsoft.com/zh-tw/library/ms365309.aspx)
- [如何判斷 SQL Server 的版本及其元件](http://support.microsoft.com/kb/321185/zh-tw)
- [讀取資料表的資料](http://msdn.microsoft.com/zh-tw/library/ms365310.aspx)
- [建立登入](http://msdn.microsoft.com/zh-tw/library/ms365326.aspx)
- [刪除資料庫物件](http://msdn.microsoft.com/zh-tw/library/ms365336.aspx)
- [CASE](http://msdn.microsoft.com/zh-tw/library/ms181765.aspx)
- [CAST 和 CONVERT](http://msdn.microsoft.com/zh-tw/library/ms187928.aspx)
- [重新命名資料庫](http://msdn.microsoft.com/zh-tw/library/ms345378.aspx)
- [sys.sysservers](http://msdn.microsoft.com/zh-tw/library/ms188740.aspx)
- [sp_addlinkedsrvlogin](http://msdn.microsoft.com/zh-tw/library/ms189811.aspx)
- [http://msdn.microsoft.com/zh-tw/library/ms189828.aspx](http://msdn.microsoft.com/zh-tw/library/ms189828.aspx)
- [CREATE USER](http://msdn.microsoft.com/zh-tw/library/ms173463.aspx)
- [DBCC SHRINKFILE](http://msdn.microsoft.com/zh-tw/library/ms189493.aspx)


---

```
shell> curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
shell> curl https://packages.microsoft.com/config/ubuntu/16.04/mssql-server.list | sudo tee /etc/apt/sources.list.d/mssql-server.list
shell> sudo apt-get update
shell> sudo apt-get install -y mssql-server
shell> sudo /opt/mssql/bin/mssql-conf setup
shell> systemctl status mssql-server
```

`Install tools on Ubuntu`
```
shell> curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
shell> curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list | sudo tee /etc/apt/sources.list.d/msprod.list
shell> sudo apt-get update 
shell> sudo apt-get install mssql-tools unixodbc-dev

shell> echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
shell> source ~/.bashrc
```



#### :books: 參考網站：
- https://docs.microsoft.com/en-us/sql/linux/sql-server-linux-setup-ubuntu
- https://docs.microsoft.com/en-us/sql/linux/sql-server-linux-setup-tools
- http://www.ithome.com.tw/news/104423
