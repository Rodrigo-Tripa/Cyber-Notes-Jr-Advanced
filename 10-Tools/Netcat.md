# Netcat

Netcat, commonly written as `nc`, is a small networking utility that can create connections between systems using TCP or UDP.

It is often described as the "Swiss army knife" of networking tools because it can be used for many different tasks.

At its core, however, Netcat is doing something fairly simple:

> **It creates network connections and allows us to send and receive data through them.**

Understanding this basic idea makes Netcat much easier to use.

Instead of memorising a collection of commands, it is better to understand what is happening underneath.

---

## What Is Netcat?

Netcat is a command-line networking utility capable of acting as both a client and a server.

It can:

- Connect to remote TCP services
- Listen for incoming TCP connections
- Send and receive data
- Work with UDP
- Read data from standard input
- Write received data to standard output
- Perform basic port connectivity tests
- Provide simple network communication between two systems

Netcat itself does not understand most application protocols.

For example, when connecting to an HTTP server, Netcat does not automatically understand HTTP.

It simply creates the connection and gives us a way to send bytes to the remote service and see the response.

This is one of the reasons it is so useful for understanding network protocols.

---

## The Basic Idea

A network connection normally has two sides.

One side acts as the **client** and initiates the connection.

The other side acts as the **server** and waits for connections.

With Netcat, either side can be created from the command line.

The basic model looks like this:

```text
Client                         Server
  │                              │
  │────── Connection ───────────>│
  │                              │
  │────── Data ─────────────────>│
  │                              │
  │<───── Data ──────────────────│
  │                              │
````

For TCP, the server first needs to listen on a port.

The client can then connect to that port.

For example:

```text
Server:
nc -lvnp 4444

Client:
nc <server-ip> 4444
```

Once the connection is established, data can flow between both sides.

---

## TCP Connections

Most Netcat usage is based around TCP.

TCP is a connection-oriented transport protocol.

Before application data is exchanged, TCP establishes a connection between the two endpoints.

Conceptually:

```text
Client                    Server
  │                         │
  │──── SYN ───────────────>│
  │<─── SYN/ACK ────────────│
  │──── ACK ───────────────>│
  │                         │
  │     TCP Connection      │
  │<───────────────────────>│
```

Once the connection has been established, applications can exchange data through it.

Netcat does not replace TCP.

It uses the operating system's networking stack to create and manage the socket.

---

## Sockets

To understand Netcat properly, it helps to understand the idea of a **socket**.

A socket is an operating-system interface that applications use to communicate over a network.

A TCP connection can be identified by information such as:

```text
Source IP
Source Port
Destination IP
Destination Port
Protocol
```

For example:

```text
192.168.1.20:49152
        │
        │ TCP
        ↓
192.168.1.10:4444
```

Here, `4444` is the destination port while `49152` could be an ephemeral source port selected by the operating system.

Netcat interacts with this socket through the operating system.

This means that when we run:

```bash
nc 192.168.1.10 4444
```

Netcat is essentially asking the operating system to create a TCP connection to:

```text
192.168.1.10:4444
```

---

## Client Mode

In client mode, Netcat initiates the connection.

The basic syntax is:

```bash
nc [options] <host> <port>
```

For example:

```bash
nc 192.168.1.10 80
```

Netcat attempts to establish a TCP connection to port `80` on `192.168.1.10`.

If the connection succeeds, Netcat provides an interface through which we can send data.

For example, we could manually interact with an HTTP service:

```text
GET / HTTP/1.1
Host: example.com
```

The server may then return an HTTP response.

This demonstrates something important:

Netcat does not need to understand HTTP.

We are manually sending HTTP data through a TCP connection.

---

## Server Mode

Netcat can also listen for incoming connections.

The basic idea is:

```bash
nc -lvnp 4444
```

The options commonly mean:

- `-l` — listen for an incoming connection
    
- `-v` — verbose output
    
- `-n` — do not perform DNS resolution
    
- `-p` — specify the local port
    

The result is a process waiting for a connection:

```text
Local Machine
     │
     │ Listening
     │
     ▼
TCP port 4444
```

Another system can then connect:

```bash
nc <server-ip> 4444
```

Once connected, both sides can exchange data.

---

## Listener vs Client

It is useful to keep these two concepts separate.

A listener waits.

A client connects.

For example:

```text
Machine A                         Machine B
Listener                          Client

nc -lvnp 4444
      ▲
      │
      │<──────── nc A 4444
      │
      └──── Connection ───────────
```

The listener needs a local port.

The client needs the destination IP address and destination port.

This same client/server model appears throughout networking.

---

## Standard Input and Output

One of the most important concepts behind Netcat is its relationship with standard input and output.

Normally, when we run:

```bash
nc 192.168.1.10 4444
```

we can type into the terminal.

That input is sent through the network connection.

Data received from the network is displayed in the terminal.

Conceptually:

```text
Keyboard
   ↓
stdin
   ↓
Netcat
   ↓
Network Socket
   ↓
Remote System
```

And in the opposite direction:

```text
Remote System
   ↓
Network Socket
   ↓
Netcat
   ↓
stdout
   ↓
Terminal
```

This is a major reason why Netcat can be combined with other Linux commands.

---

## Piping Data Through Netcat

Because Netcat uses standard input and output, it can be combined with shell pipelines.

For example:

```bash
echo "Hello" | nc 192.168.1.10 4444
```

The shell sends the output of `echo` into Netcat's standard input.

Netcat then sends that data through the network connection.

The flow is:

```text
echo
  ↓
stdout
  ↓
pipe
  ↓
Netcat stdin
  ↓
TCP socket
  ↓
Remote host
```

This is much more useful to understand than simply memorising the command.

It shows how Netcat fits into the Unix philosophy of combining small tools together.

---

## Redirecting Files

The same principle can be used with files.

For example:

```bash
nc 192.168.1.10 4444 < file.txt
```

The contents of `file.txt` become Netcat's standard input.

On the receiving side, the output can be redirected into a file:

```bash
nc -lvnp 4444 > received.txt
```

The basic data flow becomes:

```text
file.txt
   ↓
Netcat
   ↓
Network
   ↓
Netcat
   ↓
received.txt
```

This can be useful for understanding simple network transfers.

For security testing, file transfers should only be performed between systems where you have permission to do so.

---

## TCP vs UDP

Netcat can work with both TCP and UDP.

TCP is connection-oriented.

UDP is connectionless.

A TCP listener might look like:

```bash
nc -lvnp 4444
```

A UDP listener can be created with:

```bash
nc -lvnup 4444
```

The `-u` option tells Netcat to use UDP instead of TCP.

The important difference is what happens at the transport layer.

With TCP:

```text
Client
  │
  │ Establish connection
  ▼
Server
  │
  │ Exchange data
  ▼
```

With UDP:

```text
Client
  │
  │ Send datagram
  ▼
Server
```

There is no TCP-style connection establishment.

UDP communication is therefore different from TCP, even though Netcat gives us a similar command-line interface.

---

## Checking a Port

Netcat can also be used to check whether a TCP port accepts connections.

For example:

```bash
nc -zv 192.168.1.10 22
```

The `-z` option tells Netcat not to send application data and instead perform a connection check.

The `-v` option provides more information about what happened.

A successful connection indicates that something accepted the TCP connection.

For example:

```text
Connection to 192.168.1.10 22 port [tcp/ssh] succeeded!
```

This does not automatically mean that the service is vulnerable.

It only tells us that the TCP connection was successful.

This distinction is important during [[Enumeration]].

---

## Connecting to a Web Server

One of the best ways to understand Netcat is to use it to interact with a simple protocol.

HTTP is a good example.

We can connect to a web server:

```bash
nc example.com 80
```

Then send:

```text
GET / HTTP/1.1
Host: example.com
```

The server may respond with something similar to:

```text
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: ...
```

We have just interacted with an HTTP server without using a web browser.

The browser normally handles the HTTP protocol for us.

Netcat does not.

We provide the protocol data ourselves.

This makes Netcat useful for learning how protocols actually work.

---

## Banner Grabbing

Some services send information as soon as a connection is established.

This information is commonly called a **banner**.

For example:

```bash
nc 192.168.1.10 22
```

An SSH server might respond with something like:

```text
SSH-2.0-OpenSSH_9.2
```

This can reveal information about the service.

Banner grabbing is a simple form of service enumeration, although many services do not provide useful banners and some deliberately hide or modify version information.

See [[Technology-Fingerprinting]] and [[Enumeration]].

---

## Timeouts

Network connections do not always respond immediately.

A host might be unreachable, a firewall might silently drop traffic, or a service might simply not respond.

Netcat can use a timeout to avoid waiting indefinitely.

For example:

```bash
nc -zv -w 3 192.168.1.10 443
```

Here, `-w 3` specifies a three-second timeout.

Timeouts are especially useful when testing multiple hosts or ports where some connections may never receive a response.

---

## DNS Resolution

By default, networking tools may perform DNS lookups.

Netcat can be instructed not to resolve hostnames using:

```bash
-n
```

For example:

```bash
nc -nv 192.168.1.10 443
```

This tells Netcat to use the IP address directly rather than attempting reverse DNS resolution.

This can make output easier to interpret and can avoid unnecessary DNS lookups.

---

## Netcat as a Simple Network Tool

Netcat is not a full-featured scanner or protocol analyser.

Its strength comes from being simple.

It gives us a basic interface to a network socket:

```text
             Netcat
                │
        ┌───────┴───────┐
        │               │
     stdin           stdout
        │               │
        └───────┬───────┘
                │
          Network Socket
                │
        TCP or UDP
                │
           Remote Host
```

Because it stays close to the network layer, it is useful for testing connectivity and interacting with services manually.

---

## Netcat and the OSI Model

Netcat primarily operates around the transport and application boundary.

It uses transport protocols such as:

- TCP
    
- UDP
    

The operating system handles the lower-level networking details.

For example:

```text
Application
    │
    │ Netcat
    ▼
Transport
    │
    ├── TCP
    └── UDP
    │
    ▼
Internet
    │
    └── IP
    │
    ▼
Network Interface
```

Netcat does not implement Ethernet or IP itself.

It relies on the operating system's network stack to handle those layers.

---

## Netcat in Penetration Testing

Netcat is useful during several stages of a penetration test.

During reconnaissance and enumeration, it can help us:

- Check whether a port accepts TCP connections
    
- Connect directly to a service
    
- Inspect simple service banners
    
- Manually interact with protocols
    
- Test network connectivity
    

It can also be useful during post-exploitation for legitimate testing tasks such as transferring data between systems, depending on the engagement's scope.

However, Netcat should not be treated as a replacement for specialised tools.

For example:

```text
Netcat
  ↓
Basic connectivity and interaction

Nmap
  ↓
Port and service discovery

Wireshark
  ↓
Packet capture and protocol analysis
```

Each tool solves a different problem.

---

## Netcat and Reverse Connections

Netcat is also commonly associated with reverse connections.

The basic concept is that instead of the tester connecting directly to a target service, the target system initiates a connection back to a listening system.

Conceptually:

```text
Normal Connection

Tester ───────────────> Target
        Connection


Reverse Connection

Tester <─────────────── Target
        Connection
```

This concept is important in penetration testing because outbound connections may be allowed even when inbound connections are restricted.

However, the exact behaviour depends on the target environment, firewall rules, network segmentation, and the specific Netcat implementation.

It is therefore important to understand the networking concept rather than treating a particular command as universal.

---

## Different Netcat Implementations

One important detail is that there is not just one implementation of Netcat.

Different systems may provide different versions, such as:

- Traditional Netcat
    
- OpenBSD Netcat
    
- Ncat
    
- Other platform-specific implementations
    

Because of this, available options can differ between systems.

Before relying on a particular option, check the local implementation:

```bash
nc -h
```

or:

```bash
man nc
```

For example, an option available in one implementation may behave differently or not exist in another.

This is especially important when moving between Linux distributions.

---

## A Simple Mental Model

Instead of memorising dozens of Netcat commands, keep this model in mind:

```text
1. Decide who listens.
2. Decide who connects.
3. Choose TCP or UDP.
4. Choose the port.
5. Establish communication.
6. Send and receive data.
```

For example:

```text
Server:
nc -lvnp 4444

Client:
nc 192.168.1.10 4444
```

The server waits.

The client connects.

Once the connection exists, Netcat moves data between the terminal and the network socket.

That is the core of the tool.

---

## Basic Syntax

The general syntax is:

```bash
nc [options] [host] [port]
```

Some common options include:

|Option|Purpose|
|---|---|
|`-l`|Listen for incoming connections|
|`-v`|Verbose output|
|`-n`|Do not resolve hostnames|
|`-u`|Use UDP instead of TCP|
|`-z`|Check for listening services without sending data|
|`-w`|Set a connection timeout|
|`-p`|Specify a local port in implementations that support it|

Remember that available options depend on the Netcat implementation installed on the system.

---

## Basic Examples

### Listen on TCP port 4444

```bash
nc -lvnp 4444
```

### Connect to a TCP port

```bash
nc 192.168.1.10 4444
```

### Check whether a TCP port is accepting connections

```bash
nc -zv 192.168.1.10 22
```

### Connect using UDP

```bash
nc -u 192.168.1.10 4444
```

### Set a timeout

```bash
nc -zv -w 3 192.168.1.10 443
```

### Send text to a remote listener

```bash
echo "Hello" | nc 192.168.1.10 4444
```

These examples are intentionally simple. The important part is understanding what Netcat is doing underneath each command.

---

## Security Considerations

Netcat is a powerful low-level networking utility, but it does not provide security by itself.

A normal Netcat TCP connection is not encrypted.

If sensitive information is sent through it, that information may be visible to anyone capable of observing the traffic.

Netcat also does not automatically authenticate the remote endpoint.

For these reasons, it should not be treated as a secure replacement for protocols such as SSH or TLS.

When using Netcat during a penetration test, always make sure that the activity is authorised and within the defined scope.

---

## Related Topics

- [[Network-Pentesting]]
    
- [[Active-Reconnaissance]]
    
- [[Enumeration]]
    
- [[Technology-Fingerprinting]]
    
- [[Exploitation]]
    
- [[Post-Exploitation]]
    
- [[Attack-Surface]]
    
- [[Reporting]]
    

---

## Summary

Netcat is a simple tool for creating network connections and moving data through them.

Its most important concepts are:

```text
Client
Server
Socket
TCP / UDP
stdin
stdout
Port
Connection
```

The basic model is:

```text
Terminal
   ↓
Netcat
   ↓
Socket
   ↓
TCP / UDP
   ↓
Network
   ↓
Remote System
```

Once this model makes sense, most Netcat commands become much easier to understand.

Rather than thinking of Netcat as a collection of commands to memorise, think of it as a simple interface to network sockets.

That is what makes it useful: it gives us a direct and uncomplicated way to interact with network services and understand what is happening underneath higher-level tools.