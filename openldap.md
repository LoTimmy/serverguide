![OpenLDAP](http://www.openldap.org/images/headers/LDAPworm.gif)

> `目錄服務`（`Directory Service`）廣義的定義，是以目錄結構提供資料儲存機制，透過特定的通訊協定存取，藉由目錄服務資訊的提供，可以將網路上原本互不相識的兩個端點，進行連結及資訊或服務交換。就像是以前的電話簿黃頁資訊一樣，只要申請了電話，便可以在電話簿中查到其他人的連結訊息。
  
> 對於企業的資訊部門來說，想要管理員工存取網路資源的權限，最好的方式就是整合`LDAP`（`Lightweight Directory Access Protocol`），它是一種集中管理使用者帳號認證的機制，當員工人數成長到數百人，甚至上千人的時侯，資訊人員就很難以手動方式在每臺伺服器與網路設備個別建立使用者帳號，透過`LDAP`一次性完成員工網路帳號的管理，有其必要性。

> 許多`IT`廠商都提出自行開發的`LDAP`架構，其中比較常見的有微軟的`AD`（`Active Directory`），以及Unix平臺的`OpenLDAP`等。

> 在`Unix`平臺上，由於`OpenLDAP`沒有圖形化的管理介面，因此完成安裝之後，預設只能在在文字模式的終端機介面輸入指令，才能管理`LDAP`。

`LDAP` 埠號 `389`
`LDAP SSL` 埠號 `636`

安裝作業系統及`OpenLDAP`相關套件。
因本文主要介紹`OpenLDAP`如何安裝及設定，作業系統方面就不再詳述。

### 安裝 OpenLDAP
```
shell> apt-get install slapd ldap-utils  
```

![OpenLDAP](http://i.imgur.com/4ha4qG8.jpg)
![OpenLDAP](http://i.imgur.com/Bg2NX1C.jpg)

---
```
shell> ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/ldap/schema/cosine.ldif
shell> ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/ldap/schema/nis.ldif
shell> ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/ldap/schema/inetorgperson.ldif

shell> apt-get install vim-nox
shell> slappasswd -s secret
shell> vim backend.example.com.ldif
```

```
# Load dynamic backend modules
dn: cn=module,cn=config
objectClass: olcModuleList
cn: module
olcModulepath: /usr/lib/ldap
olcModuleload: back_hdb.la

# Database settings
dn: olcDatabase=hdb,cn=config
objectClass: olcDatabaseConfig
objectClass: olcHdbConfig
olcDatabase: {1}hdb
olcSuffix: dc=example,dc=com
olcDbDirectory: /var/lib/ldap
olcRootDN: cn=admin,dc=example,dc=com
olcRootPW: {SSHA}Gev3GSf6VLPqOlw/0c+1MFuFmCu1JDyU
olcDbConfig: set_cachesize 0 2097152 0
olcDbConfig: set_lk_max_objects 1500
olcDbConfig: set_lk_max_locks 1500
olcDbConfig: set_lk_max_lockers 1500
olcDbIndex: objectClass eq
olcLastMod: TRUE
olcDbCheckpoint: 512 30
olcAccess: to attrs=userPassword by dn="cn=admin,dc=example,dc=com" write by anonymous auth by self write by * none
olcAccess: to attrs=shadowLastChange by self write by * read
olcAccess: to dn.base="" by * read
olcAccess: to * by dn="cn=admin,dc=example,dc=com" write by * read
```

```
shell> ldapadd -Y EXTERNAL -H ldapi:/// -f backend.example.com.ldif
```

---

```
shell> apt-get install ldapscripts
shell> vim /etc/ldapscripts/ldapscripts.conf
```

```
SERVER=localhost
SUFFIX="dc=example,dc=com"
BINDDN="cn=admin,dc=example,dc=com"
BINDPWDFILE="/etc/ldapscripts/ldapscripts.passwd"
GSUFFIX="ou=Groups"
USUFFIX="ou=People"
MSUFFIX='ou=Computers'
GIDSTART=10000
UIDSTART=10000
MIDSTART=10000
PASSWORDGEN="pwgen"
RECORDPASSWORDS="yes"
PASSWORDFILE="/var/log/ldapscripts_passwd.log"
UTEMPLATE="/etc/ldapscripts/ldapadduser.template"
```

```
shell> cp /usr/share/doc/ldapscripts/examples/ldapadduser.template.sample /etc/ldapscripts/ldapadduser.template
shell> sh -c "echo -n 'secret' > /etc/ldapscripts/ldapscripts.passwd"
shell> chmod 400 /etc/ldapscripts/ldapscripts.passwd
```

---

```
shell> ldapinit
shell> ldapaddgroup example
shell> ldapadduser george example
shell> ldapadduser john example
shell> ldapsetpasswd george
shell> ldapdeleteuser george
shell> ldapdeletegroup example
shell> ldapsearch -xLLL -b "dc=example,dc=com" uid=john sn givenName cn
shell> ldapsearch -xLLL -b "dc=example,dc=com" sn givenName cn
```
---

### LDAP Authentication

```
shell> apt-get install libnss-ldap
shell> dpkg-reconfigure ldap-auth-config
shell> auth-client-config -t nss -p lac_ldap
shell> pam-auth-update
shell> vim /etc/pam.d/common-account
```

```
session       required pam_mkhomedir.so umask=0022 skel=/etc/skel
```

---

### 疑難排解	

```
shell> id
shell> getent group
shell> getent shadow
shell> getent passwd
```

---

#### :books: 參考網站：
[OpenLDAP Server](https://help.ubuntu.com/10.04/serverguide/openldap-server.html)
[http://www.ithome.com.tw/node/38578](http://www.ithome.com.tw/node/38578)