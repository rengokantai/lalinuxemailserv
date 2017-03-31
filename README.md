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
