# Reverse Engineering of Second Software

## Resources

```jsx
https://github.com/Hellsender01/Youtube/blob/main/reverse-engineering/crackme1
```

## Tools

**RadarE**

```jsx
┌──(kali㉿kali)-[~]
└─$ radare2

┌──(kali㉿kali)-[~]
└─$ r2
```

Analyze

```jsx
┌──(kali㉿kali)-[~]
└─$ chmod +777 crackme1 
                                                                                
┌──(kali㉿kali)-[~]
└─$ r2 -d crackme1       
[0x7f8aa69d0d40]> aaa
INFO: Analyze all flags starting with sym. and entry0 (aa)
INFO: Analyze imports (af@@@i)
INFO: Analyze entrypoint (af@ entry0)
INFO: Analyze symbols (af@@@s)
INFO: Analyze all functions arguments/locals (afva@@@F)
INFO: Analyze function calls (aac)
INFO: Analyze len bytes of instructions for references (aar)
INFO: Finding and parsing C++ vtables (avrr)
INFO: Analyzing methods (af @@ method.*)
INFO: Recovering local variables (afva@@@F)
INFO: Skipping type matching analysis in debugger mode (aaft)
INFO: Propagate noreturn information (aanr)
INFO: Use -AA or aaaa to perform additional experimental analysis
[0x7f8aa69d0d40]> 

[0x7f8aa69d0d40]> afl
0x00401080    1     11 sym.imp.puts
0x00401090    1     11 sym.imp.strlen
0x004010a0    1     11 sym.imp.__stack_chk_fail
0x004010b0    1     11 sym.imp.strcmp
0x004010c0    1     11 sym.imp.exit
0x004010d0    1     47 entry0
0x00401110    4     31 sym.deregister_tm_clones
0x00401140    4     49 sym.register_tm_clones
0x004012c0    4    101 sym.__libc_csu_init
0x00401100    1      5 sym._dl_relocate_static_pie
0x004011b6   10    257 main

[0x7f8aa69d0d40]> db main = debug breakpoint
[0x7f8aa69d0d40]> dc = debug continue
INFO: hit breakpoint at: 0x4011b6

[0x004011b6]> pdf

```

```r
V ---> press "P" = [Visual Mode] T = [Mode]

VV ---- R

Shift + ;

> px@rbp-0x34

dr =debug register
drr = more details registers
```

![image.png](Reverse%20Engineering%20of%20Second%20Software/image.png)