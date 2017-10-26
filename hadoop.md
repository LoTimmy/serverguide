
### 在 `Ubuntu` 14.04 LTS 上建置 `hadoop`

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
### 安裝 hadoop 
``` 
shell> hadoop-daemon.sh start jobtracker
shell> hadoop-daemon.sh start tasktracker
shell> hadoop-daemon.sh start namenode
shell> hadoop-daemon.sh start datanode

shell> jps
shell> hadoop fs -lsr
shell> hadoop fs -ls /
shell> hadoop fs -mkdir test
shell> hadoop fs -ls test
shell> hadoop fs -rmr test
shell> hadoop fsck /
shell> lsof -i:54310

shell>
```

