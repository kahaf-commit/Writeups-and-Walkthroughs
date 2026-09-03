# Calculate Basic Math

```nasm
global main

section .text

main:
    mov rax, 2
    add rax, 3
    
    mov rax, 2
    sub rax, 2
    
    mov rax, 6
    imul rbx, rax, 2
    
    mov rdx,0
    mov rax, 100
    mov rbx, 2
    idiv rbx

_exit:
    mov rax, 60
    mov rdi, 0
    syscall

```

```nasm
nasm -f elf64 math.asm -o math.o
ld math.o -o math
```

![image.png](Calculate%20Basic%20Math/image.png)

```bash
ld -e math.o -o math
```

![image.png](Calculate%20Basic%20Math/image%201.png)

# Debugger

## cutter

```bash
git clone https://github.com/rizinorg/cutter.git

┌──(kali㉿kali)-[~]
└─$ cutter
Command 'cutter' not found, but can be installed with:
sudo apt install rizin-cutter
Do you want to install it? (N/y)n

```

**then releases**

```bash
┌──(kali㉿kali)-[~/Downloads]
└─$ sudo mv Cutter-v2.5.0-Linux-x86_64.AppImage /opt/cutter
[sudo] password for kali: 
 
┌──(kali㉿kali)-[/opt]
└─$ chmod +x cutter      
                                                                                
┌──(kali㉿kali)-[/opt]
└─$ ./cutter    

```

![image.png](Calculate%20Basic%20Math/image%202.png)

![image.png](Calculate%20Basic%20Math/image%203.png)