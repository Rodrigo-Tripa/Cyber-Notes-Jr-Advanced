# Passive Reconnaissance

Passive reconnaissance is the process of gathering information about a target without directly interacting with its systems.

The goal is to learn as much as possible about a target while leaving little to no trace of our activity. Instead of scanning a server or connecting directly to its services, we use publicly available information and third-party sources.

Passive reconnaissance is usually one of the first steps in a penetration test because it helps us understand the target before we start interacting with it.

---

## Passive vs Active Reconnaissance

The main difference between passive and active reconnaissance is how the information is collected.

| Type | Description |
|---|---|
| Passive Reconnaissance | Gathering information without directly interacting with the target |
| Active Reconnaissance | Gathering information by directly interacting with the target |

For example, looking up information about a domain using WHOIS is passive reconnaissance. Connecting to a server to check whether a port is open would be active reconnaissance.

Passive reconnaissance is generally less likely to be detected, but the information available to us can be more limited.

---

## Why Passive Reconnaissance Matters

Before attacking a target, we want to build a basic picture of what we are dealing with.

During passive reconnaissance, we might discover:

- Domain names and registered information
- IP addresses
- Nameservers
- DNS records
- Email addresses
- Subdomains
- Technologies used by a website
- Publicly exposed information
- Organisation or infrastructure details

This information can later be used to identify the target's attack surface and decide where further enumeration should be performed.

A useful way to think about reconnaissance is:

```text
Information → Understanding → Attack Surface → Enumeration
````

The more useful information we collect early on, the better prepared we are for the next stages of a penetration test.

---

## WHOIS

WHOIS is a protocol used to query information about registered domain names.

Depending on the registrar and privacy settings, a WHOIS lookup may provide information such as:

- Domain registration dates
    
- Registrar
    
- Nameservers
    
- Domain status
    
- Organisation or registrant information
    

A basic lookup can be performed with:

```bash
whois example.com
```

The information returned by WHOIS can help us understand who manages a domain and which infrastructure is associated with it.

However, WHOIS data is not always complete. Many domains use privacy protection, which hides personal or organisational information.

---

## DNS Information

The Domain Name System (DNS) translates domain names into information that computers can use, such as IP addresses.

DNS is particularly useful during reconnaissance because it can reveal different parts of an organisation's infrastructure.

For example, a domain may have records pointing to:

- Web servers
    
- Mail servers
    
- Nameservers
    
- Other domains
    
- Third-party services
    

Common DNS record types include:

|Record|Purpose|
|---|---|
|A|Maps a domain to an IPv4 address|
|AAAA|Maps a domain to an IPv6 address|
|CNAME|Creates an alias for another domain|
|MX|Identifies mail servers|
|NS|Identifies authoritative nameservers|
|TXT|Stores additional text information|
|SOA|Contains information about the DNS zone|

DNS reconnaissance is covered in more detail in [[DNS-Reconnaissance]].

---

## nslookup

`nslookup` is a command-line tool that can be used to query DNS information.

A simple lookup can be performed with:

```bash
nslookup example.com
```

We can also ask for a specific type of DNS record.

For example, to look for mail servers:

```bash
nslookup -type=MX example.com
```

Or to find the nameservers:

```bash
nslookup -type=NS example.com
```

`nslookup` is useful when we want a quick way to check how a domain resolves and what DNS information is publicly available.

---

## dig

`dig` is another DNS lookup tool. It provides more detailed output than `nslookup` and is commonly used when performing DNS reconnaissance.

A basic query looks like this:

```bash
dig example.com
```

We can request a specific record type:

```bash
dig example.com A
```

```bash
dig example.com MX
```

```bash
dig example.com NS
```

We can also query a specific nameserver:

```bash
dig @ns1.example.com example.com
```

When using `dig`, it is useful to pay attention to the **ANSWER** and **AUTHORITY** sections, as they can provide information about how the domain is configured.

---

## OSINT

Passive reconnaissance is closely related to **Open-Source Intelligence (OSINT)**.

OSINT is the process of collecting and analysing information from publicly available sources.

Examples include:

- Search engines
    
- Public websites
    
- DNS information
    
- WHOIS records
    
- Public documents
    
- Code repositories
    
- Social media
    
- Certificate transparency data
    
- Publicly exposed metadata
    

The important part is not simply collecting information. We need to understand what that information tells us about the target.

For example:

```text
Finding: mail.example.com
        ↓
DNS record points to an IP address
        ↓
Possible mail infrastructure identified
        ↓
Investigate the service during enumeration
```

This is where reconnaissance starts becoming useful for the rest of the penetration test.

See [[OSINT]] for more information.

---

## Reconnaissance Workflow

A simple passive reconnaissance workflow could look like this:

```text
Target Domain
     ↓
WHOIS
     ↓
DNS Information
     ↓
Nameservers
     ↓
DNS Records
     ↓
Subdomains
     ↓
Publicly Available Information
     ↓
Attack Surface
```

Each discovery can lead to another question.

For example:

> Who owns the domain?

Then:

> Which nameservers are being used?

Then:

> What DNS records exist?

Then:

> Are there other domains or subdomains?

Then:

> What infrastructure is exposed?

This process allows us to gradually build a better understanding of the target.

---

## From Information to Intelligence

Not every piece of information we find is immediately useful.

During reconnaissance, it is important to distinguish between simply finding something and understanding why it matters.

A useful approach is:

```text
Source → Observation → Interpretation → Next Action
```

For example:

```text
Source:
DNS lookup

Observation:
mail.example.com points to an external IP address

Interpretation:
The organisation may be using an externally hosted mail service

Next Action:
Document the infrastructure and investigate the service during enumeration
```

This prevents reconnaissance from becoming a collection of random facts.

---

## Limitations of Passive Reconnaissance

Passive reconnaissance has some limitations.

Public information may be:

- Outdated
    
- Incomplete
    
- Protected by privacy services
    
- Incorrect
    
- Hidden from search engines
    
- Hosted by third-party providers
    

Because of this, passive reconnaissance should not be treated as the complete picture of a target.

Instead, it gives us an initial understanding that can guide active reconnaissance and enumeration.

---

## Passive Reconnaissance and the Attack Surface

The information collected during passive reconnaissance helps us start building the target's attack surface.

For example:

```text
example.com
│
├── www.example.com
│   └── Web Application
│
├── mail.example.com
│   └── Mail Server
│
├── vpn.example.com
│   └── VPN Service
│
└── dev.example.com
    └── Development Environment
```

Each discovered system or service may represent a different entry point or area that needs to be investigated.

This is why reconnaissance is more than simply running tools. The real objective is to understand what the information means.

See [[Attack-Surface]] for more information.

---

## Related Topics

- [[Active-Reconnaissance]]
    
- [[DNS-Reconnaissance]]
    
- [[OSINT]]
    
- [[Subdomain-Enumeration]]
    
- [[Technology-Fingerprinting]]
    
- [[Information-Disclosure]]
    
- [[Attack-Surface]]
    
- [[Enumeration]]
    

---

## Summary

Passive reconnaissance allows us to gather information about a target without directly interacting with its infrastructure.

Some of the most useful tools and techniques include:

- `whois` for domain registration information
    
- `nslookup` for DNS queries
    
- `dig` for detailed DNS information
    
- OSINT for gathering information from public sources
    

The main objective is not to collect as much information as possible, but to collect useful information and understand how it can help us during the rest of the penetration test.

A good reconnaissance process should answer a simple question:

> **What can we learn about the target before we start touching its systems?**