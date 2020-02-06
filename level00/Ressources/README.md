1. Get all files that are created by flag00 user and we have permission access 
`find / -user flag00 2> /dev/null`

2. `cat` the file content to get the password for user flag00, you get :
`cdiiddwpgswtgt`

3. Decode the password of user flag00 with `www.dcode.fr/caesar-cipher`, the password is :
`nottoohardhere`

4. Connect as user flag00 using the password with the command : `su flag00`

5. Once connected with user, do `getflag` and get the flag for level00 : 
`x24ti5gi3x0ol2eh4esiuxias`
