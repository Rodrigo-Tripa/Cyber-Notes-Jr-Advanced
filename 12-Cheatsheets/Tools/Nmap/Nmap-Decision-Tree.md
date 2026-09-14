# Nmap Decision Tree

> Decision tree for **Nmap-based reconnaissance and enumeration**, focused on CTFs and authorised penetration tests.

The purpose of this document is to provide a practical decision-making process for using **Nmap during reconnaissance and enumeration**. Instead of memorising commands independently, use the results of each scan to determine the next appropriate action.

---

## Index

- [[#1. Start — Identify the Target|Start]]
    
- [[#2. Is the Host Reachable?|Host Discovery]]
    
- [[#3. Port Scanning|Port Scanning]]
    
- [[#4. Did You Find Open Ports?|Open Ports]]
    
- [[#5. Service Enumeration|Service Enumeration]]
    
- [[#6. UDP Enumeration|UDP]]
    
- [[#7. Firewall or Filtering|Filtering]]
    
- [[#8. Unknown or Unusual Ports|Unknown Services]]
    
- [[#9. Web Services|HTTP / HTTPS]]
    
- [[#10. Common Network Services|Common Services]]
    
- [[#11. NSE|NSE]]
    
- [[#12. OS Detection|OS Detection]]
    
- [[#13. Complete CTF Workflow|CTF Workflow]]
    
- [[#14. Pentest Workflow|Pentest Workflow]]
    
- [[#15. Troubleshooting|Troubleshooting]]
    
- [[#16. Final Enumeration Checklist|Checklist]]

---

# 1. Start — Identify the Target

Start with the target provided by the CTF, lab, or authorised assessment.

```
TARGET
  │
  ├── Single host
  │      └── <TARGET>
  │
  ├── Network
  │      └── <NETWORK/CIDR>
  │
  └── Multiple targets
         └── targets.txt
```

### Single Target

```
nmap <TARGET>
```

### Network

```
nmap -sn <NETWORK/CIDR>
```

### Target List

```
nmap -iL targets.txt
```

Before continuing, determine whether the objective is:

|Objective|Priority|
|---|---|
|CTF machine|Discover complete attack surface|
|External pentest|Identify externally exposed services|
|Internal pentest|Discover hosts and services|
|Network inventory|Identify active systems|
|Service investigation|Focus on specific ports/services|

---

# 2. Is the Host Reachable?

The first decision is whether Nmap considers the host online.

```
                    TARGET
                      │
                      ▼
                Host Discovery
                      │
              ┌───────┴───────┐
              │               │
            UP?            DOWN?
              │               │
              ▼               ▼
         Scan ports         Try -Pn
                              │
                              ▼
                         Scan anyway
```

## Normal Discovery

```
nmap -sn <TARGET>
```

For a network:

```
nmap -sn 10.10.10.0/24
```

If the target is discovered:

```
HOST UP
  ↓
Proceed to port scanning
```

If Nmap reports:

```
Host seems down
```

do not immediately conclude that the host is offline.

Firewalls can block discovery probes.

Try:

```
nmap -Pn <TARGET>
```

Then continue with port scanning.

---

# 3. Port Scanning

Once the host is considered reachable, determine which ports are accessible.

```
                 HOST
                  │
                  ▼
            Port Scan
                  │
        ┌─────────┴─────────┐
        │                   │
     TCP scan           UDP scan
        │                   │
        ▼                   ▼
    Open TCP            Open UDP
        │                   │
        └─────────┬─────────┘
                  ▼
          Service Enumeration
```

## First TCP Scan

```
nmap -T4 <TARGET>
```

This provides a quick initial overview.

However, for a CTF, do not stop here.

## Full TCP Scan

```
sudo nmap -p- -T4 <TARGET>
```

If speed is important:

```
sudo nmap -p- -T4 --min-rate 1000 <TARGET>
```

The objective is to obtain a complete list of accessible TCP ports.

---

# 4. Did You Find Open Ports?

After the scan:

```
PORT     STATE
22/tcp   open
80/tcp   open
445/tcp  open
```

The next step is **not exploitation**.

The next step is:

```
OPEN PORT
    ↓
Identify service
    ↓
Identify version
    ↓
Enumerate service
    ↓
Research attack surface
```

Run:

```
nmap -sC -sV -p 22,80,445 <TARGET>
```

For a large list:

```
nmap -sC -sV -p <PORTS> <TARGET>
```

---

# 5. Service Enumeration

The main question after discovering a port is:

> What is actually running on this port?

Example:

```
22/tcp   open ssh
80/tcp   open http
8080/tcp open http-proxy
```

Treat the Nmap service name as a **starting point**, not absolute truth.

A service can run on any port.

For example:

```
31337/tcp open elite
```

does not necessarily mean that the actual application is "Elite".

Investigate it:

```
nmap -sV -p 31337 <TARGET>
```

Then manually connect if appropriate:

```
nc <TARGET> 31337
```

---

# 6. UDP Enumeration

TCP is not the entire attack surface.

```
Full TCP scan
      │
      ▼
Any interesting UDP services?
      │
      ├── Unknown
      │
      └── Not checked
             │
             ▼
        UDP scan
```

Start with common UDP ports:

```
sudo nmap -sU --top-ports 100 <TARGET>
```

For more coverage:

```
sudo nmap -sU --top-ports 1000 <TARGET>
```

Only perform a complete UDP scan when necessary:

```
sudo nmap -sU -p- <TARGET>
```

## UDP Result

If you see:

```
161/udp open|filtered snmp
```

do not assume SNMP is definitely accessible.

Investigate further:

```
sudo nmap -sU -sV -p 161 <TARGET>
```

Then use service-specific enumeration.

---

# 7. Firewall or Filtering

If results contain many:

```
filtered
```

or:

```
open|filtered
```

consider whether a firewall or packet filter is affecting the scan.

```
             PORT SCAN
                 │
                 ▼
          Many FILTERED?
                 │
          ┌──────┴──────┐
          │             │
         NO            YES
          │             │
          ▼             ▼
 Continue             Investigate
                      filtering
```

Try:

```
sudo nmap -Pn <TARGET>
```

Use an ACK scan to investigate filtering:

```
sudo nmap -sA -p- <TARGET>
```

You can also inspect the reasoning:

```
nmap --reason <TARGET>
```

For specialised environments, investigate:

```
sudo nmap -sF <TARGET>
sudo nmap -sN <TARGET>
sudo nmap -sX <TARGET>
```

These scans should be considered specialised techniques rather than replacements for normal SYN scanning.

---

# 8. Unknown or Unusual Ports

An unusual port is not automatically unimportant.

Examples:

```
31337/tcp open
8000/tcp  open
8080/tcp open
9001/tcp open
49152/tcp open
```

Follow:

```
UNKNOWN PORT
     │
     ▼
-sV
     │
     ▼
Did Nmap identify it?
     │
 ┌───┴────┐
 │        │
 YES      NO
 │        │
 ▼        ▼
Version   Manual
          investigation
             │
             ▼
        nc / curl / browser
             │
             ▼
        NSE / fingerprinting
```

Run:

```
nmap -sV -p <PORT> <TARGET>
```

If still unknown:

```
nmap -sV --version-all -p <PORT> <TARGET>
```

Then consider:

```
nc <TARGET> <PORT>
```

For HTTP-like behaviour:

```
curl -i http://<TARGET>:<PORT>/
```

---

# 9. Web Services

If you find:

```
80/tcp
443/tcp
8000/tcp
8080/tcp
8443/tcp
```

treat them as potential web services.

```
OPEN HTTP PORT
      │
      ▼
-sC -sV
      │
      ▼
HTTP enumeration
      │
      ├── Title
      ├── Headers
      ├── Methods
      ├── Server
      └── Technologies
```

Run:

```
nmap -sC -sV -p 80 <TARGET>
```

Useful NSE:

```
nmap --script http-title -p 80 <TARGET>
```

```
nmap --script http-headers -p 80 <TARGET>
```

```
nmap --script http-methods -p 80 <TARGET>
```

```
nmap --script http-enum -p 80 <TARGET>
```

For multiple web ports:

```
nmap --script http-title,http-headers,http-methods -p 80,443,8080 <TARGET>
```

Then continue outside Nmap with web enumeration.

---

# 10. Common Network Services

After identifying a service, switch from generic Nmap scanning to **service-specific enumeration**.

|Service|Port|First Action|
|---|---|---|
|FTP|21|`-sC -sV`, check anonymous access|
|SSH|22|`-sC -sV`, identify version|
|Telnet|23|`-sC -sV`, manual connection|
|SMTP|25|`-sC -sV`, enumerate capabilities|
|DNS|53|`-sC -sV`, DNS-specific enumeration|
|HTTP|80|HTTP NSE + browser|
|POP3|110|`-sC -sV`|
|RPC|111|`-sC -sV`|
|IMAP|143|`-sC -sV`|
|SNMP|161/UDP|UDP + SNMP enumeration|
|LDAP|389|`-sC -sV`|
|HTTPS|443|TLS/HTTP enumeration|
|SMB|139/445|SMB NSE|
|RDP|3389|`-sC -sV`|
|MySQL|3306|`-sC -sV`|
|PostgreSQL|5432|`-sC -sV`|
|Redis|6379|`-sC -sV`|
|WinRM|5985/5986|`-sC -sV`|

---

# 11. NSE

Once services have been identified, decide whether NSE can provide additional information.

```
SERVICE FOUND
     │
     ▼
Is there a useful NSE script?
     │
 ┌───┴────┐
 │        │
 YES      NO
 │        │
 ▼        ▼
Run NSE   Manual
          enumeration
```

## Default Scripts

```
nmap -sC -sV -p <PORTS> <TARGET>
```

## Vulnerability Scripts

```
nmap --script vuln -p <PORTS> <TARGET>
```

## Service-Specific

HTTP:

```
nmap --script http-* -p 80,443 <TARGET>
```

SMB:

```
nmap --script smb-* -p 139,445 <TARGET>
```

FTP:

```
nmap --script ftp-* -p 21 <TARGET>
```

SSH:

```
nmap --script ssh-* -p 22 <TARGET>
```

DNS:

```
nmap --script dns-* -p 53 <TARGET>
```

Do not blindly execute every script category. NSE contains scripts with different levels of intrusiveness.

---

# 12. OS Detection

OS detection is useful when the target's operating system is not already obvious.

```
TARGET
  │
  ▼
Enough TCP information?
  │
  ├── YES
  │    └── OS detection
  │
  └── NO
       └── Continue enumeration
```

Run:

```
sudo nmap -O <TARGET>
```

Combined:

```
sudo nmap -O -sV -p <PORTS> <TARGET>
```

If Nmap is uncertain:

```
sudo nmap -O --osscan-guess <TARGET>
```

Remember that OS detection is fingerprinting. Treat the result as evidence, not unquestionable fact.

---

# 13. Complete CTF Workflow

This is the default workflow for a new CTF target.

```
                     START
                       │
                       ▼
                 Identify Target
                       │
                       ▼
                 Quick Nmap Scan
                       │
                       ▼
                Ports discovered?
                  │           │
                 NO          YES
                  │           │
                  ▼           ▼
                 -Pn       Full TCP
                  │           │
                  └─────┬─────┘
                        ▼
                  Service Scan
                     -sC -sV
                        │
                        ▼
                 Identify Services
                        │
          ┌─────────────┼─────────────┐
          ▼             ▼             ▼
        HTTP           SMB           SSH
          │             │             │
          ▼             ▼             ▼
      Web enum      SMB enum      SSH enum
          │             │             │
          └─────────────┼─────────────┘
                        ▼
                   UDP Scan
                        │
                        ▼
                  NSE / OS Detection
                        │
                        ▼
               Vulnerability Research
                        │
                        ▼
                    EXPLOIT
```

## Commands

### 1. Quick

```
nmap -T4 <TARGET>
```

### 2. Full TCP

```
sudo nmap -p- -T4 <TARGET>
```

### 3. Service Enumeration

```
nmap -sC -sV -p <PORTS> <TARGET>
```

### 4. UDP

```
sudo nmap -sU --top-ports 100 <TARGET>
```

### 5. OS

```
sudo nmap -O <TARGET>
```

### 6. Vulnerabilities

```
nmap --script vuln -p <PORTS> <TARGET>
```

---

# 14. Pentest Workflow

A professional assessment requires more controlled scanning than a typical CTF.

```
Scope
 │
 ▼
Host Discovery
 │
 ▼
Identify Live Hosts
 │
 ▼
TCP Port Scan
 │
 ▼
UDP Port Scan
 │
 ▼
Service / Version Detection
 │
 ▼
OS Fingerprinting
 │
 ▼
NSE Enumeration
 │
 ▼
Service-Specific Enumeration
 │
 ▼
Vulnerability Assessment
 │
 ▼
Manual Validation
 │
 ▼
Exploitation
```

### Recommended sequence

```
# Host discovery
sudo nmap -sn <NETWORK> -oA scans/hosts

# Full TCP
sudo nmap -sS -p- -T4 <TARGET> -oA scans/tcp

# Service enumeration
sudo nmap -sC -sV -p <PORTS> <TARGET> -oA scans/services

# UDP
sudo nmap -sU --top-ports 1000 <TARGET> -oA scans/udp

# OS
sudo nmap -O <TARGET> -oA scans/os
```

---

# 15. Troubleshooting

## "Host seems down"

```
Host seems down
      │
      ▼
Try -Pn
      │
      ▼
nmap -Pn <TARGET>
```

---

## No Ports Found

```
No open ports
      │
      ▼
Was the scan full?
      │
 ┌────┴─────┐
 │          │
 NO         YES
 │          │
 ▼          ▼
-p-       Check UDP
 │          │
 ▼          ▼
Rescan    -sU
```

Command:

```
sudo nmap -p- -T4 <TARGET>
```

---

## Only Common Ports Found

Do not assume the target only exposes those services.

```
sudo nmap -p- -T4 <TARGET>
```

---

## Service Version Unknown

```
nmap -sV --version-all -p <PORT> <TARGET>
```

Then:

```
nc <TARGET> <PORT>
```

or, for HTTP:

```
curl -i http://<TARGET>:<PORT>/
```

---

## UDP Shows `open|filtered`

```
open|filtered
      │
      ▼
Service detection
      │
      ▼
sudo nmap -sU -sV -p <PORT> <TARGET>
      │
      ▼
NSE / manual enumeration
```

---

## Too Slow

For TCP:

```
nmap -p- -T4 <TARGET>
```

For UDP:

```
sudo nmap -sU --top-ports 100 <TARGET>
```

Do not immediately increase the rate without considering packet loss and scan accuracy.

---

# 16. Scan Selection Matrix

|Situation|First Choice|Follow-up|
|---|---|---|
|New CTF target|`nmap -T4`|`-p-`|
|Need every TCP port|`-p-`|`-sC -sV`|
|Service identification|`-sV`|`--version-all`|
|Basic enumeration|`-sC`|Service-specific NSE|
|Host discovery|`-sn`|`-Pn` if blocked|
|Host seems down|`-Pn`|Full port scan|
|UDP unknown|`-sU --top-ports 100`|Targeted UDP|
|OS unknown|`-O`|`--osscan-guess`|
|Firewall suspected|`-sA`|FIN/NULL/Xmas|
|Unknown service|`-sV`|`nc` / manual probing|
|Vulnerability discovery|`--script vuln`|Manual validation|
|Need scan evidence|`-oA`|Analyse saved output|

---

# Final Enumeration Checklist

Before moving from reconnaissance to exploitation, verify:

```
[ ] Target confirmed
[ ] Host discovery completed
[ ] Full TCP scan completed
[ ] Unusual/high TCP ports checked
[ ] UDP considered
[ ] Service versions identified
[ ] Unknown services manually investigated
[ ] HTTP/HTTPS services enumerated
[ ] SMB checked where applicable
[ ] FTP checked where applicable
[ ] DNS checked where applicable
[ ] SNMP checked where applicable
[ ] NSE used where useful
[ ] OS fingerprinting performed where useful
[ ] Scan results saved
[ ] Service-specific attack surface identified
[ ] Potential vulnerabilities researched
[ ] Findings manually validated
```

---

# Mental Model

The most important thing to remember is not a specific command. It is the progression:

```
DISCOVER
   ↓
SCAN
   ↓
IDENTIFY
   ↓
ENUMERATE
   ↓
UNDERSTAND
   ↓
RESEARCH
   ↓
VALIDATE
   ↓
EXPLOIT
```

Nmap primarily operates in the first five stages. Its purpose is to transform an unknown target into a structured description of the exposed attack surface.

```
Unknown Host
     │
     ▼
10.10.10.10
     │
     ▼
22/tcp
80/tcp
445/tcp
     │
     ▼
SSH
HTTP
SMB
     │
     ▼
OpenSSH x.x
Apache x.x
Samba x.x
     │
     ▼
Service-specific enumeration
     │
     ▼
Potential attack vectors
```

> **Core rule:** Do not stop when Nmap tells you that a port is open. An open port is an entry point into the next phase of enumeration.