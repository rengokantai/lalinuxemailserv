# lalinuxemailserv
## 1.Basic Setup
### 2 Set up
```
yum --enablerepo=centosplus install -y postfix
```

config
```
vi /etc/postfix/main.cf
```

edit(line 419) uncomment
```
home_mailbox= Maildir/
```
if not found netstat command,
```
yum install -y net-tools
```
```
netstat -ltn
```
```
telnet localhost smtp
```
type
```
EHLO server.io
```


```
vi /etc/login.defs
```
edit
```
#MAIL_DIR
MAIL_FILE Maildir/
```

then
```
vi /etc/default/useradd
```
edit
```
CREATE_MAIL_SPOOL=no
```
```
echo 'MAIL=$HOME/Maildir/' > /etc/profile.d/mailenv.sh
```

### 3
install mailx as email client
```
yum install -y mailx
```
```
useradd ke
useradd test
```
```
su - ke
mailx test
```
mail:
```
title
body
.
```
```
exit
su - test
mailx
```
(press r to reply)


### 4
```
vi /etc/postfix/main.cf
```
search myhostname
```
myhostname = mail.yidi.me
```
search mydomain
```
mydomain = yidi.me
```
search myorigin
```
myorigin= $mydomain
```
search inet
```
inet_interfaces = all
```
search mynetworks_style
```
mynetworks_style = subnet
```
search relay_dom
```
relay_domains = 
```
save and exit ,then
```
systemctl restart postfix
```
send mail
```
su - ke
mailx x@gmail.xom
```
check log
```
exit
tail -n20 /var/log/maillog
```


### 5
```
yum install -y dovecot
systemctl enable dovecot && systemctl start dovecot
```
```
passwd ke
passwd test
```
```
telnet localhost pop3
```
then
```
user ke
pass passss
list
top 1

quit
```
test imap
```
telnet localhost imap
```
login
```
a login test passss
```
select inbox
```
b select inbox
```
```
c logout
```

use openssl to connect pop3 from another machine:
- create another machine, add hostname mail.yidi.me
```
openssl s_client -connect mail.yidi.me:pop3s
```
open ports=25(smtp) 110(pop3) 143(imap)  
For openssl, smtp=465 imaps=993 pop3s=995
```
openssl s_client -connect mail.yidi.me:pop3 -starttls pop3
```


### 6
[copy from here: secureing postfix](https://access.redhat.com/articles/1468593)  
paste in 
```
vi /etc/postfix/main.cf
```
```
smtp_use_tls = yes
smtpd_use_tls = yes
smtpd_tls_security_level = encrypt
smtpd_tls_auth_only = yes
smtp_tls_note_starttls_offer = yes
smtpd_tls_key_file = /etc/pki/tls/private/postfix.key
smtpd_tls_cert_file = /etc/pki/tls/certs/postfix.pem
smtpd_tls_dh1024_param_file = /etc/pki/tls/private/postfix.dh.param
smtp_tls_CAfile = /etc/pki/tls/certs/ca-bundle.crt
smtpd_tls_loglevel = 1
smtpd_tls_session_cache_timeout = 3600s
smtpd_tls_session_cache_database = btree:/var/lib/postfix/smtpd_tls_cache
tls_random_source = dev:/dev/urandom

smtpd_tls_mandatory_protocols = !SSLv2, !SSLv3
smtpd_tls_protocols = !SSLv2, !SSLv3
smtp_tls_mandatory_protocols = !SSLv2, !SSLv3
smtp_tls_protocols = !SSLv2, !SSLv3

smtp_tls_exclude_ciphers = EXP, MEDIUM, LOW, DES, 3DES, SSLv2
smtpd_tls_exclude_ciphers = EXP, MEDIUM, LOW, DES, 3DES, SSLv2

tls_high_cipher_list = kEECDH:+kEECDH+SHA:kEDH:+kEDH+SHA:+kEDH+CAMELLIA:kECDH:+kECDH+SHA:kRSA:+kRSA+SHA:+kRSA+CAMELLIA:!aNULL:!eNULL:!SSLv2:!RC4:!MD5:!DES:!EXP:!SEED:!IDEA:!3DES
tls_medium_cipher_list = kEECDH:+kEECDH+SHA:kEDH:+kEDH+SHA:+kEDH+CAMELLIA:kECDH:+kECDH+SHA:kRSA:+kRSA+SHA:+kRSA+CAMELLIA:!aNULL:!eNULL:!SSLv2:!MD5:!DES:!EXP:!SEED:!IDEA:!3DES

smtp_tls_ciphers = high
smtpd_tls_ciphers = high
```


create key and cert
```
openssl req -new -x509 -days 3650 -nodes -out /etc/pki/tls/certs/postfix.pem -keyout /etc/pki/tls/private/postfix.key
```
press enter for all
```
postfix reload
```
connect
```
openssl s_client -connect localhost:smtp -starttls smtp
```

strong algo
```
openssl dhparam -out /etc/pki/tls/private/postfix.dh.param.tmp 1024
mv /etc/pki/tls/private/postfix.dh.param.tmp  /etc/pki/tls/private/postfix.dh.param
```


### 7 Configure SASL
