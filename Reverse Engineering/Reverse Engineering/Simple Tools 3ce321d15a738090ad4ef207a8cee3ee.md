# Simple Tools

**file command**

**ldd program.elf = libc files show**

![image.png](Simple%20Tools/image.png)

**not stripped/ stripped = symbols**

**making stripped and non-stripped**

![image.png](Simple%20Tools/image%201.png)

**Compare it [ tool = diff also use for cmp]**

![image.png](Simple%20Tools/image%202.png)

```bash
tool = nm crackme
```

![image.png](Simple%20Tools/image%203.png)

**stored password = strings (printable)**

```bash
tools = strings
```

**another**

```bash
cat crackme
```

not run = static analysis- idb

run = dynamic analysis- r2,gdb

dynaic = ltrace

```bash
ltrace ./crackme2  
```

![image.png](Simple%20Tools/image%204.png)

```bash
strace == kon kon systemcall kaj korche / run korche
```

```bash
readelf -a crackme 
```

![image.png](Simple%20Tools/image%205.png)