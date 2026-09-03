# Loops and Calling Convention in x64 Assembly

```bash
wget https://raw.githubusercontent.com/kahaf-commit/util.asm/refs/heads/master/util.asm
```

```bash
%include "util.asm"

GLOBAL _start

section .text

_start:
    mov rdi,mssg
    call printstr
    call readint ;2
    mov [user_value],rax
    mov rbx,1

LOOP_START:
    mov rcx,[user_value]
    imul rcx,rbx
    mov rdi,rcx
    call printint
    call endl
    add rbx,1
    cmp rbx,11
    jne LOOP_START
    call exit0

section .data
    mssg: db "Enter Number : ",0

section .bss
    user_value: resb 8 ;2

```

```bash
nasm -f elf64 loop.asm -o loop.o
ld loop.o -o loop
./loop

```