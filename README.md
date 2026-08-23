# Writeups & Walkthroughs

A collection of hands-on penetration testing notes, lab walkthroughs, and vulnerability writeups — documenting methodology, tools, and remediation across web, Active Directory, and infrastructure security.

**Author:** [MD Jubair Hossain](https://www.linkedin.com/in/jubairbd) · Penetration Tester
**Portfolio:** [Medium](https://medium.com/@jubairbd) · [LinkedIn](https://www.linkedin.com/in/jubairbd)

---

## 📂 Categories

### Active Directory
| Writeup | Summary |
|---|---|
| [AD Enumeration & Exploitation with BloodHound](./Active-Directory/AD-Enumeration-BloodHound.md) | Mapping attack paths in an AD environment using BloodHound CE, from initial enumeration to privilege escalation. |
| [TryHackMe – Exploiting Active Directory](./Active-Directory/THM-Exploiting-AD.md) | Structured walkthrough of AD attack paths: enumeration, lateral movement, and privilege escalation techniques. |

### Web Application Security
| Writeup | Summary |
|---|---|
| [XXE → SSRF Chain: From XML Parsing Flaw to Internal Network Access](./Web-Exploitation/XXE-SSRF-Chain.md) | How an XML External Entity injection was chained with SSRF to reach internal APIs and cloud metadata endpoints, plus remediation. |

### Red Team / C2 Infrastructure
| Writeup | Summary |
|---|---|
| [AdaptixC2 — Installation and Initial Configuration](./Red-Team/AdaptixC2-Setup.md) | Setting up and configuring the AdaptixC2 framework in a lab environment for C2 infrastructure practice. |

### TryHackMe Rooms
| Writeup | Summary |
|---|---|
| [Room writeups index](./TryHackMe/) | Individual room walkthroughs — enumeration steps, exploitation, and key takeaways. |

---

## 🧭 Writeup Format

Each writeup follows a consistent structure so findings are easy to review:

1. **Objective** — what the target/lab/room is and the goal
2. **Recon & Enumeration** — tools and techniques used to gather information
3. **Exploitation** — step-by-step methodology to gain access or extract data
4. **Privilege Escalation / Post-Exploitation** (where applicable)
5. **Remediation** — how the issue would be fixed in a real environment
6. **Lessons Learned**

## 🛠️ Tools Referenced

`BloodHound CE` · `Nmap` · `Burp Suite` · `Impacket` · `AdaptixC2` · `CrackMapExec` · `Mimikatz`

## ⚠️ Disclaimer

All content here is based on authorized lab environments, CTF platforms (TryHackMe, HackTheBox), or self-hosted infrastructure for educational purposes only. None of this reflects unauthorized access to real-world systems.

---

📫 Reach out via [LinkedIn](https://www.linkedin.com/in/jubairbd) or check my [Medium](https://medium.com/@jubairbd) for full-length articles.
