# Level02 : find flag02

## Tutorial 

1. Download wireshark
   - wireshark : https://www.wireshark.org/download.html
2. Download pcap file from level02 user
   - command line : `scp -P 4242 level02@192.168.56.3:/home/user/level02/level02.pcap .`
   - password : flag01
3. Open file `level02.pcap` in wireshark 
4. Get password for flag02 from pcap file packet anaylysis : 
   - Select `analyze` and `follow TCP`
   - Select `follow`
   - Select `TCP`
   - Select in show and save data as `Hex Dump`
   - password raw : `ft_wandr...NDRel.L0L`
   - the `.` are ASCII code `7f` which means `del`
   - final password : `ft_waNDReL0L`
5. Connect as user `flag02` and launch command `getflag` to get the flag !