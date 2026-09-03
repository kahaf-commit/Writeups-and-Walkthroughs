# Basic Knowledge

**Registers**

![2026-08-27 23_00_15-Screenshot (471).png.png](Basic%20Knowledge/2026-08-27_23_00_15-Screenshot_(471).png.png)

![2026-08-27 22_59_41-Screenshot (470).png.png](Basic%20Knowledge/2026-08-27_22_59_41-Screenshot_(470).png.png)

![2026-08-27 23_03_27-Screenshot (473).png.png](Basic%20Knowledge/2026-08-27_23_03_27-Screenshot_(473).png.png)

![2026-08-27 23_03_12-Screenshot (472).png.png](Basic%20Knowledge/2026-08-27_23_03_12-Screenshot_(472).png.png)

![2026-08-27 23_03_39-Screenshot (474).png.png](Basic%20Knowledge/2026-08-27_23_03_39-Screenshot_(474).png.png)

![2026-08-27 22_00_44-[PRACTICAL]What are System Calls__[HINDI] - YouTube.png](Basic%20Knowledge/2026-08-27_22_00_44-PRACTICALWhat_are_System_Calls__HINDI_-_YouTube.png)

**Open a system call C program**

![2026-08-27 22_03_03-[PRACTICAL]What are System Calls__[HINDI] - YouTube.png](Basic%20Knowledge/2026-08-27_22_03_03-PRACTICALWhat_are_System_Calls__HINDI_-_YouTube.png)

**Code**

```jsx
int main() {
    write(1, "hello", 5); 
    exit(11); 
}

write সিস্টেম কল
: স্ক্রিনে কোনো কিছু দেখানোর জন্য বা প্রিন্ট করার জন্য এটি ব্যবহার করা হয়
। এর জন্য তিনটি জিনিস বা আর্গুমেন্ট উল্লেখ করতে হয়
:
১ (Standard Output): প্রথম আর্গুমেন্ট হিসেবে 1 ব্যবহার করা হয়েছে। এটি দিয়ে স্ট্যান্ডার্ড আউটপুট বোঝায়, যার অর্থ লেখাটি স্ক্রিনে বা মনিটরে দেখানো হবে
।
"hello" (Buffer): দ্বিতীয় আর্গুমেন্ট হিসেবে যে ডেটা বা টেক্সটটি আমরা স্ক্রিনে দেখাতে চাই (বাফার) তা দেওয়া হয়েছে
।
৫ (Count/Length): তৃতীয় আর্গুমেন্টটি হচ্ছে লেখার দৈর্ঘ্য। যেহেতু "hello" শব্দটিতে ৫টি অক্ষর আছে, তাই এর দৈর্ঘ্য দেওয়া হয়েছে 5
।
exit সিস্টেম কল
:
এটি প্রোগ্রাম থেকে বের হয়ে যাওয়ার জন্য বা প্রোগ্রামটি শেষ করার জন্য ব্যবহার করা হয়
। এটি একটি নির্দিষ্ট স্ট্যাটাস কোড বা নম্বর (এখানে যেমন 11) রিটার্ন করে
। যদি প্রোগ্রামটি কোনো ত্রুটি ছাড়া সফলভাবে রান করে, তবে এটি এই নির্দিষ্ট স্ট্যাটাস কোডটি প্রদর্শন করবে
।
সি প্রোগ্রামিংয়ে সরাসরি write বা exit নাম ব্যবহার করে সিস্টেম কল করা গেলেও অ্যাসেম্বলি (Assembly) ল্যাঙ্গুয়েজের ক্ষেত্রে সিস্টেম কলের নির্দিষ্ট নম্বর (যেমন: write সিস্টেম কলের জন্য ১ নম্বর) ব্যবহার করে কার্নেলকে নির্দেশ দিতে হয়
।‌
```

**Systemcall in Linux**

![2026-08-27 23_17_01-[PRACTICAL]What are System Calls__[HINDI] - YouTube.png](Basic%20Knowledge/2026-08-27_23_17_01-PRACTICALWhat_are_System_Calls__HINDI_-_YouTube.png)

```bash
cat  /usr/include/x86_64-linux-gnu/asm/unistd_64.h
```

**InLinux ,**

```jsx
man 2 write ; 2 means system call; note= run it in linux
```

fd = file descriptor

In Linux, there r 3 fd: STDIN=0, STDOUT=1, STDERR=2

**Syscalls Example code:**

```c
int main() {
    write(1, "hello", 5); 
    exit(11); 
}
```

**Compile , run & any err or not**

```bash
gcc syscalls.c -o syscalls
./syscalls
echo $?
```

# **Instructions**

CMP = compare

**Jeta check korte hobe seta = SOURCE**

**Jetar sathe check/compare korte hobe = DESTINATION**

PASSWORD INPUT = SOURCE

STORED PASSWORD = DESTINATION

ZF = Zero Flag

![2026-08-27 23_59_33-[PRACTICAL]Assembly For Reverse Engineering[HINDI] - YouTube.png](Basic%20Knowledge/2026-08-27_23_59_33-PRACTICALAssembly_For_Reverse_EngineeringHINDI_-_YouTube.png)

![2026-08-28 00_07_00-[PRACTICAL]Assembly For Reverse Engineering[HINDI] - YouTube.png](Basic%20Knowledge/2026-08-28_00_07_00-PRACTICALAssembly_For_Reverse_EngineeringHINDI_-_YouTube.png)

![2026-08-27 23_46_50-[PRACTICAL]Assembly For Reverse Engineering[HINDI] - YouTube.png](Basic%20Knowledge/2026-08-27_23_46_50-PRACTICALAssembly_For_Reverse_EngineeringHINDI_-_YouTube.png)

![2026-08-27 23_47_11-[PRACTICAL]Assembly For Reverse Engineering[HINDI] - YouTube.png](Basic%20Knowledge/2026-08-27_23_47_11-PRACTICALAssembly_For_Reverse_EngineeringHINDI_-_YouTube.png)

![2026-08-27 23_50_16-[PRACTICAL]Assembly For Reverse Engineering[HINDI] - YouTube.png](Basic%20Knowledge/2026-08-27_23_50_16-PRACTICALAssembly_For_Reverse_EngineeringHINDI_-_YouTube.png)

![2026-08-27 23_53_06-[PRACTICAL]Assembly For Reverse Engineering[HINDI] - YouTube.png](Basic%20Knowledge/2026-08-27_23_53_06-PRACTICALAssembly_For_Reverse_EngineeringHINDI_-_YouTube.png)

**TEST = Value 0 or not, just check it**

**Value er man 0 Hole True,  So ,  ZF = 1**

![2026-08-28 00_20_31-[PRACTICAL]Assembly For Reverse Engineering[HINDI] - YouTube.png](Basic%20Knowledge/2026-08-28_00_20_31-PRACTICALAssembly_For_Reverse_EngineeringHINDI_-_YouTube.png)

JMP = JUMP

JMP = [DESTINATION]

This instruction is **Unconditional**

Means if an instruction comes, then do, no more talk

![2026-08-28 00_35_49-[PRACTICAL]Assembly For Reverse Engineering[HINDI] - YouTube.png](Basic%20Knowledge/2026-08-28_00_35_49-PRACTICALAssembly_For_Reverse_EngineeringHINDI_-_YouTube.png)

**Conditional jump**

![2026-08-28 00_39_37-[PRACTICAL]Assembly For Reverse Engineering[HINDI] - YouTube.png](Basic%20Knowledge/2026-08-28_00_39_37-PRACTICALAssembly_For_Reverse_EngineeringHINDI_-_YouTube.png)

![2026-08-28 00_40_17-[PRACTICAL]Assembly For Reverse Engineering[HINDI] - YouTube.png](Basic%20Knowledge/2026-08-28_00_40_17-PRACTICALAssembly_For_Reverse_EngineeringHINDI_-_YouTube.png)

![2026-08-28 00_43_21-[PRACTICAL]Assembly For Reverse Engineering[HINDI] - YouTube.png](Basic%20Knowledge/2026-08-28_00_43_21-PRACTICALAssembly_For_Reverse_EngineeringHINDI_-_YouTube.png)

![2026-08-28 00_44_12-[PRACTICAL]Assembly For Reverse Engineering[HINDI] - YouTube.png](Basic%20Knowledge/2026-08-28_00_44_12-PRACTICALAssembly_For_Reverse_EngineeringHINDI_-_YouTube.png)

**CALL = execute , then jekhan theke asche sekhan a chole jabe again**

**LOOP type**

![image.png](Basic%20Knowledge/image.png)

**RET = RETURN**

**Last CALL a Redirect kore dibe**

![image.png](Basic%20Knowledge/image%201.png)

![image.png](Basic%20Knowledge/image%202.png)

**SYSCALL**

![image.png](Basic%20Knowledge/image%203.png)

![image.png](Basic%20Knowledge/image%204.png)

![image.png](Basic%20Knowledge/image%205.png)