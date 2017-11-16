<img src="http://subversion.apache.org/images/svn-name-banner.jpg" width="200">

![](https://tortoisesvn.net/docs/nightly/TortoiseSVN_en/images/Tortoise.png)

`subversion - Advanced version control system`

### 安裝 {#installing}

```
shell> apt-get install subversion
```

```
subversion - Advanced version control system
apache2 - Apache HTTP Server
libapache2-svn - Apache Subversion server modules for Apache httpd (dummy package)
apache2-utils - Apache HTTP Server (utility programs for web servers)
```
```
shell> sudo apt-get install subversion apache2 libapache2-svn apache2-utils
shell> a2enmod dav
shell> a2enmod dav_svn 
```

```
shell> sudo mkdir -p /svn/repos/
shell> sudo svnadmin create /svn/repos/hello
shell> sudo chown -R www-data:www-data /svn/repos/hello
```

`/etc/apache2/sites-available/svn.conf`

```apacheconf
Alias /svn/ "/svn/repos/"

<Location /svn>
  DAV svn
  SVNPath /svn/repos
  #SVNPath /var/lib/svn
  #SVNParentPath /var/lib/svn
  AuthType Basic
  AuthName "Subversion Repository"
  AuthUserFile /etc/apache2/dav_svn.passwd
  AuthzSVNAccessFile /etc/apache2/dav_svn.authz
  Require valid-user
</Location>
```

```
shell> a2ensite svn
shell> service apache2 reload
shell> sudo htpasswd -cm /etc/apache2/dav_svn.passwd svnuser
shell> sudo htpasswd -m /etc/apache2/dav_svn.passwd jane
shell> sudo htpasswd -m /etc/apache2/dav_svn.passwd jsmith
shell> sudo htpasswd -bm /etc/apache2/dav_svn.passwd jones Pwd4Steve
```

---

`svn+ssh://svnuser@SvnConnection/repos`
`svn+ssh://SvnConnection/repos`
`svn+ssh://svnuser@100.101.102.103/repos`
`svn+ssh://svnuser@mydomain.com/repos`

```
usage: svn <subcommand> [options] [args]
Subversion command-line client.
Type 'svn help <subcommand>' for help on a specific subcommand.
Type 'svn --version' to see the program version and RA modules
  or 'svn --version --quiet' to see just the version number.

Most subcommands take file and/or directory arguments, recursing
on the directories.  If no arguments are supplied to such a
command, it recurses on the current directory (inclusive) by default.

Available subcommands:
   add
   auth
   blame (praise, annotate, ann)
   cat
   changelist (cl)
   checkout (co)
   cleanup
   commit (ci)
   copy (cp)
   delete (del, remove, rm)
   diff (di)
   export
   help (?, h)
   import
   info
   list (ls)
   lock
   log
   merge
   mergeinfo
   mkdir
   move (mv, rename, ren)
   patch
   propdel (pdel, pd)
   propedit (pedit, pe)
   propget (pget, pg)
   proplist (plist, pl)
   propset (pset, ps)
   relocate
   resolve
   resolved
   revert
   status (stat, st)
   switch (sw)
   unlock
   update (up)
   upgrade

Subversion is a tool for version control.
For additional information, see http://subversion.apache.org/
```

``` 
shell> svn checkout svn+ssh://svnuser@100.101.102.103/repos
shell> svn co svn+ssh://svnuser@100.101.102.103/repos
```

``` 
shell> svn checkout http://100.101.102.103/svn
shell> svn co http://100.101.102.103/svn
shell> svn co http://100.101.102.103/svn --username svnuser
shell> svn co -r 12 http://100.101.102.103/svn --username svnuser
shell> svn checkout file:///var/svn/repos/test mine


shell> svn list http://100.101.102.103/svn
shell> svn ls http://100.101.102.103/svn
```

```
shell> svn info
shell> svn info http://100.101.102.103/svn
shell> svn info --show-item revision http://100.101.102.103/svn --username svnuser
```

---

```
general usage: svnadmin SUBCOMMAND REPOS_PATH  [ARGS & OPTIONS ...]
Subversion repository administration tool.
Type 'svnadmin help <subcommand>' for help on a specific subcommand.
Type 'svnadmin --version' to see the program version and FS modules.

Available subcommands:
   crashtest
   create
   delrevprop
   deltify
   dump
   freeze
   help (?, h)
   hotcopy
   info
   list-dblogs
   list-unused-dblogs
   load
   lock
   lslocks
   lstxns
   pack
   recover
   rmlocks
   rmtxns
   setlog
   setrevprop
   setuuid
   unlock
   upgrade
   verify
```

```
shell> svnadmin dump /var/svn/repos > full.dump
* Dumped revision 0.
* Dumped revision 1.
* Dumped revision 2.
…
```
```
shell> svnadmin load /var/svn/repos < full.dump
```
---

`Incremental Backups`

```
shell> svnadmin dump /var/svn/repos -r 0:50 > base.dump
* Dumped revision 0.
* Dumped revision 1.
* Dumped revision 2.
* Dumped revision 3.
* Dumped revision 4.
* Dumped revision 5.
* Dumped revision 6.
* Dumped revision 7.
* Dumped revision 8.
* Dumped revision 9.
* Dumped revision 10.
…
```

```
shell> svnadmin dump /var/svn/repos -r 51:100 --incremental > inc1.dump
* Dumped revision 51.
* Dumped revision 52.
* Dumped revision 53.
* Dumped revision 54.
* Dumped revision 55.
* Dumped revision 56.
* Dumped revision 57.
* Dumped revision 58.
* Dumped revision 59.
* Dumped revision 60.
… 
```

```
shell> svnadmin dump /var/svn/repos -r 101:150 --incremental > inc2.dump
* Dumped revision 101.
* Dumped revision 102.
* Dumped revision 103.
* Dumped revision 104.
* Dumped revision 105.
* Dumped revision 106.
* Dumped revision 107.
* Dumped revision 108.
* Dumped revision 109.
* Dumped revision 110.
… 
```

```
shell> svnadmin load /var/svn/repos < base.dump
shell> svnadmin load /var/svn/repos < inc1.dump
shell> svnadmin load /var/svn/repos < inc2.dump
```



`--deltas`
`--incremental`


```
shell> svnadmin hotcopy REPOS_PATH NEW_REPOS_PATH 
* Copied revision 0.
* Copied revision 1.
* Copied revision 2.
* Copied revision 3.
* Copied revision 4.
* Copied revision 5.
* Copied revision 6.
* Copied revision 7.
* Copied revision 8.
* Copied revision 9.
* Copied revision 10.
* Copied revision 11.
* Copied revision 12.
* Copied revision 13.
… 
```


```
general usage: svnsync SUBCOMMAND DEST_URL  [ARGS & OPTIONS ...]
Subversion repository replication tool.
Type 'svnsync help <subcommand>' for help on a specific subcommand.
Type 'svnsync --version' to see the program version and RA modules.

Available subcommands:
   initialize (init)
   synchronize (sync)
   copy-revprops
   info
   help (?, h)
```


#### :books: 參考網站：
- [subversion](http://subversion.apache.org/)
- https://tortoisesvn.net/ssh_howto.html
- https://tortoisesvn.net/docs/nightly/TortoiseSVN_en/tsvn-repository.html
- [svn checkout](http://svnbook.red-bean.com/en/1.7/svn.ref.svn.c.checkout.html)
- [svnadmin dump](http://svnbook.red-bean.com/en/1.7/svn.ref.svnadmin.c.dump.html)
- [svnadmin load](http://svnbook.red-bean.com/en/1.7/svn.ref.svnadmin.c.load.html)
- [mod_authz_svn](http://svnbook.red-bean.com/en/1.7/svn.ref.mod_authz_svn.conf.html)
- [svnadmin hotcopy](http://svnbook.red-bean.com/en/1.7/svn.ref.svnadmin.c.hotcopy.html)

