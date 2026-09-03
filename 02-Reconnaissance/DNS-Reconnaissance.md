# DNS Reconnaissance

The Domain Name System (DNS) is responsible for translating domain names into information that computers can understand.

When we type a domain such as `example.com` into a browser, DNS helps determine where that domain should point.

For penetration testing, DNS can be very useful because it can reveal information about the target's infrastructure.

A single domain can have multiple DNS records pointing to different servers and services. By understanding these records, we can start building a better picture of the organisation's attack surface.

---

## Why DNS Matters During Reconnaissance

DNS is more than just a way of finding an IP address.

A domain can expose information about:

- Web servers
- Mail servers
- Nameservers
- Subdomains
- Third-party services
- Internal or development infrastructure
- Email security configuration
- Other infrastructure associated with the organisation

For example:

```text
example.com
│
├── www.example.com
│   └── Web Server
│
├── mail.example.com
│   └── Mail Server
│
├── vpn.example.com
│   └── VPN Service
│
└── dev.example.com
    └── Development Server
````

Not every hostname will necessarily be reachable or interesting, but each discovery gives us another piece of information about the target.

This is why DNS reconnaissance is an important part of [[Passive-Reconnaissance]].

---

## How DNS Works

DNS uses a hierarchical structure.

In a simplified example, when we request:

```text
www.example.com
```

the query eventually needs to find the authoritative DNS server responsible for `example.com`.

A simplified flow looks like this:

```text
Domain
   ↓
DNS Resolver
   ↓
Root DNS Servers
   ↓
TLD DNS Servers
   ↓
Authoritative Nameserver
   ↓
DNS Record
   ↓
IP Address / Service
```

In practice, recursive resolvers and caching make this process more efficient, but the important idea is that DNS is distributed rather than being controlled by a single server.

---

## DNS Record Types

Different DNS records provide different types of information.

|Record|Purpose|
|---|---|
|A|Maps a hostname to an IPv4 address|
|AAAA|Maps a hostname to an IPv6 address|
|CNAME|Creates an alias pointing to another hostname|
|MX|Identifies mail servers|
|NS|Identifies authoritative nameservers|
|TXT|Stores text information associated with a domain|
|SOA|Contains information about the DNS zone|
|PTR|Performs reverse DNS lookups|
|SRV|Defines the location of specific services|

Understanding these records makes it much easier to interpret DNS enumeration results.

---

## A Records

An `A` record maps a hostname to an IPv4 address.

For example:

```text
example.com → 93.184.216.34
```

We can query it using `dig`:

```bash
dig example.com A
```

The returned IP address can then be investigated further during [[Active-Reconnaissance]] and [[Enumeration]].

---

## AAAA Records

`AAAA` records work in a similar way to `A` records, but they point to IPv6 addresses.

For example:

```bash
dig example.com AAAA
```

IPv6 infrastructure is sometimes overlooked during reconnaissance, so checking for `AAAA` records can reveal additional infrastructure that might otherwise be missed.

---

## CNAME Records

A `CNAME` record creates an alias for another hostname.

For example:

```text
www.example.com → example.com
```

We can query it with:

```bash
dig www.example.com CNAME
```

CNAME records can also point to third-party services.

For example:

```text
blog.example.com → example.wordpress.com
```

This can give us clues about services or providers being used by the target.

CNAME records can become particularly interesting during [[Subdomain-Enumeration]] because they may reveal relationships between different services.

---

## MX Records

`MX` records identify the mail servers responsible for receiving email for a domain.

For example:

```bash
dig example.com MX
```

A result might point to something such as:

```text
mail.example.com
```

or an external mail provider.

This gives us information about the organisation's email infrastructure and can also help identify third-party services.

---

## NS Records

`NS` records identify the authoritative nameservers for a domain.

We can query them using:

```bash
dig example.com NS
```

For example:

```text
ns1.example.com
ns2.example.com
```

Knowing the authoritative nameservers helps us understand where the DNS zone is managed.

It can also provide useful information for further DNS enumeration.

---

## TXT Records

`TXT` records allow domain owners to store text information in DNS.

They are commonly used for things such as:

- SPF
    
- Domain verification
    
- Email security
    
- Service verification
    
- Other configuration information
    

We can query them with:

```bash
dig example.com TXT
```

TXT records can sometimes reveal information about services used by an organisation.

For example, an SPF record may show which systems are authorised to send email for the domain.

---

## SOA Records

The `SOA` record, or Start of Authority, contains information about the DNS zone.

It can provide details such as:

- Primary nameserver
    
- Responsible party
    
- Serial number
    
- Refresh interval
    
- Retry interval
    
- Expiration time
    

We can query it with:

```bash
dig example.com SOA
```

The SOA record is useful for understanding how the DNS zone is configured.

---

## PTR Records and Reverse DNS

DNS does not only work from domain names to IP addresses.

A reverse DNS lookup attempts to find the hostname associated with an IP address.

This is commonly done using a `PTR` record.

For example:

```bash
dig -x 93.184.216.34
```

Reverse DNS can sometimes reveal hostnames that are not immediately obvious from the main domain.

The information is not guaranteed to exist, but when it does, it can provide another useful clue.

---

## Using nslookup

`nslookup` is a simple tool for querying DNS information.

A basic query:

```bash
nslookup example.com
```

We can request specific record types as well.

For example:

```bash
nslookup -type=MX example.com
```

```bash
nslookup -type=NS example.com
```

```bash
nslookup -type=TXT example.com
```

`nslookup` is useful when we want a quick answer without dealing with the larger amount of information that `dig` can return.

---

## Using dig

`dig` is particularly useful for DNS reconnaissance because it gives us detailed information about DNS responses.

A basic query:

```bash
dig example.com
```

A specific record:

```bash
dig example.com A
```

Multiple useful queries might look like:

```bash
dig example.com A
dig example.com AAAA
dig example.com MX
dig example.com NS
dig example.com TXT
dig example.com SOA
```

We can also query a specific nameserver:

```bash
dig @ns1.example.com example.com
```

When reading the output, pay attention to the `ANSWER` section as well as the `AUTHORITY` section.

The goal is not just to run `dig`, but to understand what the response tells us about the target.

---

## Nameservers

Nameservers are responsible for answering DNS queries for a domain.

A domain will usually have multiple authoritative nameservers.

For example:

```text
example.com
    │
    ├── ns1.example.com
    └── ns2.example.com
```

Having more than one nameserver provides redundancy if one becomes unavailable.

During reconnaissance, nameservers can also give us clues about:

- The DNS provider
    
- The organisation's infrastructure
    
- Third-party hosting
    
- DNS configuration
    
- Other domains or services
    

This information can be useful when expanding the attack surface.

---

## Zone Transfers

A DNS zone transfer is a mechanism used to copy DNS zone information between DNS servers.

The protocol uses DNS queries such as `AXFR`.

A properly configured DNS server should only allow zone transfers to authorised servers.

If a server incorrectly allows anyone to request a zone transfer, it may expose a large amount of DNS information.

For example:

```bash
dig axfr example.com @ns1.example.com
```

If the transfer is allowed, the response may contain multiple records from the zone.

This can potentially reveal:

- Hostnames
    
- Internal naming conventions
    
- Server addresses
    
- Development systems
    
- Other DNS records
    

Zone transfers should only be tested when they are explicitly allowed by the scope of the engagement.

---

## DNS and Subdomain Enumeration

DNS reconnaissance naturally leads into [[Subdomain-Enumeration]].

Once we know how a domain is configured, we can start looking for additional hostnames.

For example:

```text
example.com
│
├── www.example.com
├── mail.example.com
├── vpn.example.com
├── dev.example.com
└── api.example.com
```

Each hostname may represent a different application or service.

Some may be public-facing while others may be intended for internal or development use.

This is why discovering a new subdomain should not be treated as the end of reconnaissance.

It is usually the beginning of another investigation.

---

## Email Security Records

DNS can also tell us a lot about how an organisation handles email.

Three important technologies are:

- SPF
    
- DKIM
    
- DMARC
    

These are commonly represented through DNS records.

For example, SPF information can often be found in a TXT record:

```bash
dig example.com TXT
```

DMARC records are normally stored under:

```text
_dmarc.example.com
```

and can be queried with:

```bash
dig _dmarc.example.com TXT
```

These records are primarily designed for email security, but they can also reveal information about the organisation's email infrastructure and third-party providers.

---

## Reading DNS Information as an Attacker

Finding a DNS record is only the first step.

We need to ask what the record tells us.

For example:

```text
Finding:
vpn.example.com → 203.0.113.10

Observation:
A public hostname points to an external IP address

Interpretation:
The organisation appears to expose a VPN service

Next Action:
Document the host and investigate the service during enumeration
```

Another example:

```text
Finding:
dev.example.com → 203.0.113.20

Observation:
A development-related hostname is publicly resolvable

Interpretation:
A development environment may be exposed to the Internet

Next Action:
Investigate the host and determine what services are available
```

This is the mindset we want to develop throughout reconnaissance:

```text
DNS Record → Meaning → Attack Surface → Next Action
```

---

## DNS Reconnaissance Workflow

A simple workflow could look like this:

```text
Target Domain
      ↓
Identify Nameservers
      ↓
Query DNS Records
      ↓
A / AAAA / CNAME / MX / NS / TXT / SOA
      ↓
Identify Hostnames and Services
      ↓
Discover Additional Subdomains
      ↓
Map Infrastructure
      ↓
Build Attack Surface
      ↓
Enumeration
```

In a real engagement, this process is not always linear.

Finding one hostname may lead to another domain. A CNAME may reveal a third-party service. A DNS record may lead to an IP address that needs further investigation.

Reconnaissance is therefore an iterative process.

---

## Common Mistakes

There are a few easy mistakes to make when performing DNS reconnaissance.

### Only checking the A record

An A record may give us an IP address, but it is only one part of the picture.

Other records may reveal mail infrastructure, nameservers, aliases, or additional information.

### Ignoring IPv6

If a domain has an `AAAA` record, there may be infrastructure available over IPv6 that would not be discovered by looking only at IPv4.

### Ignoring CNAME records

CNAME records can reveal relationships with third-party services and can sometimes point us towards additional infrastructure.

### Treating DNS as definitive

DNS information can be outdated.

A hostname resolving to an IP address does not necessarily mean that the service is currently active or vulnerable.

DNS gives us leads. Those leads still need to be investigated.

---

## DNS and the Attack Surface

DNS reconnaissance helps us transform a single domain into a much larger picture of the target.

For example:

```text
example.com
│
├── www.example.com
│   └── Web Application
│
├── api.example.com
│   └── API
│
├── mail.example.com
│   └── Mail Infrastructure
│
├── vpn.example.com
│   └── VPN
│
└── dev.example.com
    └── Development Environment
```

What initially looked like one target may actually contain several different systems.

This is one of the reasons DNS is so valuable during reconnaissance.

It helps us answer:

> **What other infrastructure is connected to this domain?**

See [[Attack-Surface]] for more information.

---

## Related Topics

- [[Passive-Reconnaissance]]
    
- [[Active-Reconnaissance]]
    
- [[OSINT]]
    
- [[Subdomain-Enumeration]]
    
- [[Attack-Surface]]
    
- [[Information-Disclosure]]
    
- [[Technology-Fingerprinting]]
    
- [[Enumeration]]
    

---

## Summary

DNS reconnaissance is the process of gathering and analysing DNS information about a target.

Useful tools include:

- `nslookup`
    
- `dig`
    
- `whois`
    

Important DNS records to understand include:

- `A`
    
- `AAAA`
    
- `CNAME`
    
- `MX`
    
- `NS`
    
- `TXT`
    
- `SOA`
    
- `PTR`
    
- `SRV`
    

The important part is not memorising every DNS record.

It is understanding what the records tell us about the target's infrastructure.

A useful way to think about DNS reconnaissance is:

```text
Domain
  ↓
DNS Records
  ↓
Hostnames
  ↓
Infrastructure
  ↓
Attack Surface
  ↓
Enumeration
```

DNS gives us another way to move from a simple domain name to a much better understanding of the systems behind it.

When combined with [[Passive-Reconnaissance]], [[Active-Reconnaissance]], and [[Subdomain-Enumeration]], DNS becomes a powerful source of information during the early stages of a penetration test.