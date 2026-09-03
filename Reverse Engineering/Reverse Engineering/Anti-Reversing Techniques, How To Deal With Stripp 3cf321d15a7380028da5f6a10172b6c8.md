# Anti-Reversing Techniques, How To Deal With Stripped Binaries

Resources

```bash
https://github.com/Hellsender01/Youtube/blob/main/reverse-engineering/crackme5
```

```bash
mkdir Symbols
cd Symbols
cp ../crackme5.c .
ls
gcc crackme5.c -o crackme5 -no-pie
gcc -s crackme5.c -o crackme5_stripped -no-pie
gcc -ggdb crackme5.c -o crackme5_debugged -no-pie
```

`gcc crackme5.c -o crackme5 -no-pie`
• Compiles the source file into a standard executable with Position Independent Executable (PIE) disabled.`gcc -s crackme5.c -o crackme5_stripped -no-pie`
• Compiles and strips all symbol tables and relocation information from the executable.`gcc -g gdb crackme5.c -o crackme5_debugged -no-pie`
• *Failed command:* Threw an error because `gdb` was mistakenly passed as an argument instead of a flag.`gcc -ggdb crackme5.c -o crackme5_debugged -no-pie`
• Compiles the source file with built-in debugging information specifically optimized for GDB.

## `pwndbg` (/paʊnˈdiˌbʌɡ/) is a GDB and LLDB plug-in that makes debugging suck less, with a focus on features needed by low-level software developers, hardware hackers, reverse-engineers and exploit developers.

```jsx
https://github.com/pwndbg/pwndbg
```

Installations

```jsx
nstall via curl/sh (Linux/macOS).

curl --proto '=https' --tlsv1.2 -LsSf 'https://install.pwndbg.re' | sh -s -- -t pwndbg-gdb

Install via GNU wget/sh (Linux/macOS)

wget --https-only --secure-protocol=TLSv1_2 -qO- 'https://install.pwndbg.re' | sh -s -- -t pwndbg-gdb
```

![image.png](Anti-Reversing%20Techniques,%20How%20To%20Deal%20With%20Stripp/image.png)

![image.png](Anti-Reversing%20Techniques,%20How%20To%20Deal%20With%20Stripp/image%201.png)

```jsx
info files
 
```

![image.png](Anti-Reversing%20Techniques,%20How%20To%20Deal%20With%20Stripp/image%202.png)

```jsx
b * address
OR
```

```jsx
start
```

![image.png](Anti-Reversing%20Techniques,%20How%20To%20Deal%20With%20Stripp/image%203.png)

**GDB** 

```jsx
X/30i $rip
```

![image.png](Anti-Reversing%20Techniques,%20How%20To%20Deal%20With%20Stripp/image%204.png)

```jsx
ni   
ni
ni
ni
```

![image.png](Anti-Reversing%20Techniques,%20How%20To%20Deal%20With%20Stripp/image%205.png)

```jsx
si
```

![image.png](Anti-Reversing%20Techniques,%20How%20To%20Deal%20With%20Stripp/image%206.png)

```nasm
ni
ni
ni
ni
to find call rax
```

![image.png](Anti-Reversing%20Techniques,%20How%20To%20Deal%20With%20Stripp/image%207.png)

```nasm
si = vetor a jao/ go inside
```

In pwndebug breakpoint add

```nasm
bp address
```

```nasm
x/20i $rip
x/30i $rip
```

The command `x/20i` is used in **pwndbg** to examine memory and decompile it into human-readable assembly instructions.
**What it stands for**
• **`x`**: The "examine" command in GDB.
• **`/`**: Introduces the formatting options.
• **`20`**: The number of units to display (in this case, 20 instructions).
• **`i`**: The format type, which stands for **instructions** (disassembled machine code).
****

### Program end = ret functions

![image.png](Anti-Reversing%20Techniques,%20How%20To%20Deal%20With%20Stripp/image%208.png)

![image.png](Anti-Reversing%20Techniques,%20How%20To%20Deal%20With%20Stripp/image%209.png)

examine / nearpc same

**In ghidra arguments pass**

![image.png](Anti-Reversing%20Techniques,%20How%20To%20Deal%20With%20Stripp/image%2010.png)