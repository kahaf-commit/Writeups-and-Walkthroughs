# Writeups & Walkthroughs

A collection of hands-on penetration testing notes, lab walkthroughs, and vulnerability writeups — documenting methodology, tools, and remediation across web, Active Directory, and infrastructure security.

**Author:** [MD Jubair Hossain](https://www.linkedin.com/in/jubairbd) · Penetration Tester
**Portfolio:** [Medium](https://medium.com/@muhammadjubairsec) · [LinkedIn](https://www.linkedin.com/in/jubairbd)

---

##  Categories

### Active Directory
| Writeup | What it covers |
|---|---|
| [Lateral Movement and Pivoting](https://github.com/kahaf-commit/Writeups-and-Walkthroughs/tree/main/Lateral%20Movement%20and%20Pivoting) | Hands-on lab work covering lateral movement across compromised hosts and network pivoting techniques — practically executed using tools like Impacket, CrackMapExec, and Chisel/Ligolo for tunneling into segmented networks, with each technique tested and documented step-by-step in a controlled environment. |
|[Exploiting Active Directory](https://github.com/kahaf-commit/Writeups-and-Walkthroughs/tree/main/Exploiting%20Active%20Directory) | Hands-on lab work completed on TryHackMe, covering practical exploitation of Active Directory environments - enumeration, attack path identification, and privilege escalation executed and documented step-by-step in a guided lab setting. |
|[Active Directory Exploitation Notes](https://github.com/kahaf-commit/Writeups-and-Walkthroughs/tree/main/Active%20Directory%20Exploitation) | Structured walkthrough connecting enumeration, lateral movement, and privilege escalation techniques into a single attack chain. |
|[Active Directory CVE Exploitation](https://github.com/kahaf-commit/Writeups-and-Walkthroughs/tree/main/Active%20Directory%20CVE%20Exploitation) | Writeups covering exploitation of known CVEs affecting Active Directory environments — walking through vulnerability identification, proof-of-concept exploitation, and the underlying misconfiguration or patch gap that made each CVE exploitable, along with remediation guidance. |
|[Active Directory Persistence](https://github.com/kahaf-commit/Writeups-and-Walkthroughs/tree/main/Active%20Directory%20Persistence) | Techniques for maintaining long-term access in a compromised AD environment — covering methods like Golden/Silver Ticket abuse, DCSync, and AdminSDHolder manipulation, along with detection and remediation notes.Underlying misconfiguration or patch gap that made each CVE exploitable, along with remediation guidance. |

### Web Application Security
| Writeup | What it covers |
|---|---|
|[Web Application Pentest](https://github.com/kahaf-commit/Writeups-and-Walkthroughs/blob/main/Web%20Application%20Pentest/Web%20Application%20Pentest.md)| Methodology and findings from web application penetration testing exercises, including vulnerability identification and exploitation. |

### Red Team / C2 Infrastructure
| Writeup | What it covers |
|---|---|
|[Adaptix C2 Framework](https://github.com/kahaf-commit/Writeups-and-Walkthroughs/blob/main/Adaptix%20C2%20Framework/Adaptix%20C2.md)  | Building and configuring a C2 framework in an isolated lab, understanding listener/agent architecture relevant to red team infrastructure. |

<!--### 🧩 CTF & Guided Labs (TryHackMe / HackTheBox)
| Writeup | What it covers |
|---|---|
| [Room & machine writeups](./TryHackMe/) | Individual room/machine walkthroughs — enumeration methodology, exploitation steps, and takeaways per target. |

- -->

## Writeup Forma

Each writeup follows a consistent structure so findings are easy to review:

1. **Objective** — what the target/lab/room is and the goal
2. **Recon & Enumeration** — tools and techniques used to gather information
3. **Exploitation** — step-by-step methodology to gain access or extract data
4. **Privilege Escalation / Post-Exploitation** (where applicable)
5. **Remediation** — how the issue would be fixed in a real environment
6. **Lessons Learned**

## Tools Referenced

`BloodHound CE` · `Nmap` · `Burp Suite` · `Impacket` · `AdaptixC2` · `CrackMapExec` · `Mimikatz`

## Disclaimer

All content here is based on authorized lab environments, CTF platforms (TryHackMe, HackTheBox), or self-hosted infrastructure for educational purposes only. None of this reflects unauthorized access to real-world systems.

---

📫 Reach out via [LinkedIn](https://www.linkedin.com/in/jubairbd) or check my [Medium](https://medium.com/@muhammadjubairsec) for full-length articles.
