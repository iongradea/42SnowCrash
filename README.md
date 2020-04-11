# 42SnowCrash

SnowCrash project at 42

## Useful commands

- find command : `find path -user user 2> /dev/null`
- secure copy : `scp -P port user@ip:path destination`

## User tools

- virtualbox : open-source software for virtualizing the x86 computing architecture. It acts as a hypervisor, creating a VM (virtual machine) in which the user can run another OS (operating system)
  - virtual networking section : https://www.virtualbox.org/manual/ch06.html
- john the ripper : fast password cracker
  - url : https://www.openwall.com/john/
  - graphical interface with Johnny : http://openwall.info/wiki/john/johnny
- wireshark : network protocol analyzer
  - download url : https://www.wireshark.org/download.html
  - option : follow stream (tcp, udp etc.)

## Useful websites

- dcode : universal site for decoding messages
  - url : https://www.dcode.fr/en
- cloudshark : network protocol analyzer (same as wireshark)
  - url : https://www.cloudshark.org/captures/d9353f10683c

## Useful resources

- EFL format : Executable and linker format
  - https://linux-audit.com/elf-binaries-on-linux-understanding-and-analysis/
- Permissions (suid, sgid, sticky bit)
  - https://fr.wikipedia.org/wiki/Permissions_UNIX
  - https://www.tecmint.com/how-to-find-files-with-suid-and-sgid-permissions-in-linux/
  - https://linux.goffinet.org/administration/securite-locale/permissions-linux/
  - https://linuxconfig.org/how-to-use-special-permissions-the-setuid-setgid-and-sticky-bits
- Alias don't execute in scripts by default, need to set the shell as interactive :
  - https://stackoverflow.com/questions/30130954/alias-doesnt-work-inside-a-bash-script
