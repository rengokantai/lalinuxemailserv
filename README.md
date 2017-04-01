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
