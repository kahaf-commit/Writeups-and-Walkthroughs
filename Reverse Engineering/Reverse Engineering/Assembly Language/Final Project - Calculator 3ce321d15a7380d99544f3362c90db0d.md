# Final Project - Calculator

## Code from video

```nasm
%include "util.asm"

GLOBAL _start

section .text  

_start:

    mov rdi,num1
    call printstr
    call readint
    mov [user_num1],rax
    
    mov rdi,num2
    call printstr
    call readint
    mov [user_num2],rax
    
    mov rdi,operators
    call printstr
    
    mov rdi,user_operator
    mov rsi,2
    call readstr

cmp_operators:

    mov rdi,[user_operator]
		cmp rdi, 43;+	
		je addition
		cmp rdi, 45
		je substraction;-
	  cmp rdi, 42;*	
		je multiply
		cmp rdi, 47;/
		je division;-
		
exception:

	mov rdi, error_msg	
	call printstr
	call endl   ; for new line
	call exit0

addition:
	mov rdi,[user_num1]
	add rdi,[user_num2]
	call results
		
substraction:
	
	mov rdi,[user_num1]
	sub rdi,[user_num2]
	call results

multiply:
	
	mov rdi,[user_num1]
	imul rdi,[user_num2]
	call results

division:
	
	mov rdx, 0
	mov rax,[user_num1]
	mov rbx,[user_num2]
	idiv rbx
	mov rdi,rax
	call results

results:
	
	call printint
	call endl
	call exit0

section .data

    num1: db "Enter Number 1 : ",0
    num2: db "Enter Number 2 : ",0
    operators: db "Enter operation to perform(+,-,*,/) : ",0
		error_msg: db "Cannot Perform this Calculation. "

section .bss
    user_num1: resb 8            ; প্রথম সংখ্যা রাখার জন্য ৮ বাইট
    user_num2: resb 8            ; দ্বিতীয় সংখ্যা রাখার জন্য ৮ বাইট
    user_operator:   resb 2            ; অপারেটর এবং ট্রেইলিং নিউলাইন রাখার জন্য ২ বাইট

```

```nasm
nasm -f elf64 final_calculator.asm -o final_calculator.o
ld final_calculator.o -o final_calculator
./final_calculator

```

## Corrected one from goole

```nasm
%include "util.asm"

GLOBAL _start

section .text               ; Added text section declaration

_start:
    mov rdi, prompt_num1    ; Fixed: Use the string label from .data
    call printstr
    call readint
    mov [user_num1], rax    ; Saves value into .bss variable

    mov rdi, prompt_num2    ; Fixed: Use the string label from .data
    call printstr
    call readint
    mov [user_num2], rax    ; Saves value into .bss variable

    mov rdi, prompt_operators ; Fixed: Use the string label from .data
    call printstr
    
    mov rdi, user_operators ; Passes the pointer to the buffer
    mov rsi, 2
    call readstr

cmp_operators:
    movzx rdi, byte [user_operators] ; Loads a single byte (character) to compare
    cmp rdi, 43             ; +
    je addition
    cmp rdi, 45             ; -
    je subtraction
    cmp rdi, 42             ; *
    je multiply
    cmp rdi, 47             ; /
    je division
		
exception:
    mov rdi, error_msg	
    call printstr
    call endl   
    call exit0

addition:                   ; Fixed typo: changed 'adddition' to 'addition'
    mov rdi, [user_num1]
    add rdi, [user_num2]
    call results
		
subtraction:
    mov rdi, [user_num1]
    sub rdi, [user_num2]
    call results

multiply:
    mov rdi, [user_num1]
    imul rdi, [user_num2]
    call results

division:
    mov rax, [user_num1]
    cqo                     ; Properly clears/extends sign bits into RDX for idiv
    mov rbx, [user_num2]
    idiv rbx
    mov rdi, rax
    call results

results:
    call printint
    call endl
    call exit0

section .data
    ; Renamed these so they don't clash with your .bss variables
    prompt_num1: db "Enter Number 1 : ",0
    prompt_num2: db "Enter Number 2 : ",0
    prompt_operators: db "Enter operation to perform(+,-,*,/) : ",0
    error_msg: db "Cannot Perform this Calculation",0 ; Added missing null terminator

section .bss
    user_num1: resb 8            ; প্রথম সংখ্যা রাখার জন্য ৮ বাইট
    user_num2: resb 8            ; দ্বিতীয় সংখ্যা রাখার জন্য ৮ বাইট
    user_operators: resb 2      ; অপারেটর এবং ট্রেইলিং নিউলাইন রাখার জন্য ২ বাইট

```

```bash
nasm -f elf64 final_calculator.asm -o final_calculator.o
ld final_calculator.o -o final_calculator
./final_calculator

```