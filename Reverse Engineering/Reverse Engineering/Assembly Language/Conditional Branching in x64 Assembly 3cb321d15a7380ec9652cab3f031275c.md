# Conditional Branching in x64 Assembly

```bash
man ascii
```

```nasm
global _start 

section .text

_start:
    jmp main

main:
    mov rax,0
    mov rdi,0
    mov rsi,user_key
    mov rdx,64
    syscall

cmp_key:
    cmp rax,original_key_len
    jne access_denied
    mov rsi,original_key
    mov rdi,user_key
    mov rcx, original_key_len 
    repe cmpsb
    je access_granted
    jne access_denied

access_granted:
    mov rax,1
    mov rdi,1
    mov rsi,access_granted_mssg
    mov rdx,access_granted_mssg_len
    mov rdx,access_granted_mssg_len
    syscall
    jmp exiting

access_denied:

    mov rax,1
    mov rdi,1
    mov rsi,access_denied_mssg
    mov rdx,access_denied_mssg_len
    syscall

exiting:
    mov rax,60
    mov rdi,0
    syscall

section .data

    access_granted_mssg: db "Access Granted!",10
    access_granted_mssg_len: equ $-access_granted_mssg
    access_denied_mssg: db "Access Denied!",10
    access_denied_mssg_len: equ $-access_denied_mssg
    original_key: db "1789-7654-0987-4532"
	  original_key_len: equ $-original_key
section .bss
    user_key: resb 64

```

```jsx
global _start                      ; লিংকারকে (ld) জানানোর জন্য যে প্রোগ্রামটি এখান থেকে শুরু হবে

section .text

_start:
    jmp main                       ; সরাসরি 'main' লেবেলে চলে যাও

main:
    ; ইউজার ইনপুট নেওয়ার জন্য সিস্টেম কল (Syscall: read)
    mov rax, 0                     ; সিস্টেম কল নাম্বার ০ (sys_read)
    mov rdi, 0                     ; স্ট্যান্ডার্ড ইনপুট ফাইল ডেসক্রিপ্টর (stdin - কিবোর্ড)
    mov rsi, user_key              ; ইনপুট ডাটা যেখানে সেভ হবে সেই বাফারের ঠিকানা
    mov rdx, 64                    ; সর্বোচ্চ ৬৪ বাইট পর্যন্ত ইনপুট নেওয়া হবে
    syscall                        ; কার্নেলকে কল করে ইনপুট সম্পন্ন করো (ইনপুটের সাইজ rax রেজিস্টারে জমা হবে)

cmp_key:
    ; ইনপুট করা কী (Key) এর সাইজ যাচাই করা
    cmp rax, original_key_len      ; ইউজার ইনপুটের সাইজ (rax) আসল কী-এর সাইজের সমান কিনা চেক করো
    jne access_denied              ; সাইজ না মিললে সরাসরি 'access_denied' লেবেলে চলে যাও
    
    ; স্ট্রিং কম্পারিজনের (String Comparison) প্রস্তুতি
    mov rsi, original_key          ; উৎস বা সঠিক কী-এর মেমোরি অ্যাড্রেস rsi তে রাখো
    mov rdi, user_key              ; ইউজারের দেওয়া ইনপুটের মেমোরি অ্যাড্রেস rdi তে রাখো
    mov rcx, original_key_len      ; লুপ কাউন্টার হিসেবে আসল কী-এর সাইজ rcx এ রাখো
    cld                            ; ক্লিয়ার ডিরেকশন ফ্ল্যাগ (স্ট্রিং কম্পারিজন যেন বাম থেকে ডানে বা সামনে অগ্রসর হয়)
    
    ; প্রতিটি ক্যারেক্টার মিলিয়ে দেখা
    repe cmpsb                     ; rcx শূন্য না হওয়া পর্যন্ত এবং ক্যারেক্টারগুলো মিলতে থাকা পর্যন্ত তুলনা করতে থাকো
    je access_granted              ; যদি সবগুলো ক্যারেক্টার হুবহু মিলে যায় (Zero Flag = 1), তবে অ্যাক্সেস দাও
    jne access_denied              ; সামান্য অমিল থাকলেও অ্যাক্সেস রিফিউজ করো

access_granted:
    ; অ্যাক্সেস গ্র্যান্টেড মেসেজ প্রিন্ট করার সিস্টেম কল (Syscall: write)
    mov rax, 1                     ; সিস্টেম কল নাম্বার ১ (sys_write)
    mov rdi, 1                     ; স্ট্যান্ডার্ড আউটপুট ফাইল ডেসক্রিপ্টর (stdout - স্ক্রিন)
    mov rsi, access_granted_mssg   ; স্ক্রিনে দেখানোর মেসেজের মেমোরি অ্যাড্রেস
    mov rdx, access_granted_mssg_len ; মেসেজের দৈর্ঘ্য বা সাইজ
    syscall                        ; কার্নেলকে কল করে মেসেজটি স্ক্রিনে দেখাও
    jmp exiting                    ; মেসেজ দেখানোর পর প্রোগ্রাম শেষ করার জন্য 'exiting' লেবেলে যাও

access_denied:
    ; অ্যাক্সেস ডিনাইড মেসেজ প্রিন্ট করার সিস্টেম কল (Syscall: write)
    mov rax, 1                     ; সিস্টেম কল নাম্বার ১ (sys_write)
    mov rdi, 1                     ; স্ট্যান্ডার্ড আউটপুট ফাইল ডেসক্রিপ্টর (stdout - স্ক্রিন)
    mov rsi, access_denied_mssg    ; স্ক্রিনে দেখানোর মেসেজের মেমোরি অ্যাড্রেস
    mov rdx, access_denied_mssg_len ; মেসেজের দৈর্ঘ্য বা সাইজ
    syscall                        ; কার্নেলকে কল করে মেসেজটি স্ক্রিনে দেখাও

exiting:
    ; প্রোগ্রামটি নিরাপদে বন্ধ বা এক্সিট করার সিস্টেম কল (Syscall: exit)
    mov rax, 60                    ; সিস্টেম কল নাম্বার ৬০ (sys_exit)
    mov rdi, 0                     ; এক্সিট কোড ০ (কোনো এরর বা সমস্যা ছাড়া সফলভাবে শেষ হওয়া বোঝায়)
    syscall                        ; কার্নেলকে কল করে প্রোগ্রামটি বন্ধ করো

section .data
    ; প্রি-ডিফাইনড বা ফিক্সড ডাটা সেকশন
    access_granted_mssg: db "Access Granted!", 10       ; মেসেজ এবং শেষে একটি নিউলাইন (ASCII 10)
    access_granted_mssg_len: equ $ - access_granted_mssg ; এই মেসেজের মোট সাইজ বা ক্যারেক্টার সংখ্যা হিসাব করা

    access_denied_mssg: db "Access Denied!", 10         ; মেসেজ এবং শেষে একটি নিউলাইন (ASCII 10)
    access_denied_mssg_len: equ $ - access_denied_mssg   ; এই মেসেজের মোট সাইজ বা ক্যারেক্টার সংখ্যা হিসাব করা

    original_key: db "1789-7654-0987-4532"             ; সিস্টেমে সেভ থাকা আসল পাসওয়ার্ড বা কী
    original_key_len: equ $ - original_key             ; আসল কী-এর মোট সাইজ হিসাব করা (এখানে ১৯ বাইট)

section .bss
    ; আন-ইনিশিয়ালাইজড ডাটা সেকশন (রানিং টাইমে মেমোরি রির্জাভ করার জন্য)
    user_key: resb 64              ; ইউজারের ইনপুট করা কী সাময়িকভাবে রাখার জন্য ৬৪ বাইট মেমোরি বরাদ্দ করো

```

```jsx
nasm -f elf64 condition.asm -o condition.o
ld condition.o -o condition

./condition
aaaa
Access Denied!

./condition < key
Access Granted!

echo -n '1111111111111111111' | ./condition
Access Denied! 

```