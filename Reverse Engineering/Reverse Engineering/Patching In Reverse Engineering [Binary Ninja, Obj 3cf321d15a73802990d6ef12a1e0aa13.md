# Patching In Reverse Engineering [Binary Ninja, Objdump]

**Resource**

```bash
https://github.com/Hellsender01/Youtube/blob/main/reverse-engineering/crackme4
```

**Tools = binaryninja**

```bash
https://binary.ninja/free/
```

![image.png](Patching%20In%20Reverse%20Engineering%20%5BBinary%20Ninja,%20Obj/image.png)

![image.png](Patching%20In%20Reverse%20Engineering%20%5BBinary%20Ninja,%20Obj/image%201.png)

## Objdump

```bash
                                                                                
┌──(kali㉿kali)-[~]
└─$ objdump -D crackme4 -M intel --disassemble=main

```

![image.png](Patching%20In%20Reverse%20Engineering%20%5BBinary%20Ninja,%20Obj/image%202.png)

```bash
google = x64 to je op code
```

![image.png](Patching%20In%20Reverse%20Engineering%20%5BBinary%20Ninja,%20Obj/image%203.png)

**Hex Editor = ghex  / hexedit**

![image.png](Patching%20In%20Reverse%20Engineering%20%5BBinary%20Ninja,%20Obj/image%204.png)

![image.png](Patching%20In%20Reverse%20Engineering%20%5BBinary%20Ninja,%20Obj/image%205.png)