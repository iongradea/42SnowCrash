1. get content of `/etc/passwd`, you can see the line for user of flag01 :
`flag01:42hDRfypTqqnw:3001:3001::/home/flag/flag01:/bin/bash`

2. Use john the ripper to decrypt the encrypted password of user flag01 :
- go to this website : `https://www.openwall.com/john/`
- download this for macos : `https://download.openwall.net/pub/projects/john/contrib/macosx/`

3. Download `john-1.8.0.9-jumbo-macosx_sse4.zip`, unzip it !

4. Go to `john-1.8.0.9-jumbo-macosx_sse4/run` and run `john -show /etc/passwd`,
you get the line : `flag01:abcdefg:3001:3001::/home/flag/flag01:/bin/bash`

5. Connect to flag01 user and get the flag !

