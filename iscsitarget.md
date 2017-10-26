
### 在 `Ubuntu` 14.04 LTS 上建置 `iscsitarget`

安裝作業系統及`iscsitarget`相關套件。
因本文主要介紹`iscsitarget`如何安裝及設定，作業系統方面就不再詳述。

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
### 安裝 iscsitarget 
```
shell> apt-get install iscsitarget
```

```
shell> vim /etc/default/iscsitarget
```

```
ISCSITARGET_ENABLE=true
```

```
shell> dd if=/dev/zero of=/storage/disk1.img count=0 obs=1 seek=640G
```

```
shell> vim /etc/ietd.conf
```

```
Target iqn.2001-04.com.example:storage.disk1
Lun 0 Path=/storage/disk1.img,Type=fileio
```















