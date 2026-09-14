# Nmap Cheat Sheet — Pentest & CTF

> Practical Nmap reference focused on **penetration testing, CTFs, reconnaissance, enumeration, and service discovery**.

---

## Index

- [[#Top 10 Commands|Top 10 Commands]]
    
- [[#Target Specification|Target Specification]]
    
- [[#Host Discovery|Host Discovery]]
    
- [[#Port Scanning|Port Scanning]]
    
- [[#Service Enumeration|Service Enumeration]]
    
- [[#OS Detection|OS Detection]]
    
- [[#NSE — Nmap Scripting Engine|NSE]]
    
- [[#UDP Scanning|UDP Scanning]]
    
- [[#Advanced Scans|Advanced Scans]]
    
- [[#Timing and Performance|Timing & Performance]]
    
- [[#Firewall and IDS|Firewall / IDS]]
    
- [[#Output and Reporting|Output]]
    
- [[#Port States|Port States]]
    
- [[#CTF Workflows|CTF Workflows]]
    
- [[#Pentest Workflows|Pentest Workflows]]
    
- [[#Troubleshooting|Troubleshooting]]
    
- [[#Quick Reference Tables|Quick Reference]]

---

# Top 10 Commands

These are the commands worth knowing first. In a typical CTF or penetration test, most Nmap work can be built from these.

|#|Command|Purpose|
|---|---|---|
|1|`nmap <TARGET>`|Basic TCP scan of common ports|
|2|`nmap -p- <TARGET>`|Scan all 65,535 TCP ports|
|3|`nmap -sV -p <PORTS> <TARGET>`|Detect service versions|
|4|`nmap -sC -sV -p <PORTS> <TARGET>`|Default NSE scripts + version detection|
|5|`nmap -Pn <TARGET>`|Scan without host discovery|
|6|`nmap -sS -p- -T4 <TARGET>`|Fast full TCP SYN scan|
|7|`nmap -sU --top-ports 100 <TARGET>`|Scan common UDP ports|
|8|`nmap -A <TARGET>`|Aggressive detection|
|9|`nmap --script vuln <TARGET>`|Run vulnerability NSE scripts|
|10|`nmap -oA <NAME> <TARGET>`|Save results in normal, XML and grepable formats|

### The practical CTF sequence

```
# 1. Quick scan
nmap -T4 <TARGET>

# 2. Full TCP scan
nmap -p- -T4 <TARGET>

# 3. Enumerate discovered services
nmap -sC -sV -p <PORTS> <TARGET>

# 4. Check UDP
nmap -sU --top-ports 100 <TARGET>

# 5. Save everything
nmap -sC -sV -p <PORTS> -oA nmap/<TARGET> <TARGET>
```

> Replace `<TARGET>` with the target IP, hostname, or CIDR range.

---

# Target Specification

Nmap can scan individual hosts, ranges, subnets, hostnames, and target lists.

|Syntax|Example|Description|
|---|---|---|
|Single IP|`nmap 10.10.10.10`|Scan one host|
|Hostname|`nmap target.local`|Scan hostname|
|Multiple IPs|`nmap 10.10.10.10 10.10.10.20`|Scan multiple hosts|
|Range|`nmap 10.10.10.1-50`|Scan IP range|
|CIDR|`nmap 10.10.10.0/24`|Scan subnet|
|File|`nmap -iL targets.txt`|Read targets from file|
|Exclude|`nmap 10.10.10.0/24 --exclude 10.10.10.5`|Exclude target|
|IPv6|`nmap -6 <TARGET>`|Scan IPv6 target|

### Useful target options

```
nmap <TARGET>
nmap 10.10.10.10
nmap 10.10.10.10-20
nmap 10.10.10.0/24
nmap -iL targets.txt
nmap --exclude 10.10.10.5 10.10.10.0/24
```

---

# Host Discovery

Before scanning ports, Nmap can determine which hosts are alive.

## Basic Host Discovery

```
nmap -sn 10.10.10.0/24
```

`-sn` performs host discovery without performing a port scan.

This is useful during internal network reconnaissance when the first objective is identifying live systems.

|Option|Purpose|
|---|---|
|`-sn`|Host discovery only|
|`-Pn`|Skip host discovery and assume host is up|
|`-PE`|ICMP Echo discovery|
|`-PP`|ICMP Timestamp discovery|
|`-PM`|ICMP Address Mask discovery|
|`-PS`|TCP SYN ping|
|`-PA`|TCP ACK ping|
|`-PU`|UDP ping|

### When host discovery fails

A target may be alive but ignore ICMP or other discovery probes.

Use:

```
nmap -Pn <TARGET>
```

### Decision table

|Situation|Use|
|---|---|
|Normal network|`nmap -sn <TARGET>`|
|ICMP blocked|`nmap -PS80,443 <TARGET>`|
|Firewall blocks discovery|`nmap -Pn <TARGET>`|
|Internal LAN|ARP discovery is generally effective|
|CTF target|Usually start with normal scan; use `-Pn` if necessary|

---

# Port Scanning

Port scanning is the core Nmap functionality used to identify exposed services.

## Default Scan

```
nmap <TARGET>
```

Scans Nmap's default set of commonly used TCP ports.

Useful for a quick initial overview, but **not sufficient for CTF enumeration** when hidden services may exist on high ports.

---

## Scan Specific Ports

```
nmap -p 22 <TARGET>

nmap -p 22,80,443 <TARGET>

nmap -p 80-443 <TARGET>
```

### Port specification

|Syntax|Meaning|
|---|---|
|`-p 22`|Single port|
|`-p 22,80,443`|Specific ports|
|`-p 1-1000`|Port range|
|`-p-`|All TCP ports|
|`-p 80,443,8000-9000`|Mixed selection|
|`-p U:53,T:80`|UDP 53 + TCP 80|

---

# Full TCP Port Scan

One of the most important commands in a CTF:

```
nmap -p- -T4 <TARGET>
```

For a more aggressive but still common CTF scan:

```
nmap -p- -T4 --min-rate 1000 <TARGET>
```

### Why use `-p-`?

The default Nmap scan does not check every TCP port. CTF machines frequently expose services on unusual ports.

Example:

```
22/tcp
80/tcp
31337/tcp
49152/tcp
```

A default scan may not provide the complete attack surface.

---

# TCP SYN Scan

```
nmap -sS <TARGET>
```

`-sS` performs a TCP SYN scan.

It sends a SYN and analyses the response without completing a normal TCP connection. A SYN/ACK generally indicates an open port, while RST generally indicates a closed port. Lack of response or certain ICMP errors can result in a filtered classification. citeturn0search4

### Recommended CTF usage

```
sudo nmap -sS -p- -T4 <TARGET>
```

When raw packet privileges are unavailable, Nmap may fall back to a TCP Connect scan. citeturn0search4

---

# TCP Connect Scan

```
nmap -sT <TARGET>
```

`-sT` uses the operating system's normal TCP connection mechanism.

It is useful when SYN scanning is unavailable, but `-sS` is generally preferable when privileges permit raw packet scanning. citeturn0search4

---

# Service Enumeration

Finding an open port is only the beginning.

Example:

```
80/tcp open http
```

The next question is:

> What software and version is actually running?

Use:

```
nmap -sV -p 80 <TARGET>
```

For multiple services:

```
nmap -sV -p 22,80,443,8080 <TARGET>
```

### Version detection

|Command|Use|
|---|---|
|`-sV`|Service/version detection|
|`--version-light`|Faster, less comprehensive detection|
|`--version-all`|Try every available probe|
|`-sC -sV`|Default scripts + version detection|

### Recommended

```
nmap -sC -sV -p <PORTS> <TARGET>
```

This is one of the most useful enumeration commands after discovering the ports.

---

# Aggressive Scan

```
nmap -A <TARGET>
```

`-A` enables several advanced detection features, including:

- OS detection
    
- Version detection
    
- Default NSE scripts
    
- Traceroute
    

For CTFs:

```
nmap -A -T4 <TARGET>
```

For more controlled enumeration, it is often preferable to specify the individual components:

```
nmap -sC -sV -O -p <PORTS> <TARGET>
```

This gives you more control over what is being performed.

---

# OS Detection

```
sudo nmap -O <TARGET>
```

Nmap attempts to fingerprint the target operating system.

More detailed:

```
sudo nmap -O -v <TARGET>
```

Combined with service detection:

```
sudo nmap -O -sV <TARGET>
```

### OS detection requirements

OS detection works best when Nmap has enough open and closed ports to analyse the target's TCP/IP behaviour.

If results are uncertain:

```
sudo nmap -O --osscan-guess <TARGET>
```

---

# NSE — Nmap Scripting Engine

The **Nmap Scripting Engine (NSE)** extends Nmap with scripts for discovery, enumeration, version detection, vulnerability detection, authentication checks, brute-force tasks, and other security-related operations. Nmap officially documents categories including `auth`, `broadcast`, `brute`, `default`, `discovery`, `exploit`, `safe`, `version`, `vuln`, and others. citeturn0search0turn0search3

## Default Scripts

```
nmap -sC <TARGET>
```

Equivalent to:

```
nmap --script=default <TARGET>
```

Common CTF combination:

```
nmap -sC -sV -p <PORTS> <TARGET>
```

---

## Specific Script

```
nmap --script <SCRIPT> -p <PORT> <TARGET>
```

Example:

```
nmap --script http-title -p 80 <TARGET>
```

---

## Script Categories

|Category|Purpose|
|---|---|
|`auth`|Authentication-related checks|
|`broadcast`|Broadcast-based discovery|
|`brute`|Brute-force scripts|
|`default`|Default NSE scripts|
|`discovery`|Information discovery|
|`dos`|Denial-of-service testing|
|`exploit`|Exploitation|
|`external`|External third-party services|
|`fuzzer`|Fuzzing|
|`info`|General information|
|`intrusive`|Potentially intrusive scripts|
|`malware`|Malware detection|
|`safe`|Designed to be safer|
|`version`|Version detection|
|`vuln`|Vulnerability detection|

NSE scripts are not sandboxed, and some categories can be intrusive or potentially dangerous. Only use them against systems you are authorized to test. citeturn0search0turn0search3

---

# Useful NSE Commands

## Vulnerability Scan

```
nmap --script vuln <TARGET>
```

Against specific ports:

```
nmap --script vuln -p 80,443 <TARGET>
```

---

## HTTP Enumeration

```
nmap --script http-* -p 80,443 <TARGET>
```

Useful individual scripts:

```
nmap --script http-title -p 80 <TARGET>

nmap --script http-headers -p 80 <TARGET>

nmap --script http-methods -p 80 <TARGET>

nmap --script http-enum -p 80 <TARGET>
```

---

## SMB Enumeration

```
nmap --script smb-enum-* -p 445 <TARGET>
```

Useful:

```
nmap --script smb-os-discovery -p 445 <TARGET>

nmap --script smb-enum-shares -p 445 <TARGET>
```

---

## FTP Enumeration

```
nmap --script ftp-anon -p 21 <TARGET>
```

Useful for checking anonymous FTP access.

---

## DNS Enumeration

```
nmap --script dns-* -p 53 <TARGET>
```

---

## SSH Enumeration

```
nmap --script ssh-* -p 22 <TARGET>
```

---

## Script Search

List available scripts:

```
ls /usr/share/nmap/scripts/
```

Search for a specific protocol:

```
ls /usr/share/nmap/scripts/ | grep smb
```

Search for HTTP scripts:

```
ls /usr/share/nmap/scripts/ | grep http
```

Check script documentation:

```
nmap --script-help <SCRIPT>
```

The official NSE documentation provides the current script database and individual script descriptions. citeturn0search7turn0search11

---

# UDP Scanning

UDP is frequently forgotten during enumeration.

```
sudo nmap -sU <TARGET>
```

Full UDP:

```
sudo nmap -sU -p- <TARGET>
```

This can be slow.

A practical CTF approach:

```
sudo nmap -sU --top-ports 100 <TARGET>
```

More coverage:

```
sudo nmap -sU --top-ports 1000 <TARGET>
```

Combine TCP and UDP:

```
sudo nmap -sS -sU -p T:22,80,443,U:53,161 <TARGET>
```

UDP scanning is particularly relevant for services such as DNS, SNMP and DHCP. Nmap's UDP scan marks a port `closed` when it receives an appropriate ICMP port-unreachable response; otherwise the result may remain `open|filtered` without a useful response. citeturn0search4

---

# UDP Port States

|State|Meaning|
|---|---|
|`open`|UDP service responded|
|`closed`|ICMP port unreachable received|
|`open\|filtered`|No response or filtering prevents determination|
|`filtered`|Filtering prevents determining state|

When you see:

```
161/udp open|filtered snmp
```

do not assume the service is definitely running.

Perform additional enumeration:

```
sudo nmap -sU -sV -p 161 <TARGET>
```

---

# Advanced Scans

Nmap supports several TCP scans based on different flag combinations. These are useful for analysing filtering behaviour and specialised reconnaissance scenarios. citeturn0search9

## FIN Scan

```
sudo nmap -sF <TARGET>
```

---

## NULL Scan

```
sudo nmap -sN <TARGET>
```

---

## Xmas Scan

```
sudo nmap -sX <TARGET>
```

---

## ACK Scan

```
sudo nmap -sA <TARGET>
```

Primarily useful for determining whether ports are filtered rather than determining whether they are open.

---

## Window Scan

```
sudo nmap -sW <TARGET>
```

---

## Maimon Scan

```
sudo nmap -sM <TARGET>
```

---

## Custom TCP Flags

```
sudo nmap --scanflags <FLAGS> <TARGET>
```

Example:

```
sudo nmap --scanflags SYNFIN <TARGET>
```

---

# Advanced Scan Decision Table

|Situation|Technique|
|---|---|
|Normal TCP enumeration|`-sS`|
|No raw packet privileges|`-sT`|
|UDP services|`-sU`|
|Determine filtering|`-sA`|
|Firewall behaviour investigation|`-sA`, `-sF`, `-sN`, `-sX`|
|Special TCP behaviour|`--scanflags`|
|Idle/zombie technique|`-sI`|

FIN, NULL and Xmas scans have platform-dependent behaviour and can produce ambiguous results against non-RFC-compliant systems. They should therefore be treated as specialised techniques rather than replacements for normal SYN scanning. citeturn0search4

---

# Idle / Zombie Scan

```
sudo nmap -sI <ZOMBIE> <TARGET>
```

Idle scanning attempts to use another host as a "zombie" to make the scan appear to originate indirectly.

Basic structure:

```
sudo nmap -sI <ZOMBIE>:<PORT> <TARGET>
```

This is an advanced technique and is generally not required for normal CTF enumeration.

---

# Timing and Performance

Nmap provides timing templates from `T0` through `T5`.

|Template|Name|Use|
|---|---|---|
|`-T0`|Paranoid|Extremely slow|
|`-T1`|Sneaky|Very slow|
|`-T2`|Polite|Reduced network load|
|`-T3`|Normal|Default|
|`-T4`|Aggressive|Fast|
|`-T5`|Insane|Very aggressive|

### CTF default

```
nmap -T4 <TARGET>
```

### Full CTF scan

```
nmap -p- -T4 <TARGET>
```

### Faster scanning

```
nmap -p- -T4 --min-rate 1000 <TARGET>
```

Use aggressive rate settings carefully. Network conditions, packet loss, IDS/IPS behaviour and target stability can affect accuracy.

---

# Rate Control

```
--min-rate <NUMBER>
--max-rate <NUMBER>
```

Example:

```
nmap -p- -T4 --min-rate 1000 <TARGET>
```

Maximum rate:

```
nmap -p- --max-rate 500 <TARGET>
```

---

# Firewall and IDS

Some environments filter or alter scan traffic.

## Skip Host Discovery

```
nmap -Pn <TARGET>
```

## Fragment Packets

```
sudo nmap -f <TARGET>
```

Larger fragmentation:

```
sudo nmap --mtu 32 <TARGET>
```

## Decoy Scan

```
sudo nmap -D RND:5 <TARGET>
```

The scan is mixed with randomly generated decoy sources.

## Spoof MAC Address

```
sudo nmap --spoof-mac 0 <TARGET>
```

Or specify a vendor prefix:

```
sudo nmap --spoof-mac <PREFIX> <TARGET>
```

> These techniques are primarily relevant when analysing filtering or IDS behaviour in an authorized assessment. They should not be treated as a way to make a scan "invisible".

---

# Port States

Nmap commonly reports six port states.

|State|Meaning|
|---|---|
|`open`|Application is actively accepting connections|
|`closed`|Host is reachable but no application is listening|
|`filtered`|Firewall/filter prevents Nmap from determining the state|
|`unfiltered`|Port is reachable but Nmap cannot determine whether it is open|
|`open\|filtered`|Nmap cannot distinguish between open and filtered|
|`closed\|filtered`|Nmap cannot distinguish between closed and filtered|

### Practical interpretation

```
22/tcp open ssh
```

→ SSH service is accessible.

```
22/tcp closed ssh
```

→ Host is reachable, but nothing is listening on TCP/22.

```
22/tcp filtered ssh
```

→ A filtering mechanism prevents Nmap from determining the state.

```
53/udp open|filtered domain
```

→ Nmap cannot determine whether the port is open or being filtered.

---

# Port-State Decision Table

|Result|Next Step|
|---|---|
|`open`|Enumerate service|
|`closed`|Usually deprioritise|
|`filtered`|Investigate filtering|
|`open\|filtered`|Perform service/version detection|
|`unfiltered`|Use additional scan types|
|Unknown service|Banner grabbing + `-sV` + NSE|

---

# Service Enumeration Workflow

Suppose Nmap returns:

```
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
445/tcp  open  microsoft-ds
```

Do not immediately attack the target.

Enumerate:

```
nmap -sC -sV -p 22,80,445 <TARGET>
```

Then perform service-specific enumeration.

### Example

```
22/tcp → SSH
80/tcp → HTTP
445/tcp → SMB
```

The Nmap phase should produce a service inventory that can then guide manual enumeration.

---

# Output and Reporting

## Normal Output

```
nmap -oN scan.txt <TARGET>
```

## XML

```
nmap -oX scan.xml <TARGET>
```

## Grepable

```
nmap -oG scan.gnmap <TARGET>
```

## All Major Formats

```
nmap -oA scan <TARGET>
```

`-oA` creates the main output formats using the specified base filename.

Example:

```
nmap -sC -sV -p- -oA scans/initial <TARGET>
```

This is strongly recommended for structured pentest work because the results can be reused later rather than relying only on terminal output.

---

# Verbosity and Debugging

## Verbose

```
nmap -v <TARGET>
```

More verbose:

```
nmap -vv <TARGET>
```

## Reason

```
nmap --reason <TARGET>
```

Useful for understanding why Nmap classified a port or host in a particular way.

## Debugging

```
nmap -d <TARGET>
```

More debugging:

```
nmap -dd <TARGET>
```

### Useful combination

```
nmap -sS -sV --reason -vv <TARGET>
```

---

# CTF Workflows

## Workflow 1 — Initial Scan

Start simple:

```
nmap -T4 <TARGET>
```

If nothing useful appears:

```
nmap -Pn -T4 <TARGET>
```

---

## Workflow 2 — Full TCP Enumeration

```
sudo nmap -p- -T4 <TARGET>
```

Record the discovered ports:

```
22,80,139,445,8080
```

Then:

```
nmap -sC -sV -p 22,80,139,445,8080 <TARGET>
```

---

## Workflow 3 — Fast CTF Enumeration

```
sudo nmap -p- -T4 --min-rate 1000 <TARGET>
```

Then:

```
nmap -sC -sV -p <PORTS> <TARGET>
```

Then investigate each exposed service individually.

---

## Workflow 4 — Complete TCP + UDP

```
sudo nmap -sS -p- -T4 <TARGET>
```

Then:

```
sudo nmap -sU --top-ports 100 <TARGET>
```

For important UDP findings:

```
sudo nmap -sU -sV -p <UDP_PORTS> <TARGET>
```

---

## Workflow 5 — Aggressive Enumeration

```
sudo nmap -A -T4 -p <PORTS> <TARGET>
```

Equivalent conceptual components:

```
sudo nmap -sC -sV -O --traceroute -p <PORTS> <TARGET>
```

The second form gives more explicit control over the individual features.

---

## Workflow 6 — Save Everything

```
mkdir -p scans

sudo nmap -p- -T4 -oA scans/full-tcp <TARGET>

nmap -sC -sV -p <PORTS> -oA scans/services <TARGET>

sudo nmap -sU --top-ports 100 -oA scans/udp <TARGET>
```

---

# Pentest Workflows

## External Host

```
nmap -Pn -T4 <TARGET>
```

Then:

```
sudo nmap -Pn -p- -T4 <TARGET>
```

Then:

```
nmap -Pn -sC -sV -p <PORTS> <TARGET>
```

---

## Internal Network

First discover hosts:

```
sudo nmap -sn 10.10.10.0/24
```

Then scan discovered systems:

```
sudo nmap -sS -p- -T4 <TARGET>
```

Then enumerate services:

```
nmap -sC -sV -p <PORTS> <TARGET>
```

---

## Web Server

```
nmap -p 80,443,8080,8000,8443 -sC -sV <TARGET>
```

Useful NSE:

```
nmap --script http-title,http-headers,http-methods -p 80,443 <TARGET>
```

---

## SMB

```
nmap -p 139,445 -sC -sV <TARGET>
```

Then:

```
nmap --script smb-os-discovery,smb-enum-shares -p 445 <TARGET>
```

---

## FTP

```
nmap -p 21 -sC -sV <TARGET>
```

Anonymous access:

```
nmap --script ftp-anon -p 21 <TARGET>
```

---

## SSH

```
nmap -p 22 -sC -sV <TARGET>
```

Additional enumeration:

```
nmap --script ssh-* -p 22 <TARGET>
```

---

# Common Service → Enumeration Map

|Service|Port(s)|First Nmap Command|
|---|---|---|
|FTP|21|`nmap -sC -sV -p 21`|
|SSH|22|`nmap -sC -sV -p 22`|
|Telnet|23|`nmap -sC -sV -p 23`|
|SMTP|25|`nmap -sC -sV -p 25`|
|DNS|53|`nmap -sC -sV -p 53`|
|HTTP|80|`nmap -sC -sV -p 80`|
|POP3|110|`nmap -sC -sV -p 110`|
|RPC|111|`nmap -sC -sV -p 111`|
|IMAP|143|`nmap -sC -sV -p 143`|
|SNMP|161/UDP|`nmap -sU -sV -p 161`|
|LDAP|389|`nmap -sC -sV -p 389`|
|HTTPS|443|`nmap -sC -sV -p 443`|
|SMB|139/445|`nmap -sC -sV -p 139,445`|
|RDP|3389|`nmap -sC -sV -p 3389`|
|MySQL|3306|`nmap -sC -sV -p 3306`|
|PostgreSQL|5432|`nmap -sC -sV -p 5432`|
|Redis|6379|`nmap -sC -sV -p 6379`|
|WinRM|5985/5986|`nmap -sC -sV -p 5985,5986`|

> Ports are defaults, not guarantees. Always trust the scan result rather than assuming a service must be running on its standard port.

---

# Troubleshooting

## Nmap Says Host Seems Down

Try:

```
nmap -Pn <TARGET>
```

---

## No Ports Found

Run:

```
sudo nmap -p- -T4 <TARGET>
```

If still nothing:

```
sudo nmap -Pn -p- -T4 <TARGET>
```

---

## Only Some Ports Appear

The default scan does not cover every TCP port.

Use:

```
nmap -p- <TARGET>
```

---

## Service Shows as Unknown

Try:

```
nmap -sV --version-all -p <PORT> <TARGET>
```

Then:

```
nmap -sC -sV -p <PORT> <TARGET>
```

Manual banner grabbing can then be performed with tools such as `nc`.

---

## Everything Is Filtered

Check with:

```
sudo nmap -sA -p- <TARGET>
```

Try:

```
sudo nmap -Pn -p- <TARGET>
```

Increase verbosity:

```
sudo nmap -Pn -p- -vv --reason <TARGET>
```

---

## UDP Is Too Slow

Start with:

```
sudo nmap -sU --top-ports 100 <TARGET>
```

Then increase coverage:

```
sudo nmap -sU --top-ports 1000 <TARGET>
```

Only perform a full UDP scan when necessary:

```
sudo nmap -sU -p- <TARGET>
```

---

# Quick Reference Tables

## Scan Type

|Goal|Command|
|---|---|
|Basic scan|`nmap <TARGET>`|
|Host discovery|`nmap -sn <TARGET>`|
|Skip discovery|`nmap -Pn <TARGET>`|
|SYN scan|`sudo nmap -sS <TARGET>`|
|Connect scan|`nmap -sT <TARGET>`|
|UDP scan|`sudo nmap -sU <TARGET>`|
|Full TCP|`sudo nmap -p- <TARGET>`|
|Service detection|`nmap -sV <TARGET>`|
|Default NSE|`nmap -sC <TARGET>`|
|OS detection|`sudo nmap -O <TARGET>`|
|Aggressive|`sudo nmap -A <TARGET>`|
|Vulnerability scripts|`nmap --script vuln <TARGET>`|

---

## Enumeration Level

|Level|Command|
|---|---|
|Quick|`nmap <TARGET>`|
|Full TCP|`nmap -p- <TARGET>`|
|Service|`nmap -sV -p <PORTS> <TARGET>`|
|Default NSE|`nmap -sC -sV -p <PORTS> <TARGET>`|
|OS + Services|`sudo nmap -O -sV -p <PORTS> <TARGET>`|
|Aggressive|`sudo nmap -A -p <PORTS> <TARGET>`|
|Vulnerability|`nmap --script vuln -p <PORTS> <TARGET>`|

---

## Speed

|Requirement|Command|
|---|---|
|Slow / cautious|`-T1` / `-T2`|
|Normal|`-T3`|
|Fast|`-T4`|
|Very aggressive|`-T5`|
|Control minimum rate|`--min-rate <N>`|
|Control maximum rate|`--max-rate <N>`|

---

## Output

|Requirement|Command|
|---|---|
|Normal|`-oN file.txt`|
|XML|`-oX file.xml`|
|Grepable|`-oG file.gnmap`|
|All formats|`-oA basename`|
|Verbose|`-v`|
|Very verbose|`-vv`|
|Show reasoning|`--reason`|
|Debug|`-d`|

---

# Practical Decision Tree

```
START
  │
  ▼
Is the target reachable?
  │
  ├── YES ──► Scan ports
  │
  └── NO / UNKNOWN
           │
           ▼
        Use -Pn
           │
           ▼
     Full TCP scan
           │
           ▼
      Open ports?
           │
           ├── NO ──► Check UDP / filtering
           │
           └── YES
                │
                ▼
          Service detection
                │
                ▼
             -sV
                │
                ▼
          Default NSE
                │
                ▼
             -sC
                │
                ▼
       Identify services
                │
                ▼
    Service-specific enumeration
                │
                ▼
       Vulnerability research
```

---

# Recommended CTF Template

For a fresh machine:

```
# Initial scan
nmap -T4 <TARGET>

# Full TCP
sudo nmap -p- -T4 -oA scans/tcp-full <TARGET>

# Service enumeration
nmap -sC -sV -p <PORTS> -oA scans/services <TARGET>

# UDP
sudo nmap -sU --top-ports 100 -oA scans/udp-top100 <TARGET>

# OS detection
sudo nmap -O -p <PORTS> <TARGET>

# Vulnerability enumeration
nmap --script vuln -p <PORTS> <TARGET>
```

---

# Recommended Pentest Template

```
# Host discovery
sudo nmap -sn <NETWORK> -oA scans/host-discovery

# TCP discovery
sudo nmap -sS -p- -T4 <TARGET> -oA scans/tcp-full

# Service enumeration
sudo nmap -sC -sV -p <PORTS> <TARGET> -oA scans/services

# OS detection
sudo nmap -O <TARGET> -oA scans/os

# UDP discovery
sudo nmap -sU --top-ports 1000 <TARGET> -oA scans/udp

# NSE vulnerability checks
sudo nmap --script vuln -p <PORTS> <TARGET> -oA scans/vuln
```

---

# Rules of Thumb

|Rule|Reason|
|---|---|
|Never rely only on the default scan|Hidden services may use uncommon ports|
|Always consider `-p-` in CTFs|Services are often placed on high ports|
|Enumerate versions after finding ports|The service/version determines the attack surface|
|Do not forget UDP|Important services can run over UDP|
|Treat `open\|filtered` carefully|It does not prove that a service is open|
|Save your results|Enumeration data is useful throughout the engagement|
|Use NSE selectively|Some scripts can be intrusive|
|Verify Nmap results manually|Fingerprinting is probabilistic|
|Do not assume a port equals a service|Services can run on arbitrary ports|
|Enumerate before exploiting|Good enumeration reduces blind trial-and-error|

---

# Minimal CTF Memory Card

If you only remember a few commands:

```
# Quick
nmap <TARGET>

# All TCP ports
sudo nmap -p- -T4 <TARGET>

# Services + versions + default scripts
nmap -sC -sV -p <PORTS> <TARGET>

# UDP
sudo nmap -sU --top-ports 100 <TARGET>

# OS
sudo nmap -O <TARGET>

# Aggressive
sudo nmap -A <TARGET>

# Vulnerability scripts
nmap --script vuln <TARGET>

# Skip ping
nmap -Pn <TARGET>

# Save everything
nmap -oA scan <TARGET>
```

---

## Official References

- [Nmap — Official Website](https://nmap.org/)
    
- [Nmap Network Scanning — Official Guide](https://nmap.org/book/)
    
- [Nmap Reference Guide](https://nmap.org/book/man.html)
    
- [NSE Documentation](https://nmap.org/nsedoc/)
    
- [NSE Script Reference](https://nmap.org/nsedoc/scripts/)