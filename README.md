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
