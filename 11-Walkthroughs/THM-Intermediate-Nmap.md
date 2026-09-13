# Intermediate Nmap — Nmap, Netcat and SSH

> TryHackMe: [Intermediate Nmap](https://tryhackme.com/room/intermediatenmap)

## Reconnaissance

I started by performing a full TCP port scan against the target:

```bash
nmap -p- -sT -T5 10.129.150.225
```

The scan revealed a high-numbered open port:

```text
31337/tcp open  elite
```

Because the challenge mentioned that a high port could provide information for accessing a lower port, I investigated this service.

## Netcat

I connected to port `31337` using Netcat:

```bash
nc 10.129.150.225 31337
```

The service returned credentials for the `ubuntu` user.

This was the key piece of information needed to continue.

## SSH

The challenge mentioned a lower port commonly used for remote access. SSH normally runs on TCP port `22`, so I used the credentials obtained from the service to connect:

```bash
ssh ubuntu@10.129.150.225
```

The login was successful and I obtained a shell on the target as the `ubuntu` user.

## Finding the Flag

After gaining access to the machine, I performed basic local enumeration and located the flag.

The flag is omitted from this write-up to comply with TryHackMe's write-up requirements.

## Attack Path

The complete attack chain was:

1. Full TCP scan with Nmap.
2. Discovery of port `31337`.
3. Connection to the service using Netcat.
4. Retrieval of valid credentials.
5. SSH access through port `22`.
6. Local enumeration.
7. Flag retrieval.

## Lessons Learned

This room demonstrated the importance of scanning beyond the default top 1000 TCP ports.

It also showed how different tools can be combined during enumeration. Nmap identified the exposed service, Netcat allowed direct interaction with it, and the information obtained was then used to access SSH.

The main lesson was to follow the information provided by each service instead of immediately trying to exploit everything discovered.
