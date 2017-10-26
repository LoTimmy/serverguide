
`MTA`

`郵件傳送代理程式` (Mail Transfer Agent，MTA) 是整個電子郵件架構中最關鍵的角色，一般稱為郵件伺服器或`SMTP`郵件伺服器，主要用來寄送電子郵件，就像現實生活中的郵局角色一樣，幫忙寄發電子郵件。 
另外一個重要功能為接收別台郵件伺服器所傳遞過來的電子郵件。在開源碼社群中最著名的郵件伺服器莫過於`Postfix`和`Sendmail`。

`MUA`

`郵件使用代理程式` (Mail User Agent，MUA) 即是平常撰寫電子郵件時所使用的軟體（如Outlook），使用者可以利用MUA軟體來建立電子郵件並寄出電子郵件，或者利用`MUA`軟體來收取郵件伺服器上的電子郵件。 

`MDA`

所謂的郵件遞送代理人軟體 (Mail Delivery Agent，MDA)，是指當電子郵件到達目的郵件伺服器時，可先設定呼叫相關的`MDA`軟體來對電子郵件進行處理。 

`SMTP` 

![Imgur](http://i.imgur.com/xaLmV5v.png)

`SMTP` (Simple Mail Transfer Protocol，簡易郵件傳輸協定) 是電子郵件在進行傳遞時所使用的標準，顧名思義，它是個簡單的通訊協定（可是管理過郵件伺服器的網管人員，都不認為管理郵件伺服器是件簡單的事）。 

`SMTP`通訊協定相關指令

![](http://www.netadmin.com.tw/images/news/NP130401000513040115020002.png)

![Imgur](http://i.imgur.com/xTv2Emy.png)

`POP3`
`POP3` (Post Office Protocol Version 3) 是用來下載郵件伺服器上之電子郵件所使用的通訊協定。使用者利用`MUA`等軟體，將郵件伺服器上的電子郵件下載至本地端的電腦上，此種方式最大的缺點在於使用者必須將所有的信件下載至本地端後，方可得知信件的內容。 
在垃圾郵件橫行的現在，通常都在下載之後，才發現都是垃圾郵件，也因此浪費許多的頻寬和時間，而這也是一般網管人員建議使用者利用`IMAP`通訊協定來收取信件的原因。 

`IMAP` 
`IMAP` (Internet Mail Access Protocol) 是另一種使用者讀取郵件伺服器上郵件的通訊協定，它與`POP3`最大的不同在於，`POP3`協定通常必須將信件下載至本地端電腦內，方可檢視信件的內容，而且不可瀏覽過去的舊信，因為舊信都已下載到使用者的機器上，郵件伺服器內不會保留，雖然`POP3`也支援「在伺服器上保留郵件」的選項，但預設並不會勾選此選項。 

而`IMAP`協定則是將所有信件均保存於郵件主機上，使用者可以先查看信件的主旨來決定是否要下載該電子郵件，若決定不下載，即可將該電子郵件直接從郵件主機上刪除，不必浪費頻寬來下載不需要的電子郵件。 

`Relay` 
**`Relay`是郵件伺服器轉發電子郵件的機制，電子郵件會經由`Relay`機制，將電子郵件交由下一個郵件主機來進行轉發，直到找到正確的郵件伺服器（收件者所在的伺服器）為止。** 

由於原始SMTP通訊協定本身並未提供相關認證功能，因此當郵件伺服器上未設定任何的限制，即可能允許任何人透過自己的郵件伺服器發送電子郵件，而此種郵件伺服器即被稱為`Open Relay`郵件伺服器。 

一般來說，設定`Open Relay`的郵件伺服器往往是垃圾信發送者的最愛，也因此一個`Open Relay`的郵件伺服器通常會被視為垃圾信的發送站。 

在原始的`SMTP`通訊協定中並未考慮身分認證的問題，因此一般郵件伺服器會利用限制可存取郵件伺服器網域的方式，來限制僅有內部網域或是某一段網域才可利用郵件伺服器寄發電子郵件，藉此達到控管的目的。 

但此種方式並不實用，因為在現實生活中有非常大的機率，合法使用者並不會在該網域內，如此將造成合法使用者無法使用郵件伺服器因而產生不便，因此有了身分認證的需求（利用驗證帳號…密碼的方式）。 

為解決郵件伺服器身分認證問題，而誕生了`ESMTP` (Extension SMTP) 通訊協定，`ESMTP`通訊協定主要是增加身分認證機制。 

用戶在連接郵件伺服器時，會先認證用戶的合法性（利用帳號和密碼的機制進行驗證），一旦驗證通過，方可使用郵件伺服器收發電子郵件。 

---

```
shell> apt-get install postfix 
shell> apt-get install procmail
shell> apt-get install dovecot-common dovecot-pop3d dovecot-imapd dovecot-postfix

shell> postalias hash:/etc/aliases
```

```
disable_plaintext_auth = no
```

```
shell> dpkg-reconfigure postfix

shell> postconf
shell> postconf -m
shell> postconf -n

shell> postconf mail_version
shell> postconf myhostname
shell> postconf mydestination
```

```
shell> postconf default_destination_concurrency_limit
shell> postconf default_destination_recipient_limit
shell> postconf default_process_limit
shell> postconf hash_queue_depth
```

```
smtpd_sender_restrictions = reject_unknown_sender_domain
smtpd_sender_restrictions = reject_unknown_sender_domain,
    check_sender_access hash:/etc/postfix/access
```

**The Spamhaus Block List**

**`SBL`**

```
myorigin = $myhostname
myorigin = $mydomain


accept_domain = test.net 
mydestination = $accept_domain

mydestination = $myhostname localhost.$mydomain localhost
mydestination = $myhostname localhost.$mydomain localhost $mydomain
mydestination = $myhostname localhost.$mydomain localhost www.$mydomain ftp.$mydomain

mynetworks_style = subnet
mynetworks_style = host

mynetworks = 172.168.96.0/24
mynetworks = 127.0.0.0/8
mynetworks = 127.0.0.0/8 168.100.189.2/32
mynetworks = 168.100.189.0/28, 127.0.0.0/8

relay_domains = $mydestination
relay_domains =
relay_domains = $mydomain

relayhost =
relayhost = $mydomain
relayhost = [mail.$mydomain]
relayhost = [mail.isp.tld]

proxy_interfaces = 1.2.3.4

virtual_alias_domains = example.com ...other hosted domains...
virtual_alias_maps = hash:/etc/postfix/virtual
```

```
postmaster@example.com postmaster
info@example.com       joe
sales@example.com      jane
```

```
shell> postmap /etc/postfix/virtual
shell> postfix reload
```

#### :books: 參考網站：
- [Postfix Virtual Domain Hosting Howto](http://www.postfix.org/VIRTUAL_README.html)

```
shell> postfix reload

shell> postfix check
shell> egrep '(reject|warning|error|fatal|panic):' /some/log/file 
 
```

#### :books: 參考網站：
- [BASIC_CONFIGURATION_README](http://www.postfix.org/BASIC_CONFIGURATION_README.html)

```
permit_mynetworks
permit_sasl_authenticated
```

```
shell> postconf -e 'myhostname = mail.example.com'
shell> postconf -e 'mydomain = example.com'
shell> postconf -e 'myorigin = $mydomain'

shell> postconf -e 'smtpd_banner = $myhostname ESMTP $mail_name'
shell> postconf -e 'smtpd_use_tls = no'
shell> postconf -e 'home_mailbox = Maildir/'
shell> postconf -e 'inet_protocols = ipv4'
shell> postconf -e 'smtpd_client_restrictions = permit_mynetworks, permit_sasl_authenticated, reject_rbl_client zen.spamhaus.org'

shell> postconf -e 'message_size_limit = 0'
shell> postconf -e 'smtpd_recipient_limit = 1000'

shell> postconf -e 'append_dot_mydomain = no'
shell> postconf -e 'append_at_myorigin = no'

shell> postconf -e 'maximal_queue_lifetime = 0'
shell> postconf -e 'bounce_queue_lifetime = 0'

shell> postconf -e 'disable_vrfy_command = yes'
shell> postconf -e 'smtpd_helo_required = yes'

shell> service dovecot restart
shell> service postfix restart

shell> postqueue -d
shell> postsuper -d queue_id
shell> postsuper -d ALL
shell> postcat -q queue_id
```

---


```
shell> apt-get install clamav 
shell> apt-get install spamassassin
shell> apt-get install mailscanner

shell> vim /etc/MailScanner/MailScanner.conf
```

```
Run As User = postfix
Run As Group = postfix

Incoming Queue Dir = /var/spool/mqueue.in
Outgoing Queue Dir = /var/spool/mqueue
MTA = sendmail

Virus Scanning = yes
Virus Scanners = clamav

Use SpamAssassin = yes
SpamAssassin User State Dir = /var/spool/MailScanner/spamassassin

Sign Clean Messages = no
Dangerous Content Scanning = no
Maximum Archive Depth = 0
Use Stricter Phishing Net = no
Highlight Phishing Fraud = no

Spam Modify Subject = start
High Scoring Spam Modify Subject = start

Spam Subject Text = {Spam?}
High Scoring Spam Subject Text = {Spam?}


Spam Actions = deliver header "X-Spam-Status: Yes"
Spam Actions = store forward anonymous@ecs.soton.ac.uk
High Scoring Spam Actions = store
Non Spam Actions = deliver header "X-Spam-Status: No"

Spam List Definitions = %etc-dir%/spam.lists.conf
Spam List = # spamhaus-ZEN # You can un-comment this to enable them
Spam Lists To Be Spam = 1
Spam Domain List =

Required SpamAssassin Score = 6
High SpamAssassin Score = 10

Phishing Subject Text = {Fraud?}
Phishing Modify Subject = no

Disarmed Modify Subject = start
Disarmed Subject Text = {Disarmed}

Virus Modify Subject = start
Virus Subject Text = {Virus?}

Scanned Modify Subject = no
Scanned Subject Text = {Scanned}

```

#### :books: 參考網站：
- [mailscanner](https://www.mailscanner.info/)
- [mailscanner](https://www.mailscanner.info/MailScanner.conf.index.html)
- [mailscanner](https://www.mailscanner.info/postfix/)


```
shell> vim /etc/MailScanner/rules/spam.whitelist.rules
```

```
shell> vim /etc/default/mailscanner
```
```
run_mailscanner=1
```

```
shell> vim /etc/default/spamassassin	
```
```
ENABLED=1
```

```
shell> vim /etc/postfix/header_checks	
```
```
/^Received:/ HOLD
```
```
shell> postconf -e 'header_checks = regexp:/etc/postfix/header_checks'	
```

```
shell> vim /etc/init.d/mailscanner
```
```
start-stop-daemon --start --quiet --nicelevel $run_nice --exec $DAEMON --name $NAME -- $DAEMON_ARGS
start-stop-daemon --start --quiet --nicelevel $run_nice -c ${user} --exec $DAEMON --name $NAME -- $DAEMON_ARGS
```

```
shell> service spamassassin restart
shell> service mailscanner restart
```

```
shell> apt-get install mysql-server
shell> apt-get install roundcube roundcube-mysql
```

```
shell> vim /etc/roundcube/apache.conf
```

```
Alias /roundcube/program/js/tiny_mce/ /usr/share/tinymce/www/
Alias /roundcube /var/lib/roundcube
```

```
shell> vim /etc/roundcube/main.inc.php
```
```
$rcmail_config['default_host'] = '127.0.0.1';
$rcmail_config['smtp_server'] = '127.0.0.1';
$rcmail_config['language'] = 'zh_TW';
$rcmail_config['create_default_folders'] = TRUE;
$rcmail_config['default_charset'] = 'UTF-8';
$rcmail_config['htmleditor'] = TRUE;
$rcmail_config['timezone'] = '8';
```

```
shell> apt-get install fetchmail
shell> vim ~/.fetchmailrc
```

```
shell> grep sasl_method=LOGIN /var/log/mail.log
```

```
shell> qshape -s hold | head
shell> qshape -s active
shell> qshape active
```

```
shell> vim /etc/rsyslog.d/50-default.conf
```
```
mail.*                          -/dev/null
```

```
"protocol imap {
  mail_plugins = autocreate
}
plugin {
  autocreate = Trash
  autocreate2 = Spam
  #autocreate3 = ..etc..
  autosubscribe = Trash
  autosubscribe2 = Spam
  #autosubscribe3 = ..etc..
}
"
```
---

<img src="http://i.imgur.com/GHjoJiD.jpg" alt="" width=58 height=58>

### 服务器使用 SpamAssassin 防治垃圾邮件

> SpamAssassin 是免費的垃圾郵件篩選器
> SpamAssassin 是一个邮件过滤器，它部署在邮件服务器端，可以使用一系列的机制来确认垃圾邮件，这些机制包括：文本分析、Bayesian （贝叶斯判决规则）过滤、DNS 数据块列表，以及合作性的过滤数据库。
> SpamAssassin 包括 spamd 守护进程和 spamc 客户端。

- Headeranalysis (标题分析)：检查疑似垃圾邮件的标题，有些垃圾邮件被人采用某种技巧处理后，可能会被误认为是合法的电子邮件。
- Text analysis (文本分析)：检查电子邮件正文中的垃圾邮件特征。
- Blacklists (黑名单)：检查名单，看看发件人是否在现有垃圾邮件发送者列表中。
- Database (数据库)：检查针对 Vipul's Razor (razor.sourceforge.net) 的邮件签名，它是一个垃圾邮件跟踪数据库。


sample-nonspam.txt.gz

```
shell> echo "hi there" | spamc
```

``` 
 X-Spam-Checker-Version: SpamAssassin 3.3.2-r929478 (2010-03-31) on sobell.com 
 X-Spam-Flag: YES 
 X-Spam-Level: ****** 
 X-Spam-Status: Yes, score=6.9 required=5.0 tests=EMPTY_MESSAGE,MISSING_DATE, 
 MISSING_HEADERS,MISSING_MID,MISSING_SUBJECT,NO_HEADERS_MESSAGE,NO_RECEIVED, 
 NO_RELAYS autolearn=no version=3.3.2-r929478 
 X-Spam-Report: 
 * -0.0 NO_RELAYS Informational: message was not relayed via SMTP 
 * 1.2 MISSING_HEADERS Missing To: header 
 * 0.1 MISSING_MID Missing Message-Id: header 
 * 1.8 MISSING_SUBJECT Missing Subject: header 
 * 2.3 EMPTY_MESSAGE Message appears to have no textual parts and no 
 * Subject: text 
 * -0.0 NO_RECEIVED Informational: message has no Received headers 
 * 1.4 MISSING_DATE Missing Date: header 
 * 0.0 NO_HEADERS_MESSAGE Message appears to be missing most RFC-822 
 * headers 
 hi there 
 Subject: [SPAM] 
 X-Spam-Prev-Subject: (nonexistent)
```

```
shell> spamassassin --help

shell> spamassassin --test-mode < /usr/share/doc/spamassassin/examples/sample-nonspam.txt
shell> spamassassin --test-mode < /usr/share/doc/spamassassin/examples/sample-spam.txt

shell> mail -s "mail-tester" web-PDYtN2@mail-tester.com < /usr/share/doc/spamassassin/examples/sample-nonspam.txt
shell> mail -s "mail-tester" web-PDYtN2@mail-tester.com < /usr/share/doc/spamassassin/examples/sample-spam.txt

shell> spamc -R < /usr/share/doc/spamassassin/examples/sample-spam.txt

shell> sendmail root < /usr/share/doc/spamassassin/examples/sample-nonspam.txt
shell> sendmail root < /usr/share/doc/spamassassin/examples/sample-spam.txt
```

```
shell> sa-learn --clear
shell> sa-learn --showdots --spam /home/spam/Maildir/{cur,new}
shell> sa-learn --dump magic
```

$HOME/.procmailrc
```
MAILDIR=$HOME/Mail


:0 c
* ^X-Spam-Status: Yes
.\&V4NXPpD1TvY-/     # 垃圾郵件

.Junk/
```

#### :books: 參考網站：
- [sample-nonspam.txt](http://spamassassin.apache.org/full/3.0.x/dist/sample-nonspam.txt)
- [sample-spam.txt](http://spamassassin.apache.org/full/3.0.x/dist/sample-spam.txt)
- [procmailrc](http://spamassassin.apache.org/full/3.0.x/dist/procmailrc.example)
- [透過Procmail連結Postfix伺服器與MySQL](http://www.netadmin.com.tw/article_content.aspx?sn=1304010005)

---


![Imgur](http://i.imgur.com/EMVHPdi.png)

```
mydomain.com.   3600    IN  TXT "v=spf1 mx mx:mydomain.com -all"
mydomain.com.   3600    IN  TXT "v=spf1 ip4:192.168.1.100 -all"
```

```
"+"	Pass
"-"	Fail
"~"	SoftFail
"?"	Neutral
```

```
"v=spf1 -all"
"v=spf1 a -all"
"v=spf1 a mx -all"
"v=spf1 +a +mx -all"
```

---

**opendkim-genkey - DKIM filter key generation tool**

```
shell> apt-get install opendkim-tools
shell> opendkim-genkey -r -D -d mydomain.com
```
---

#### :books: 參考網站：
- [mail-tester](http://www.mail-tester.com/)

---

**swaks - SMTP command-line test tool**

```
shell> apt-get install swaks 
```

```
shell> swaks --to user@example.com --server test-server.example.net
shell> swaks --to user@example.com --from me@example.com --auth CRAM-MD5 --auth-user me@example.com --header-X-Test "test email"
shell> swaks --to user@example.com --from me@gmail.com --auth --auth-user=me --auth-password= -tls --server smtp.gmail.com:587 --header ""
```

```
shell> swaks --attach-type text/html --attach report.html
shell> swaks --body report.html --add-header "MIME-Version: 1.0" --add-header "Content-Type: text/html"
```

---

```
shell> sa-update && /etc/init.d/spamassassin reload
```
---

`轉發` (`Relay`)

`DNS 黑名單` (`DNSBL`)

- `DNSBL` 是儲存網際網路`SMTP 主機記錄的資料庫，這些主機是已知的垃圾電子郵件來源或容許的第三者、開放式轉遞。

即時黑名單殺傷力很強，經常因誤攔合法信件頻率過高，遭致批評。因為大部分郵件伺服器軟體都支援RBL，當企業擁有的郵件伺服器IP或網域名稱被列入DNS的黑名單，企業寄送的任何信件，甚至SMTP連線，都可能被視為垃圾信行為，導致收件人無法收到信件。收到垃圾信，處理起來固然令人困擾，但是收件人無法順利接到信件，使得整個郵件系統形同停擺。

在黑名單榜上有名，除了歸咎於企業刻意主動發出大量信件，造成網路使用者的困擾外，被列入黑名單有時也可能是因為IT人員未注意伺服器的不當設定，例如Open Relay、Open Proxy，或是遭遇電腦病毒感染、網路入侵，在不知情的情況下發出大量垃圾信所致。

`Open Relay`
不限制信件轉發的來源就是`Open Relay`。發送垃圾郵件的人能利用網際網路上這些伺服器的`Open Relay`來傳送大批的訊息，以便偽裝真實的郵件來源進而隱藏自己的身分，讓無辜的企業成為「免費」、「匿名」傳送大量郵件的資源，增加無謂的郵件處理工作。

`Open Proxy`
`代理伺服器` (`Proxy Server`) 可透過HTTP連接任意的TCP埠，如果不限制連線來源的TCP埠或服務對象範圍，可能會被濫用，例如連至郵件伺服器的25埠作為中繼站，建立SMTP或Telnet等服務傳送特定指令，即可發出大量垃圾信，甚至攻擊內部網路。

各種即時黑名單組織各自採取不同封鎖立場和做法，例如`Spamhaus`提供2種黑名單，`SBL`分別針對垃圾郵件來源和已知垃圾郵件運作記錄的發信人，`XBL`專攻非法用途，包括Proxy、蠕蟲和木馬等；而Composite Blocking List不收集Open Relay名單，站方立場不提供支援資料或證據檔案，你無法詢問被列入的原因。

解除封鎖最簡單的方法是從網頁上直接解除，但大部分都需要寫信通知`RBL`管理者。`Spamhaus`的移除程序中提到，他們希望用戶主動聯繫`SBL`的工作人員，解釋企業解決垃圾郵件濫發的方法，如果處理完成，他們才會從`SBL`裡移除。

---

#### :books: 參考網站：
- [zen](https://www.spamhaus.org/zen/)
- [setup.dns](http://www.iredmail.org/docs/setup.dns.html)
- [SPF_Record_Syntax](http://www.openspf.org/SPF_Record_Syntax)
- [關於 DKIM](https://support.google.com/a/answer/174124?hl=zh-Hant)
- [啟動 SMTP 連線的 DNS 黑名單過濾程式](http://www.ibm.com/support/knowledgecenter/zh-tw/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_ENABLING_HOST_CHECKING_FOR_SMTP_CONNECTIONS_OVER.html)
- [當企業上了垃圾郵件黑名單](http://www.ithome.com.tw/node/31174)
- [大量寄件者指南](https://support.google.com/mail/answer/81126?hl=zh-Hant)
- [設定 MX 紀錄](https://support.google.com/a/answer/140034?hl=zh-Hant&ref_topic=2705493)
- [使用 DKIM 驗證電子郵件](https://support.google.com/a/answer/174124?hl=zh-Hant&ref_topic=2752442&rd=1)
- [LOCAL_RECIPIENT_README](http://www.postfix.org/LOCAL_RECIPIENT_README.html)

---

example.com
example.com

```
canonical_maps = hash:/etc/postfix/canonical
```

#### :books: 參考網站：
- [ADDRESS_REWRITING_README](http://www.postfix.org/ADDRESS_REWRITING_README.html)
- [canonical](http://www.postfix.org/canonical.5.html)

---

Default user:	admin
Default password: 	secret

```
myserver.net.in.	3599	IN	TXT	"v=spf1 mx mx:ds1515.dyndns.info -all"
```
#### :books: 參考網站：
- [企业开源电子邮件系统安全保障实战精要: 第 2 部分，Postfix 安全防护实战及垃圾邮件防范](http://www.ibm.com/developerworks/cn/linux/1304_liyang_mailsecure2/)