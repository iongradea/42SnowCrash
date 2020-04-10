# Level00 : find flag00

## Tutorial

1. Get all files that are created by flag00 user and we have permission access 
`find / -user flag00 2> /dev/null`
2. `cat` the file content to get the password for user flag00, you get :
`cdiiddwpgswtgt`
3. Decode the password of user flag00 with `www.dcode.fr/caesar-cipher`, the password is :
`nottoohardhere`
4. Connect as user flag00 using the password with the command : `su flag00`
5. Once connected to user `flag00`, do `getflag` and get the flag !

## Additional notes and resources 

- Encryption with Caesar code is a monoalphabetical substitution
- find command usage and example :
  - https://www.linode.com/docs/tools-reference/tools/find-files-in-linux-using-the-command-line/
  - https://www.geeksforgeeks.org/find-command-in-linux-with-examples/
  - https://www.tecmint.com/35-practical-examples-of-linux-find-command/
  - http://www.linux-france.org/article/memo/node126.html