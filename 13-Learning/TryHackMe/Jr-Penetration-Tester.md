# Jr Penetration Tester

## Introduction

The **Jr Penetration Tester** path on TryHackMe is designed to develop the fundamental skills required to begin working in penetration testing.

Throughout the path, different areas of offensive security are covered, including networking, reconnaissance, enumeration, vulnerability assessment, exploitation, web application security, Windows and Linux systems, Active Directory, and other techniques commonly used during penetration tests.

The goal is to progressively develop a practical methodology for assessing targets, identifying attack surfaces, validating vulnerabilities, and documenting findings.

[TryHackMe — Jr Penetration Tester](https://tryhackme.com/path/outline/jrpenetrationtester)

---

# 1. Penetration Testing Foundations

This module introduces the fundamental concepts of penetration testing through guided engagements and the methodologies used to structure professional security assessments.

It covers the penetration testing process from reconnaissance through exploitation using both a web application and vulnerable infrastructure. The module also introduces the **Cyber Kill Chain** and several **penetration testing frameworks**, providing the foundation for understanding how real-world penetration tests are structured.

[TryHackMe — Penetration Testing Foundations](https://tryhackme.com/module/penetration-testing-foundations)

## Guided Pentest: Web

[TryHackMe — Guided Pentest: Web](https://tryhackme.com/room/guidedpentestweb)

### Content

This room introduces a practical web application penetration test, following the process from initial reconnaissance and enumeration through vulnerability discovery and exploitation. The engagement focuses on understanding how a penetration tester maps a web application's attack surface, identifies interesting functionality, and uses the information gathered during reconnaissance to guide further testing.

The assessment covers several common web application weaknesses, including **Insecure Direct Object References (IDOR)**, weaknesses in **password reset functionality**, and insecure **file upload mechanisms**. These vulnerabilities demonstrate the importance of testing both application functionality and access controls rather than relying only on automated scanning.

The room ultimately demonstrates how multiple vulnerabilities can be chained together to escalate the impact of an attack, progressing from information disclosure and account compromise to access to administrative functionality and **Remote Code Execution (RCE)**. This highlights the importance of understanding the complete attack chain rather than analysing vulnerabilities in isolation.

## Guided Pentest: Infrastructure

[TryHackMe — Guided Pentest: Infrastructure](https://tryhackme.com/room/guidedpentestinfrastructure)

### Content

This room introduces the methodology behind an infrastructure penetration test, covering the process from initial enumeration to exploitation, privilege escalation, and reporting. The assessment focuses on a Linux host and demonstrates how a penetration tester combines scanning, service identification, vulnerability research, exploitation, and post-exploitation enumeration to compromise a target.

The room uses **Nmap** for service enumeration and demonstrates how the results can be analysed to identify vulnerable software and potential exploits. After gaining initial access through a vulnerable service, the assessment continues from inside the target, where local Linux files and configurations are enumerated to identify a path to privilege escalation.

The final stage introduces the importance of **penetration testing reports**, showing how findings should be documented with their impact, exploitation evidence, and remediation recommendations. The overall engagement demonstrates the complete workflow of an infrastructure pentest: **enumeration → vulnerability analysis → initial access → privilege escalation → reporting**.

## Dive Into Pentesting

[TryHackMe — Dive Into Pentesting](https://tryhackme.com/room/diveintopentesting)

### Content

This room introduces the core concepts and methodology behind penetration testing, focusing on how authorised security testing differs from malicious hacking. It covers the main areas of web application and network penetration testing, as well as the relationship between **vulnerabilities, threats, and risk**, including the concept of `Vulnerability × Threat = Risk` and the risk management cycle.

The room also explores why vulnerabilities exist, including human assumptions, software bugs, system complexity, excessive customisation, and technical or design weaknesses. It emphasises the mindset required from an effective penetration tester, particularly attention to detail, contextual thinking, avoiding excessive reliance on automated tools, and maintaining good notes and evidence throughout an engagement.

Finally, the room covers the importance of **ethics, permission, and trust** during penetration tests. Professional testing requires proper authorisation, defined scope, responsible communication, effective time management, and professional conduct to ensure that security testing benefits the organisation without causing unnecessary impact.

## Cyber Kill Chain

[TryHackMe — Cyber Kill Chain](https://tryhackme.com/room/cyberkillchain)

### Content

This room introduces the **Cyber Kill Chain**, a framework developed by Lockheed Martin to describe the different stages of a cyber attack. It breaks an attack into seven stages: **Reconnaissance, Weaponization, Delivery, Exploitation, Installation, Command and Control (C2), and Actions on Objectives**. Understanding these stages helps security professionals analyse how an attack progresses from initial preparation to achieving its final objective.

The room demonstrates how the Cyber Kill Chain can be applied to real-world attacks and how defenders can use it to identify opportunities to detect, disrupt, or prevent an attack at different stages. It also highlights that attackers do not necessarily follow every stage in a strict sequence, making the framework a useful model for understanding attacker behaviour rather than a rigid procedure.

Understanding the Cyber Kill Chain is useful during penetration testing because it provides a structured way to think about an attack from both the **attacker's and defender's perspective**. It helps identify where security controls can be bypassed, where detection is possible, and how individual actions contribute to the overall attack lifecycle.

## Penetration Testing Frameworks

[TryHackMe — Penetration Testing Frameworks](https://tryhackme.com/room/penetrationtestingframeworks)

### Content

This room introduces penetration testing frameworks and explains how they provide a structured methodology for conducting security assessments, from planning and scoping through exploitation, reporting, and remediation validation. Using a recognised framework improves thoroughness, consistency, compliance, and communication during an engagement, while also helping testers avoid unstructured testing and activities outside the agreed scope.

The room introduces several major frameworks and methodologies, including **OSSTMM**, **OWASP Web Security Testing Guide (WSTG)**, **NIST SP 800-115**, **PTES**, and **ISSAF**. It also introduces **MITRE ATT&CK** as a complementary knowledge base for mapping adversary tactics and techniques, alongside other frameworks such as WASC, CSA Cloud Controls Matrix, OWASP MASTG, PCI DSS Penetration Testing Guidelines, and CBEST.

The final task provides a practical exercise where different testing scenarios are presented and the appropriate framework must be selected for each situation. This reinforces the main concept of the room: **the most suitable penetration testing framework depends on the type, scope, and objectives of the engagement**.

---
# 2. Network Reconnaissance

This module introduces the reconnaissance phase of a penetration test, focusing on how information about a target can be collected, analysed, and used to build an initial understanding of its infrastructure and attack surface.

It covers both **passive and active reconnaissance**, introducing techniques and tools such as WHOIS, DNS queries, ping, traceroute, telnet, and web browsers. The module focuses on understanding the difference between gathering information without directly interacting with a target and actively probing systems to learn more about their network and services.

The knowledge gained throughout this module provides the foundation for moving from initial information gathering into **enumeration**, helping to identify hosts, services, technologies, and other areas that may require further investigation.

[TryHackMe — Reconnaissance](https://tryhackme.com/module/network-reconnaissance)

## Passive Reconnaissance

[TryHackMe — Passive Reconnaissance](https://tryhackme.com/room/passiverecon)

### Content

This room introduces the fundamentals of passive reconnaissance and how publicly available information can be used to build an initial understanding of a target without directly interacting with its systems.

It covers essential reconnaissance tools such as **WHOIS**, **nslookup**, and **dig**, demonstrating how they can be used to gather information about domain registration, DNS records, nameservers, and other publicly exposed infrastructure.

The room highlights the importance of gathering information before moving into more active testing. By understanding the target's publicly available information first, a penetration tester can build an initial attack surface and use these findings to guide further reconnaissance and enumeration.

## Active Reconnaissance

[TryHackMe — Active Reconnaissance](https://tryhackme.com/room/activerecon)

### Content

This room introduces active reconnaissance, focusing on how penetration testers interact directly with a target to gather information about its systems, network, and services. It builds on the concepts introduced in passive reconnaissance and demonstrates how direct interaction can provide information that is not available through public sources alone.

The room covers practical tools and techniques including **ping**, **traceroute**, **telnet**, and web browsers. These tools are used to determine whether hosts are reachable, understand network paths, interact with services, and inspect how web servers and applications respond to requests.

The room demonstrates how the information collected through active reconnaissance can be interpreted and used to guide the next stages of a penetration test. It reinforces the idea that reconnaissance is not simply about collecting information, but about understanding what each observation tells us about the target and its potential attack surface.

## Protocols and Servers

[TryHackMe — Protocols and Servers](https://tryhackme.com/room/protocolsandservers)

### Content

This room introduces the fundamental network protocols and services commonly encountered during penetration testing. It focuses on understanding how different protocols operate, what services they provide, and how they can expose useful information about a target system.

It covers common protocols and services such as **FTP**, **SSH**, **SMTP**, **DNS**, **HTTP**, and **SMB**, explaining their purpose, how clients and servers communicate, and the role that ports play in identifying accessible services. Understanding these protocols is essential for interpreting the results of network reconnaissance and service enumeration.

The room also demonstrates how knowledge of protocols and server behaviour can be applied during a penetration test. By understanding what a service is designed to do and how it normally communicates, a penetration tester can identify interesting functionality, recognise potential weaknesses, and determine which services require further enumeration or testing.

## Protocols and Servers 2

[TryHackMe — Protocols and Servers 2](https://tryhackme.com/room/protocolsandservers2)

### Content

This room builds on the fundamentals of network protocols and services by focusing on how common server technologies operate and how they can be investigated during a penetration test.

It covers practical interaction with network services and introduces techniques for communicating with servers, retrieving information, and understanding how different protocols respond to client requests. The room reinforces the importance of ports, services, and protocol behaviour when performing network enumeration.

The knowledge gained throughout the room helps penetration testers move from simply identifying an open service to understanding what that service actually provides. This allows the information gathered during enumeration to be used more effectively when identifying potential attack vectors and deciding which services require further investigation.

--- 
# 3. Nmap

This module introduces **Nmap**, one of the most widely used network scanning tools in penetration testing, focusing on how it can be used to discover live hosts, identify open ports, and enumerate services across a target network.

It covers the complete Nmap scanning process, progressing from **live host discovery** and basic TCP/UDP port scans to more advanced techniques such as SYN, NULL, FIN, Xmas, and idle scans. The module also introduces techniques for controlling scan scope, timing, packet rates, and other parameters to adapt scanning behaviour to different network environments.

The module then moves beyond port discovery into **service and operating system detection**, introducing the Nmap Scripting Engine (NSE) for extending the scanner's capabilities and gathering additional information from discovered services. It also covers different methods of saving and documenting scan results for later analysis.

The knowledge gained throughout this module provides a practical foundation for **network enumeration**, allowing penetration testers to systematically map a target's exposed infrastructure and use the information gathered to identify services, technologies, and potential attack surfaces requiring further investigation.

[TryHackMe — Nmap](https://tryhackme.com/module/nmap)

# Nmap Live Host Discovery

[TryHackMe — Nmap Live Host Discovery](https://tryhackme.com/room/nmap01vu)

### Content

This room introduces **Nmap** as a network reconnaissance tool, focusing specifically on **live host discovery**. It explains how penetration testers can determine which systems are online before proceeding to more intensive port and service enumeration, avoiding unnecessary scans against inactive or unreachable hosts. The room is the first in a four-part Nmap series covering host discovery, basic port scanning, advanced scanning, and post-port-scan techniques.

It covers how Nmap can identify live systems using different network protocols and mechanisms, including **ARP, ICMP, TCP, and UDP**. The room explores how each technique works at different layers of the TCP/IP model and how the responses generated by target systems can be used to infer whether a host is reachable. It also introduces tools such as `arp-scan` and `masscan`, which overlap with some aspects of Nmap's host-discovery capabilities. 

A significant part of the room focuses on practical Nmap host-discovery techniques, including ARP scans for local networks, ICMP Echo and Timestamp requests, TCP SYN and ACK pings, and UDP-based discovery. It also introduces target specification using individual addresses, ranges, subnets, and input files, alongside options such as `-sn`, `-PE`, `-PP`, `-PM`, `-PS`, `-PA`, and `-PU`. 

The room also introduces **reverse DNS resolution** as an additional reconnaissance technique. By resolving discovered IP addresses to hostnames, a penetration tester may obtain useful contextual information about systems and their roles within a network. Overall, the room establishes the foundation for using Nmap to systematically map a target environment before progressing to **port scanning and service enumeration**, which are covered in the subsequent Nmap rooms. 

# Nmap Basic Port Scans

[TryHackMe — Nmap Basic Port Scans](https://tryhackme.com/room/nmap02)

### Content

This room builds on **Nmap Live Host Discovery** by introducing the next stage of network reconnaissance: **port scanning**. It focuses on identifying which TCP and UDP ports are accessible on a discovered host and using their states to determine whether network services are listening. The room is the second part of the four-room Nmap series, progressing from host discovery into basic and more advanced port-scanning techniques. 

It covers the fundamentals of **TCP and UDP ports**, including how services are associated with specific ports and how Nmap classifies ports into states such as **open, closed, filtered, unfiltered, open|filtered, and closed|filtered**. The room also introduces the relevant **TCP flags**, particularly SYN, ACK, and RST, which are essential for understanding how Nmap performs different types of TCP scans. 

A significant part of the room focuses on **TCP Connect Scans (`-sT`)**, **TCP SYN Scans (`-sS`)**, and **UDP Scans (`-sU`)**. It explains the differences between completing a TCP three-way handshake and using a SYN-based approach that terminates the exchange before establishing a full connection, as well as the different behaviour and limitations involved in UDP scanning. 

The room also covers **scan scope and performance tuning**, including specifying individual ports and ranges with `-p`, scanning all 65,535 ports with `-p-`, using `-F` and `--top-ports` for faster scans, controlling timing with `-T0` through `-T5`, and adjusting packet rates and probe parallelisation. These techniques allow penetration testers to balance scan speed, network load, and the level of coverage required during reconnaissance. 

# Nmap Advanced Port Scans

[TryHackMe — Nmap Advanced Port Scans](https://tryhackme.com/room/nmap03)

### Content

This room builds on the fundamentals of TCP port scanning by introducing **advanced Nmap scan types** designed to analyse target systems in situations where standard SYN or Connect scans may not be sufficient. It focuses on how different TCP flag combinations influence the responses received from a target and how those responses can be used to infer port states. 

It covers **Null, FIN, Xmas, Maimon, ACK, Window, and Custom scans**, demonstrating how each technique manipulates TCP flags for a specific reconnaissance purpose. These scans can be useful for investigating unusual network configurations, differentiating firewall behaviour, and probing systems where conventional scanning techniques may be restricted.

The room also introduces **evasion and spoofing techniques**, including source IP and MAC spoofing, decoy scans, fragmented packets, and **Idle/Zombie scans**. These techniques demonstrate how Nmap can modify the characteristics and apparent origin of scan traffic, while also illustrating how firewalls and IDS systems can affect network reconnaissance.

Finally, the room covers options for obtaining **more detailed information from Nmap**, including increased verbosity, scan reasoning, and debugging output. The knowledge gained throughout the room allows penetration testers to move beyond conventional port scans and understand how different network conditions, filtering mechanisms, and TCP behaviours can influence the results of reconnaissance. 

# Nmap Post Port Scans

[TryHackMe — Nmap Post Port Scans](https://tryhackme.com/room/nmap04)

### Content

This room focuses on the activities that follow **port scanning**, moving from identifying open ports to gathering more detailed information about the services and operating system running on the target. It is the final room in the Nmap series and introduces techniques for turning the results of previous scans into a more complete picture of the target's attack surface. 

It covers **service and version detection**, allowing Nmap to identify the software and versions associated with discovered open ports. This information is particularly useful during enumeration because knowing the exact service and version can help determine what technologies are being used and whether they may be associated with known vulnerabilities or misconfigurations. 

The room also introduces **OS detection and traceroute**, demonstrating how Nmap can fingerprint the target's operating system and map the network path between the scanning machine and the target. It then introduces the **Nmap Scripting Engine (NSE)**, which extends Nmap beyond traditional port scanning by allowing scripts to perform additional enumeration and security-related checks against discovered services.

Finally, the room covers **saving and documenting Nmap scan results** in different output formats. Properly preserving scan results is important during penetration tests because reconnaissance data may need to be analysed later, compared with subsequent scans, or incorporated into technical reports. Together, these techniques complete the Nmap workflow from host discovery and port scanning through **service enumeration, OS fingerprinting, scripting, and result documentation**. 

---
# 4. Web Application Security Fundamentals

This module introduces the **fundamentals of web application security**, focusing on how modern web applications are structured and how penetration testers approach them from an attacker's perspective.

It covers **application walkthroughs, content discovery, modern web stacks, and web server attacks**, teaching how to identify hidden pages, directories, files, technologies, and exposed functionality that can expand the application's attack surface.

The knowledge gained throughout this module provides a foundation for **web application enumeration and security testing**, preparing for more advanced topics such as authentication attacks, injection vulnerabilities, SSRF, XSS, IDOR, and other web application vulnerabilities.

[TryHackMe — Web Application Security Fundamentals](https://tryhackme.com/module/web-application-security-fundamentals)
# Walking An Application

[TryHackMe — Walking An Application](https://tryhackme.com/room/walkinganapp)

### Content

This room introduces the **manual analysis of web applications** using only the tools built into a web browser. The focus is on understanding how an application behaves and identifying useful information or potential security issues that automated tools may overlook.

It covers **page source inspection** and the use of browser **Developer Tools**, including the Inspector for analysing and modifying HTML elements, and the Debugger for examining and controlling client-side JavaScript. These techniques can reveal hidden content, application logic, comments, and other information that is not immediately visible through the rendered interface.

The room also introduces the **Network** and **Storage** developer tools. Network inspection allows the requests and responses generated by the application to be analysed, while Storage inspection provides visibility into data stored by the website on the client side. Together, these techniques establish a practical methodology for manually walking through a web application and understanding its **structure, client-side logic, communications, and locally stored data** before moving towards more active security testing.

# Content Discovery

[TryHackMe — Content Discovery](https://tryhackme.com/room/contentdiscoveryx)

### Content

This room introduces **content discovery** as a fundamental part of web application reconnaissance, focusing on identifying hidden files, directories, endpoints, and resources that are not directly exposed through the application's interface.

It covers **manual discovery, OSINT, and automated enumeration**, using techniques such as analysing `robots.txt`, `sitemap.xml`, HTTP headers, Google dorking, GitHub, the Wayback Machine, and technology-specific information to uncover additional application content.

The room also introduces **Gobuster** for automated directory and file discovery, as well as DNS subdomain and virtual host enumeration. Together, these techniques provide a practical methodology for mapping a web application's **attack surface** and identifying additional resources that may require further security testing.

