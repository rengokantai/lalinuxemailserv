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
