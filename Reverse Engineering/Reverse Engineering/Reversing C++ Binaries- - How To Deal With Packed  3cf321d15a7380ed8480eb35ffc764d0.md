# Reversing C++ Binaries-*- How To Deal With Packed Binaries

Resources

```nasm
https://www.youtube.com/watch?v=dyaXk0X4Mf8&list=PL-DxAN1jsRa9151ezNuCbh7UkGS0bMPdw&index=18
```

```nasm
https://github.com/Hellsender01/Youtube/blob/main/reverse-engineering/crackme7
```

```nasm
chmod
info functions
disassemble main

```

```nasm
┌──(kali㉿kali)-[~]
└─$ file crackme7
crackme7: ELF 64-bit LSB executable, x86-64, version 1 (GNU/Linux), statically linked, no section header

```

## checksec comes with pwntools

```nasm
sudo pip3 install pwntools
```

```nasm
──(kali㉿kali)-[~]
└─$ checksec --file=./crackme7    
[*] '/home/kali/crackme7'
    Arch:       amd64-64-little
    RELRO:      No RELRO
    Stack:      No canary found
    NX:         NX enabled
    PIE:        No PIE (0x400000)
    Packer:     Packed with UPX

```

```nasm
upx -d crackme7 -o crackme7_unpacked
info functions
disassemble main
b main
r

```

# **Commands Extraction**

- `rev unhex 706d552267616b4c`
    - Reverses and decodes a hex string.
- `rev unhex 2369`
    - Reverses and decodes another hex string.
- `rev python3`
    - Starts the Python 3 interactive interpreter.
- **Python Interactive Script:**
    
    **python**
    
    ```
    string ='Lkag"Umpi#'foriin string:
        print(chr(ord(i)^2))
    ```
    
    Use code with caution.
    
    - Decrypts the target string by performing a bitwise XOR operation with `2` on each character.
- `./crackme7`
    - Runs the compiled `crackme7` binary executable.

---

# **Decrypted Password**

By calculating the output of the Python script shown in the terminal (e.g., `L^2 = N`, `k^2 = i`, `a^2 = c`), the complete password resolves to:

**`NiceWoRk!`**