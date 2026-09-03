# Write Hello World in Assembly Language

## **Syscall value**

```bash
cat  /usr/include/x86_64-linux-gnu/asm/unistd_64.h
```

**3 Section = Text(Code),  Data(Variable/ Initialized) , BSS(User Input/ uninitialized)** 

```nasm
global _start

section .text
_start:
	; print hello world
	 mov rax,1 ;write syscall
	 mov rdi,1 ;fd -> 1 (output)
	 mov rsi,hello ;buff -> hello -> 'hello world'
	 mov rdx,11 ;count -> size / data length=11
	 syscall
	 
	 ; exit syscall
	 mov rax,60 ;exit syscall
	 mov rdi,22 ;status code -> 22 
	 syscall	
	  
section .data
	hello: db 'hello world'
```

## Install an assembler

```bash
apt install nasm
nasm -f elf64 helloword.asm -o helloworld.o
chmod +x helloworld.o
ld helloworld.o -o helloworld

file helloworld (check)
./helloworld
```

.o = object file

ld = linker ( .o file er sathe necessery aro file add kore diye run koray)

![Screenshot From 2026-08-27 16-34-23.png](Write%20Hello%20World%20in%20Assembly%20Language/Screenshot_From_2026-08-27_16-34-23.png)

```nasm
section .bss
    ; এখানে আন-ইনিশিয়ালাইজড ডেটা বা ভেরিয়েবল ঘোষণা করা হয়
    buffer: resb 64    ; যেমন: ইউজার ইনপুট নেওয়ার জন্য ৬৪ বাইটের একটি খালি বাফার
    
    
```

`.data` সেকশনে যেমন আপনি আগে থেকেই মান নির্দিষ্ট করে দেন (যেমন: `'hello world'`), `.bss` সেকশনে কোনো মান দেওয়া যায় না। এটি শুধু মেমরিতে জায়গা বুক (reserve) করে রাখে।২. **কীভাবে ঘোষণা করতে হয়?**

`resb` (Reserve Byte) — বাইট বুক করার জন্য।`resw` (Reserve Word) — ২ বাইট বুক করার জন্য।`resd` (Reserve Double Word) — ৪ বাইট বুক করার জন্য।৩. **ফাইলের সাইজ ছোট রাখে:** `.bss` সেকশনে রাখা ভেরিয়েবলগুলো ফাইনাল এক্সিকিউটেবল ফাইলের সাইজ বাড়ায় না। প্রোগ্রামটি যখন র‍্যামে (RAM) লোড হয়, কেবল তখনই এই জায়গাগুলো তৈরি হয় এবং স্বয়ংক্রিয়ভাবে সেগুলোর মান `0` (Zero) সেট হয়ে যায়।

## **Working with user input**

```nasm
global _start

section .text
_start:
	mov rax,1
	mov rdi,1
	mov rsi,hello
	mov rdx,hello_length
	syscall
	 
	mov rax,60
	mov rdi,22
	syscall	
	  
section .data
	hello: db 'Enter Your Name : '
	hello_length: equ $-hello

```

```nasm
global _start

section .text
_start:
    ; ১. স্ক্রিনে মেসেজ প্রিন্ট করা (Write)
    mov rax, 1
    mov rdi, 1
    mov rsi, welcome_message
    mov rdx, welcome_length
    syscall

user_input: 
		; a lable ২. ইউজারের কাছ থেকে ইনপুট নেওয়া (Read) -> এখানে .bss সেকশনের বাফার ব্যবহার হচ্ছে
    mov rax, 0          ; sys_read (ইনপুট নেওয়ার সিস্টেম কল নম্বর ০)
    mov rdi, 0          ; fd -> 0 (Standard Input / কীবোর্ড)
    mov rsi, input  ; .bss সেকশনে তৈরি করা খালি জায়গার অ্যাড্রেস
    mov rdx, 32         ; সর্বোচ্চ ৩২ বাইট পর্যন্ত ইনপুট নেওয়া যাবে
    syscall
    mov rbx,rax
    
 printing_hello:
	 mov rax, 1
	 mov rdi, 1
	 mov rsi, hello
	 mov rdx, hello_length
	 syscall
	 
printing_userinput:
	mov rax, 1
	mov rdi, 1
	mov rsi, input
	mov rdx, rbx
	syscall
	 
exiting_program: 
		; a lable ৩. প্রোগ্রামটি সফলভাবে শেষ করা (Exit)
    mov rax, 60
    mov rdi, 22
    syscall

section .data
    welcome_message: db 'Enter Your Name : '
    welcome_length: equ $-welcome_message
    hello: db 'hello, '
    hello_length: equ $-hello

section .bss
    input: resb 32  ; Reserve byte ইউজারের নাম জমা রাখার জন্য ৩২ বাইটের একটি খালি জায়গা (Buffer)

```

![image.png](Write%20Hello%20World%20in%20Assembly%20Language/image.png)