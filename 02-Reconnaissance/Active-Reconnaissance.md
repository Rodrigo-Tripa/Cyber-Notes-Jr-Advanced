# Active Reconnaissance

Active reconnaissance is the process of gathering information by directly interacting with the target.

Unlike [[Passive-Reconnaissance]], we are no longer relying only on public information. We are sending traffic to the target and observing how it responds.

This can help us discover things that passive reconnaissance cannot, such as whether a host is reachable, what network path traffic takes, which services respond, and how a web server behaves.

Because we are directly interacting with the target, active reconnaissance is more likely to be detected and should always be performed within the agreed scope of a penetration test.

---

## Passive vs Active Reconnaissance

The main difference is whether we directly interact with the target.

| Type | Description |
|---|---|
| Passive Reconnaissance | Collecting information without directly interacting with the target |
| Active Reconnaissance | Collecting information by directly interacting with the target |

For example, finding an IP address through public DNS information is passive reconnaissance.

Sending a `ping` request to that IP address is active reconnaissance.

The two approaches work together:

```text
Passive Reconnaissance
        ↓
Initial Information
        ↓
Active Reconnaissance
        ↓
More Accurate Information
        ↓
Enumeration
````

Passive reconnaissance gives us an initial picture. Active reconnaissance lets us start testing whether that picture matches reality.

---

## Why Active Reconnaissance Matters

The information we find during passive reconnaissance is useful, but it does not always tell us what is actually reachable.

A domain might resolve to an IP address, but that does not necessarily mean the server is currently reachable.

A hostname might suggest that a service exists, but we still need to understand how the target responds.

Active reconnaissance helps answer questions such as:

- Is the host reachable?
    
- What network path exists between us and the target?
    
- Does a particular service respond?
    
- What does a web server return?
    
- Does the target expose information through its responses?
    
- Are there differences between what we expected and what we actually observe?
    

The goal is to turn assumptions into observations.

---

## Ping

`ping` is one of the simplest tools we can use during active reconnaissance.

It sends an ICMP Echo Request to a target and waits for an Echo Reply.

For example:

```bash
ping example.com
```

A successful response can tell us that the target is reachable and give us information such as:

- The resolved IP address
    
- Response time
    
- Packet loss
    
- Whether ICMP traffic is allowed
    

A typical response might look like:

```text
64 bytes from 93.184.216.34: icmp_seq=1 ttl=56 time=20.4 ms
```

The response time can give us a rough idea of network latency.

However, a failed `ping` does **not** automatically mean that the host is offline.

Firewalls and network configurations can block ICMP traffic while still allowing other services to be reached.

So instead of thinking:

```text
No ping response = Host is down
```

Think:

```text
No ping response = ICMP did not receive a response
```

This distinction becomes important during real-world reconnaissance.

---

## Traceroute

`traceroute` is used to discover the network path between our machine and a target.

On Linux, we can run:

```bash
traceroute example.com
```

The Windows equivalent is:

```cmd
tracert example.com
```

Instead of only telling us whether a target responds, traceroute shows the intermediate hops that traffic takes to reach it.

For example:

```text
1    192.168.1.1
2    10.0.0.1
3    203.0.113.1
4    93.184.216.34
```

Each hop represents a device or routing point along the path.

This can help us understand:

- How traffic reaches the target
    
- Where the target may be located in the network
    
- Network boundaries
    
- Possible filtering points
    
- Approximate network distance
    

Not every hop will necessarily respond. Firewalls and routers may intentionally hide themselves, so seeing `* * *` does not automatically mean something is broken.

Traceroute is therefore useful for understanding the path, not for assuming that every hop must be visible.

---

## Telnet

`telnet` can be useful during reconnaissance because it allows us to connect directly to a specific TCP port.

For example:

```bash
telnet example.com 80
```

If the connection succeeds, we know that something is accepting TCP connections on that port.

We can then interact with the service manually.

For example, when connecting to a web server on port 80, we could send a basic HTTP request:

```text
GET / HTTP/1.1
Host: example.com
```

The server may respond with HTTP headers and other information.

This can give us useful clues about the service running behind the port.

However, Telnet itself is an insecure protocol because its traffic is sent without encryption. It should not be used for secure administration.

During penetration testing, we are mainly interested in its ability to establish a raw TCP connection and interact with services.

---

## Web Browser Reconnaissance

A web browser is also a useful reconnaissance tool.

When we visit a website, we are interacting directly with the target's web server.

For example:

```text
https://example.com
```

The browser allows us to observe things such as:

- HTTP responses
    
- Redirects
    
- Page content
    
- Cookies
    
- Response headers
    
- Error messages
    
- Authentication pages
    
- Technologies used by the application
    

Sometimes the website itself reveals information that was not obvious from passive reconnaissance.

For example, a page might contain:

```text
Powered by Example CMS
```

or a response header might reveal information about the web server.

This is where active reconnaissance starts to overlap with [[Technology-Fingerprinting]] and [[Information-Disclosure]].

---

## Following Redirects

Web applications often redirect users from one location to another.

For example:

```text
http://example.com
        ↓
https://example.com
        ↓
https://www.example.com
```

Following these redirects can reveal additional hostnames, domains, or application behaviour.

Redirects can also tell us something about how the application is configured.

It is worth paying attention to where the application sends us rather than only looking at the final page.

---

## What We Learn From Responses

One of the most important skills in active reconnaissance is learning how to interpret responses.

A tool does not automatically give us useful intelligence.

For example:

```text
Tool:
ping

Observation:
Host responds with 20 ms latency

Interpretation:
The target is reachable through ICMP

Next Action:
Continue investigating the target
```

Or:

```text
Tool:
Browser

Observation:
Application redirects to /login

Interpretation:
The target exposes an authentication interface

Next Action:
Document the application and investigate it during enumeration
```

A useful mindset is:

```text
Tool → Observation → Interpretation → Next Action
```

This keeps reconnaissance focused on understanding the target instead of simply running commands.

---

## Detection and Noise

Active reconnaissance creates traffic.

This means that the target may be able to detect what we are doing.

Depending on the environment, activity may appear in:

- Firewall logs
    
- Web server logs
    
- IDS/IPS alerts
    
- Authentication logs
    
- Network monitoring systems
    

For example, repeatedly connecting to a service or sending large numbers of requests is much more noticeable than simply viewing a public webpage.

This is why the scope and rules of engagement are important.

Before performing active reconnaissance, we should know:

- What systems are in scope
    
- Which IP addresses are allowed
    
- Which services can be tested
    
- Which techniques are allowed
    
- Whether stealth is required
    
- What actions are explicitly prohibited
    

See [[Rules-of-Engagement]] for more information.

---

## Active Reconnaissance Workflow

A simple workflow could look like this:

```text
Known Target
     ↓
Ping
     ↓
Is the host reachable?
     ↓
Traceroute
     ↓
Understand the network path
     ↓
Service Interaction
     ↓
Observe Responses
     ↓
Web Reconnaissance
     ↓
Identify Applications and Technologies
     ↓
Enumeration
```

We do not necessarily have to follow this exact order every time.

Real penetration tests are usually iterative.

One discovery can lead us back to reconnaissance:

```text
Web Application
      ↓
New Hostname Found
      ↓
DNS Reconnaissance
      ↓
New IP Address
      ↓
Active Reconnaissance
      ↓
New Service
      ↓
Enumeration
```

The important part is being able to follow the information wherever it leads.

---

## From Reconnaissance to Enumeration

Active reconnaissance helps us move from a general understanding of the target to a more detailed view of its infrastructure.

For example:

```text
Target
  ↓
Host is reachable
  ↓
Network path identified
  ↓
Service responds
  ↓
Application discovered
  ↓
Technology identified
  ↓
Enumeration
```

At this point, we should start asking more specific questions about the discovered services.

This is where [[Enumeration]] becomes important.

Reconnaissance tells us **what might be there**.

Enumeration helps us understand **what is actually there**.

---

## Things to Keep in Mind

Active reconnaissance is relatively simple from a technical perspective, but the important part is interpreting what we see.

A few things are worth remembering:

- A host not responding to `ping` does not mean it is offline.
    
- A missing traceroute hop does not necessarily mean there is a network problem.
    
- An open TCP connection does not automatically tell us which service is running.
    
- A website can reveal useful information through headers, redirects, errors, and content.
    
- Active reconnaissance generates traffic and can be detected.
    
- Always stay within the agreed scope of the engagement.
    

The tools are easy to learn.

Understanding what their results actually mean is the more important skill.

---

## Related Topics

- [[Passive-Reconnaissance]]
    
- [[DNS-Reconnaissance]]
    
- [[Attack-Surface]]
    
- [[Enumeration]]
    
- [[Technology-Fingerprinting]]
    
- [[Information-Disclosure]]
    
- [[Subdomain-Enumeration]]
    
- [[Rules-of-Engagement]]
    

---

## Summary

Active reconnaissance involves directly interacting with a target to learn more about its systems and services.

Some of the basic tools and techniques include:

- `ping` for checking ICMP reachability
    
- `traceroute` for understanding the network path
    
- `telnet` for interacting with TCP services
    
- Web browsers for observing web applications and their responses
    

The most important part is not the commands themselves.

It is the process of taking a response, understanding what it tells us, and deciding what to investigate next.

A good way to remember the process is:

```text
Interact → Observe → Understand → Investigate
```

Active reconnaissance turns the information gathered during [[Passive-Reconnaissance]] into a more realistic picture of the target and helps prepare us for [[Enumeration]].