#pentesting #knowledge-map #moc #navigation

---

# Penetration Testing

## Core Concepts

- [[Penetration-Testing]]
    
- [[Pentesting-Methodology]]
    
- [[Rules-of-Engagement]]
    
- [[Attack-Surface]]
    
- [[Reconnaissance]]
    
- [[Enumeration]]
    
- [[Vulnerability-Assessment]]
    
- [[Exploitation]]
    
- [[Post-Exploitation]]
    
- [[Reporting]]
    

---

# Reconnaissance

## Reconnaissance

- [[Passive-Reconnaissance]]
    
- [[Active-Reconnaissance]]
    
- [[OSINT]]
    
- [[Information-Disclosure]]
    

## DNS & Domains

- [[DNS-Reconnaissance]]
    
- [[Subdomain-Enumeration]]
    

## Identification

- [[Technology-Fingerprinting]]
    

---

# Network Pentesting

## Enumeration

- [[Network-Enumeration]]
    
- [[Port-Scanning]]
    
- [[Service-Enumeration]]
    
- [[Banner-Grabbing]]
    

## Protocols & Services

- [[TCP]]
    
- [[UDP]]
    
- [[DNS]]
    
- [[FTP]]
    
- [[SSH]]
    
- [[SMB]]
    
- [[HTTP]]
    

---

# Web Pentesting

## Fundamentals

- [[Web-Pentesting]]
    
- [[HTTP]]
    
- [[Web-Enumeration]]
    
- [[Directory-Enumeration]]
    
- [[Virtual-Host-Enumeration]]
    

## Authentication & Access Control

- [[Authentication]]
    
- [[Authorization]]
    
- [[Session-Management]]
    
- [[IDOR]]
    

## Input & Injection

- [[Input-Validation]]
    
- [[SQL-Injection]]
    
- [[Cross-Site-Scripting]]
    
- [[Command-Injection]]
    

## File-Based Vulnerabilities

- [[File-Inclusion]]
    
- [[File-Upload]]
    

## Server-Side Attacks

- [[SSRF]]
    

---

# Exploitation

## Vulnerability Research

- [[Exploitation]]
    
- [[Vulnerability-Research]]
    
- [[CVE]]
    
- [[Exploit-DB]]
    
- [[Proof-of-Concepts]]
    

## Payloads & Shells

- [[Payloads]]
    
- [[Shells]]
    
- [[Reverse-Shells]]
    
- [[Bind-Shells]]
    

## Exploit Development

- [[Exploit-Development]]
    

---

# Privilege Escalation

## Fundamentals

- [[Privilege-Escalation]]
    

## Linux

- [[Linux-Privilege-Escalation]]
    
- [[SUID]]
    
- [[Sudo]]
    
- [[Linux-Capabilities]]
    
- [[Cron]]
    
- [[Services]]
    

## Windows

- [[Windows-Privilege-Escalation]]
    
- [[Scheduled-Tasks]]
    
- [[Windows-Privileges]]
    
- [[Services]]
    

---

# Credentials

## Credential Attacks

- [[Credential-Attacks]]
    
- [[Password-Attacks]]
    
- [[Password-Cracking]]
    
- [[Authentication-Attacks]]
    

## Credential Discovery

- [[Hashes]]
    
- [[Credential-Discovery]]
    
- [[Credential-Reuse]]
    

---

# Active Directory

## Fundamentals

- [[Active-Directory]]
    
- [[Domain-Users-and-Groups]]
    
- [[GPO]]
    

## Authentication & Directory Services

- [[LDAP]]
    
- [[Kerberos]]
    
- [[NTLM]]
    
- [[SMB]]
    

## Enumeration & Escalation

- [[AD-Enumeration]]
    
- [[Domain-Privilege-Escalation]]
    

---

# Post-Exploitation

## Host & Environment

- [[Post-Exploitation]]
    
- [[Situational-Awareness]]
    

## Credentials & Access

- [[Credential-Harvesting]]
    
- [[Persistence]]
    

## Movement

- [[Lateral-Movement]]
    
- [[Pivoting]]
    
- [[Tunneling]]
    

---

# Tools

## Reconnaissance & Enumeration

- [[Nmap]]
    
- [[Gobuster]]
    
- [[ffuf]]
    
- [[Nikto]]
    

## Web Pentesting

- [[Burp-Suite]]
    
- [[SQLMap]]
    

## Password Attacks

- [[Hydra]]
    
- [[John-the-Ripper]]
    

## Exploitation

- [[Metasploit]]
    
- [[Msfconsole]]
    
- [[Meterpreter]]
    
- [[Msfvenom]]
    

## Networking

- [[Netcat]]
    

## Active Directory

- [[Impacket]]
    

---

# Cheatsheets

## Pentesting

- [[Pentesting]]
    
- [[Reconnaissance]]
    
- [[Enumeration]]
    
- [[Web-Pentesting]]
    
- [[Linux-Privilege-Escalation]]
    
- [[Windows-Privilege-Escalation]]
    
- [[Active-Directory]]
    

## Tools

- [[Nmap-Cheat]]
    
- [[Gobuster-Cheat]]
    
- [[Burp-Suite-Cheat]]
    
- [[SQLMap-Cheat]]
    
- [[Hydra-Cheat]]
    
- [[John-Ripper-Cheat]]
    
- [[Metasploit-Cheat]]
    

---

# Learning

## TryHackMe

- [[Jr-Penetration-Tester]]
    

---

# Resources

- [[Books-Documents]]
    
- [[Websites]]
    
- [[Glossary]]
    
- [[Methodologies]]
    
- [[Cheat-Sheets]]
    

---

# Walkthroughs

> Room writeups, machine walkthroughs and CTF notes.

---

# Pentesting Workflow

Reconnaissance  
→ [[Passive-Reconnaissance]]  
→ [[Active-Reconnaissance]]  
→ [[Attack-Surface]]

Enumeration  
→ [[Network-Enumeration]]  
→ [[Port-Scanning]]  
→ [[Service-Enumeration]]  
→ [[Web-Enumeration]]

Vulnerability Assessment  
→ [[Vulnerability-Assessment]]  
→ [[Vulnerability-Research]]  
→ [[CVE]]  
→ [[Exploit-DB]]

Exploitation  
→ [[Exploitation]]  
→ [[Payloads]]  
→ [[Shells]]  
→ [[Reverse-Shells]]

Privilege Escalation  
→ [[Linux-Privilege-Escalation]]  
→ [[Windows-Privilege-Escalation]]  
→ [[Domain-Privilege-Escalation]]

Post-Exploitation  
→ [[Situational-Awareness]]  
→ [[Credential-Harvesting]]  
→ [[Lateral-Movement]]  
→ [[Pivoting]]

Reporting  
→ [[Reporting]]

---

# Tool Relationships

Reconnaissance  
→ [[Nmap]]  
→ [[Gobuster]]  
→ [[ffuf]]  
→ [[Nikto]]

Web Pentesting  
→ [[Burp-Suite]]  
→ [[Gobuster]]  
→ [[ffuf]]  
→ [[SQLMap]]

Credential Attacks  
→ [[Hydra]]  
→ [[John-the-Ripper]]

Exploitation  
→ [[Metasploit]]  
→ [[Msfconsole]]  
→ [[Meterpreter]]  
→ [[Msfvenom]]  
→ [[Netcat]]

Active Directory  
→ [[Impacket]]  
→ [[SMB]]  
→ [[LDAP]]  
→ [[Kerberos]]  
→ [[NTLM]]

Privilege Escalation  
→ [[Linux-Privilege-Escalation]]  
→ [[Windows-Privilege-Escalation]]  
→ [[Domain-Privilege-Escalation]]