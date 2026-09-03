# Ghidra, GDB, Decompiler

Resources

```bash
https://github.com/Hellsender01/Youtube/blob/main/reverse-engineering/crackme3
```

```bash
┌──(kali㉿kali)-[~]
└─$ gdb crackme3    

(gdb) info functions

(gdb) disassemble main

(gdb) set  disassemble-flavor intel

(gdb) disassemble main

(gdb) x/s 0x402004
0x402004:	"Enter Key: "            ;Examine

(gdb) x/s 0x402010
0x402010:	"%d"                    ; Value= intiger type

```

![image.png](Ghidra,%20GDB,%20Decompiler/image.png)

```bash
(gdb) disassemble check_key
Dump of assembler code for function check_key:
   0x0000000000401196 <+0>:	endbr64
   0x000000000040119a <+4>:	push   rbp
   0x000000000040119b <+5>:	mov    rbp,rsp
   0x000000000040119e <+8>:	mov    DWORD PTR [rbp-0x4],edi
   0x00000000004011a1 <+11>:	mov    eax,DWORD PTR [rbp-0x4]
   0x00000000004011a4 <+14>:	sub    eax,0x64
   0x00000000004011a7 <+17>:	lea    edx,[rax+rax*1]
   0x00000000004011aa <+20>:	movsxd rax,edx
   0x00000000004011ad <+23>:	imul   rax,rax,0x51eb851f
   0x00000000004011b4 <+30>:	shr    rax,0x20
   0x00000000004011b8 <+34>:	mov    ecx,eax
   0x00000000004011ba <+36>:	sar    ecx,0x5
   0x00000000004011bd <+39>:	mov    eax,edx
   0x00000000004011bf <+41>:	sar    eax,0x1f
   0x00000000004011c2 <+44>:	sub    ecx,eax
   0x00000000004011c4 <+46>:	mov    eax,ecx
   0x00000000004011c6 <+48>:	imul   eax,eax,0x64
   0x00000000004011c9 <+51>:	sub    edx,eax
   0x00000000004011cb <+53>:	mov    eax,edx
   0x00000000004011cd <+55>:	test   eax,eax
   0x00000000004011cf <+57>:	jne    0x4011d8 <check_key+66>
   0x00000000004011d1 <+59>:	mov    eax,0x1
   0x00000000004011d6 <+64>:	jmp    0x4011d8 <check_key+66>
   0x00000000004011d8 <+66>:	pop    rbp
   0x00000000004011d9 <+67>:	ret

(gdb) p 0x64
$1 = 100
(gdb) print 0x64
$2 = 100
(gdb) p 0x51eb851f
$3 = 1374389535
(gdb) 

```

# Decompiler = Ghidra

```bash
┌──(kali㉿kali)-[~]
└─$ sudo apt install openjdk-11-jre

```

Rename important things

![image.png](Ghidra,%20GDB,%20Decompiler/image%201.png)

![image.png](Ghidra,%20GDB,%20Decompiler/image%202.png)

![image.png](Ghidra,%20GDB,%20Decompiler/image%203.png)

![image.png](Ghidra,%20GDB,%20Decompiler/image%204.png)