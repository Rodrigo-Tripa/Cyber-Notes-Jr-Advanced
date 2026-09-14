# Nmap

**Nmap (Network Mapper)** is a network discovery and security auditing tool. It is mainly used to discover hosts, scan ports, identify services and versions, fingerprint operating systems, and perform additional enumeration through the **Nmap Scripting Engine (NSE)**.

In penetration testing, Nmap is normally one of the first tools used against a target because it helps turn an unknown system into a map of its exposed attack surface.

## How Nmap Works

At a basic level, Nmap sends network packets to a target and analyses the responses.

```
Nmap
  │
  ├── Sends probes
  │
  ▼
Target
  │
  ├── Responds
  ├── Rejects
  └── Drops packets
  │
  ▼
Nmap analyses response
  │
  ▼
Port / Service / OS information
```

The important part is that Nmap is not simply "checking a port".

It is making observations based on how the target responds to carefully constructed packets.

This is why understanding [[03-Network-Pentesting/TCP|TCP]] and [[03-Network-Pentesting/UDP|UDP]] is important when learning Nmap.

## Port Scanning

The most common scan for TCP ports is the SYN scan:

```
sudo nmap -sS <TARGET>
```

Nmap sends a TCP `SYN` packet.

If the target responds with:

```
SYN/ACK
```

Nmap normally considers the port **open**.

If the target responds with:

```
RST
```

the port is normally considered **closed**.

If a firewall silently drops the probe, Nmap may classify the port as **filtered**.

```
SYN
 │
 ├──────────────> OPEN PORT
 │                 │
 │              SYN/ACK
 │                 │
 │<────────────────┘
 │
 ▼
Open
```

This is the basic idea behind SYN scanning.

## SYN Scan vs Connect Scan

Nmap has several ways of scanning TCP ports.

### SYN Scan

```
sudo nmap -sS <TARGET>
```

Nmap sends a SYN but normally does not complete the connection.

This is often preferred because it is fast and gives Nmap direct control over the packets.

### TCP Connect Scan

```
nmap -sT <TARGET>
```

Nmap uses the operating system's normal TCP connection mechanism and completes the three-way handshake.

The difference is roughly:

```
-sS

SYN → SYN/ACK → stop/reset


-sT

SYN → SYN/ACK → ACK → connection
```

Understanding this difference makes commands such as `-sS` much easier to remember.

## Host Discovery

Nmap can also determine which hosts appear to be online.

```
nmap -sn <TARGET>
```

This performs host discovery without a normal port scan.

For example, scanning a subnet:

```
nmap -sn 192.168.1.0/24
```

can identify which systems respond.

If discovery probes are blocked, a host can appear offline even when it is actually running.

In this situation:

```
nmap -Pn <TARGET>
```

tells Nmap to skip host discovery and scan the target anyway.

## Port Selection

Nmap does not always scan every port.

A normal scan may focus on commonly used ports. To scan all TCP ports:

```
sudo nmap -p- <TARGET>
```

The `-p-` option covers ports `1–65535`.

This is particularly useful in CTFs, where services are often placed on unusual ports.

You can also specify individual ports:

```
nmap -p 22,80,443 <TARGET>
```

or a range:

```
nmap -p 1-1000 <TARGET>
```

## Service and Version Detection

Finding an open port is only the beginning.

The `-sV` option attempts to identify the service and its version:

```
nmap -sV -p 22,80 <TARGET>
```

For example:

```
22/tcp   open   ssh    OpenSSH 9.x
80/tcp   open   http   Apache httpd 2.4.x
```

Nmap performs this by sending probes and comparing the responses against its service fingerprints.

This is more reliable than assuming:

```
80 = HTTP
22 = SSH
```

because services can run on non-standard ports.

## Default NSE Scripts

The `-sC` option runs Nmap's default NSE scripts:

```
nmap -sC <TARGET>
```

It is commonly combined with version detection:

```
nmap -sC -sV <TARGET>
```

NSE allows Nmap to perform tasks beyond basic port scanning, such as retrieving HTTP information, checking SMB configuration, discovering DNS information, and performing other service-specific enumeration.

Scripts can also be selected individually:

```
nmap --script http-title -p 80 <TARGET>
```

## OS Detection

Nmap can attempt to identify the target's operating system:

```
sudo nmap -O <TARGET>
```

This is called **OS fingerprinting**.

Nmap analyses characteristics of network responses and compares them with known fingerprints.

It may produce something such as:

```
OS details:
Linux 5.x
```

OS detection is an estimate. Network devices, firewalls, virtualisation and custom configurations can make fingerprinting less accurate.

## UDP Scanning

Nmap can scan UDP ports with:

```
sudo nmap -sU <TARGET>
```

UDP is different from TCP because there is no connection handshake.

This makes UDP scanning slower and often less certain.

A useful starting point is:

```
sudo nmap -sU --top-ports 100 <TARGET>
```

For more information, see [[03-Network-Pentesting/UDP|UDP]].

## Advanced Scans

Nmap also supports scans based on different TCP flag combinations:

```
nmap -sN <TARGET>   # NULL
nmap -sF <TARGET>   # FIN
nmap -sX <TARGET>   # Xmas
nmap -sA <TARGET>   # ACK
```

These scans can be useful for investigating filtering behaviour and understanding how firewalls respond to different packets.

They are specialised techniques and should not replace a normal TCP scan.

## Timing

Nmap provides timing templates from `-T0` to `-T5`.

```
T0  Very slow
T1  Slow
T2  Reduced speed
T3  Normal
T4  Aggressive
T5  Very aggressive
```

For CTFs and controlled labs, `-T4` is commonly useful:

```
nmap -T4 <TARGET>
```

Faster is not always better. Increasing scan speed can increase packet loss and produce less reliable results.

## Saving Results

Nmap can save its output for later analysis.

A useful option is:

```
nmap -sC -sV <TARGET> -oA scan
```

This creates several output formats using the same base name.

Keeping scan results is useful during longer assessments because enumeration is iterative and information discovered later may make an earlier result important again.

## Typical Workflow

A simple CTF workflow could look like:

```
Host Discovery
      ↓
Full TCP Scan
      ↓
Service / Version Detection
      ↓
UDP Scan
      ↓
NSE / OS Detection
      ↓
Service-Specific Enumeration
      ↓
Vulnerability Research
```

Typical commands:

```
nmap -sn <TARGET>
sudo nmap -p- -T4 <TARGET>
nmap -sC -sV -p <PORTS> <TARGET>
sudo nmap -sU --top-ports 100 <TARGET>
```

The exact sequence depends on the target and the information already available.

## Common Mistake

One of the biggest beginner mistakes is treating Nmap output as the final answer.

For example:

```
80/tcp open http
```

does not mean:

> "There is a website. Done."

It means:

> "Something appears to be providing HTTP on this port. Now investigate it."

That might lead to web enumeration, technology fingerprinting, directory discovery, virtual-host enumeration, or vulnerability research.

Nmap is therefore best understood as an **enumeration and reconnaissance tool**, not simply a port scanner.

See also [[03-Network-Pentesting/Port-Scanning|Port Scanning]], [[03-Network-Pentesting/Network-Enumeration|Network Enumeration]], and [[03-Network-Pentesting/Service-Enumeration|Service Enumeration]].