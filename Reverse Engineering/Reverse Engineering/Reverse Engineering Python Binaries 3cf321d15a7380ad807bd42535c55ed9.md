# Reverse Engineering Python Binaries

Resources

```nasm
https://github.com/Hellsender01/Youtube/blob/main/reverse-engineering/crackme8
https://raw.githubusercontent.com/Hellsender01/Youtube/refs/heads/main/reverse-engineering/crackme8.py
```

```nasm
┌──(kali㉿kali)-[~]
└─$ checksec --file=./crackme8
[*] '/home/kali/crackme8'
    Arch:       amd64-64-little
    RELRO:      Partial RELRO
    Stack:      No canary found
    NX:         NX enabled
    PIE:        No PIE (0x400000)
    FORTIFY:    Enabled

┌──(kali㉿kali)-[~]
└─$ file crackme8
crackme8: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=f6af5bc244c001328c174a6abf855d682aa7401b, for GNU/Linux 2.6.32, stripped

```

![image.png](Reverse%20Engineering%20Python%20Binaries/image.png)

**Byte code**

![image.png](Reverse%20Engineering%20Python%20Binaries/image%201.png)

## Tools =

**Check if it pydata or not**

```bash
readelf -a crackme8
```

![image.png](Reverse%20Engineering%20Python%20Binaries/image%202.png)

**copy pydata**

```bash
objcopy --dump-section pydata=pydata.data crackme8 
```

```bash
┌──(kali㉿kali)-[~]
└─$ file pydata.data 
pydata.data: zlib compressed data
```

**Pyinstractor for extract zlib**

```bash
https://raw.githubusercontent.com/extremecoders-re/pyinstxtractor/refs/heads/master/pyinstxtractor.py
```

```bash
┌──(kali㉿kali)-[~]
└─$ chmod +x pyinstxtractor.py 

                                                                               
┌──(kali㉿kali)-[~]
└─$ python3 pyinstxtractor.py pydata.data 
[+] Processing pydata.data
[+] Pyinstaller version: 2.1+
[+] Python version: 3.8
[+] Length of package: 6805013 bytes

```

```bash
┌──(kali㉿kali)-[~/pydata.data_extracted]
└─$ ls
base_library.zip  libpython3.8.so.1.0      pyimod04_ctypes.pyc
crackme8.pyc  
```

```bash
pip3 install uncompyle6 --break-system-packages
```

```bash
┌──(kali㉿kali)-[~/pydata.data_extracted]
└─$ uncompyle6 crackme8.pyc > crackme8.py
                                                                               
┌──(kali㉿kali)-[~/pydata.data_extracted]
└─$ cat crackme8.py
# uncompyle6 version 3.9.3
# Python bytecode version base 3.8.0 (3413)
# Decompiled from: Python 3.13.12 (main, Feb  4 2026, 15:06:39) [GCC 15.2.0]
# Embedded file name: crackme8.py
import sys
passwd = input("Enter Password - ").replace("\n", "")
if "cj{bj^ewcjfxj" == lambda passwd: "".join((chr((ord(i) ^ 9) - 4 + 2) for i in ))(passwd):
    sys.exit("G00D J08!!")
else:
    sys.exit("W04K H4RD!!!")

# okay decompiling crackme8.pyc
                                 
```

![image.png](Reverse%20Engineering%20Python%20Binaries/image%203.png)

## More

```bash
https://www.youtube.com/watch?v=UK-YhoTkuPA&list=PL-DxAN1jsRa9151ezNuCbh7UkGS0bMPdw&index=19 
```