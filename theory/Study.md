# 90 Days of Cybersecurity: The Text Version

> A read-instead-of-watch companion to the [90DaysOfCyberSecurity](https://github.com/farhanashrafdev/90DaysOfCyberSecurity) repo.
> Everything the repo points you to (videos, courses, labs) is explained here in plain text, so you can learn the concepts by reading first and use the original resources only for practice and depth.

---

## How to use this file in VS Code

| Action | Shortcut (Windows/Linux) | Shortcut (Mac) |
|---|---|---|
| Open Markdown preview | `Ctrl+Shift+V` | `Cmd+Shift+V` |
| Preview side by side | `Ctrl+K` then `V` | `Cmd+K` then `V` |
| Jump to a heading | `Ctrl+Shift+O` | `Cmd+Shift+O` |
| Show Outline panel | Explorer sidebar > **Outline** | same |
| Tick a checkbox | edit `[ ]` to `[x]` | same |

Tips:

- Use the **Outline** panel (left sidebar) as a table of contents. Every day-block below is a heading.
- Install the extension **Markdown All in One** for a live table of contents and checkbox toggling (`Alt+C`).
- Save the file inside a Git repo and commit after each day. This gives you Day 57 (Git) practice for free.
- Code blocks are meant to be typed out, not pasted. Typing builds muscle memory.

---

## Table of contents

1. [What this plan is and how to use it](#1-what-this-plan-is-and-how-to-use-it)
2. [Days 1-7: Network+ (networking fundamentals)](#2-days-1-7-network-networking-fundamentals)
3. [Days 8-14: Security+ (security concepts)](#3-days-8-14-security-security-concepts)
4. [Days 15-28: Linux](#4-days-15-28-linux)
5. [Days 29-42: Python](#5-days-29-42-python-for-security)
6. [Days 43-56: Traffic analysis](#6-days-43-56-traffic-analysis)
7. [Days 57-63: Git](#7-days-57-63-git)
8. [Days 64-70: ELK stack](#8-days-64-70-elk-stack)
9. [Days 71-77: Cloud platforms](#9-days-71-77-cloud-platforms-gcp-aws-azure)
10. [Days 78-84: Review and practice](#10-days-78-84-review-and-practice)
11. [Days 85-90: Hacking](#11-days-85-90-ethical-hacking)
12. [Bonus: Landing the job](#12-bonus-landing-the-job-days-91-95)
13. [Resource index (every link from the repo)](#13-resource-index)
14. [Glossary](#14-glossary)
15. [Progress tracker](#15-progress-tracker)

---

# 1. What this plan is and how to use it

## The big picture

The repo is a 90-day self-study path. It goes from "how do computers talk to each other" to "how do I find and exploit weaknesses" to "how do I get hired".

| Days | Topic | What you gain |
|---|---|---|
| 1-7 | Network+ | How networks work: IPs, ports, protocols, devices |
| 8-14 | Security+ | The vocabulary and concepts of security |
| 15-28 | Linux | The operating system most security tools run on |
| 29-42 | Python | Automate tasks, write small security tools |
| 43-56 | Traffic analysis | Read network packets, detect attacks |
| 57-63 | Git | Version control, working like a professional |
| 64-70 | ELK | Collect and search logs (SIEM basics) |
| 71-77 | Cloud | Security in AWS / GCP / Azure |
| 78-84 | Review + practice | Home lab, TryHackMe, a small project |
| 85-90 | Hacking | Practice on vulnerable machines (HTB, VulnHub) |
| 91-95 | Resume + job search | One-page resume, applications |

## The "Start Here" rules from the repo

1. **Time budget: 1-2 focused hours a day.** Day numbers are guidance, not a deadline.
2. **Follow the order.** Days 1-28 are the foundation. Everything later assumes you know networking, security basics and Linux. Do not jump to hacking.
3. **Certifications are optional.** The Network+ and Security+ material teaches the concepts. You do not have to sit the exams.
4. **Track progress.** Tick boxes in section 15 of this file.
5. **Everything is free.** If something is paywalled, use the alternative listed.
6. **Questions:** use GitHub Discussions. Issues are for broken links.

## A study method that works with reading

For each day:

1. **Read** the section (15-30 min).
2. **Do** the lab or command examples yourself (30-60 min).
3. **Write** 3-5 lines in your own words about what you learned (10 min).
4. **Quiz yourself** the next morning on yesterday's terms (5 min).

Reading alone is the weakest way to learn security. Whenever you see a command or a config, run it.

---

# 2. Days 1-7: Network+ (networking fundamentals)

**Source in repo:** Professor Messer's CompTIA Network+ N10-009 playlist.
**Goal:** understand how data moves across networks well enough to spot what is normal and what is not.

The N10-009 exam has five domains. This guide follows them.

| Domain | Topic |
|---|---|
| 1 | Networking concepts |
| 2 | Network implementation |
| 3 | Network operations |
| 4 | Network security |
| 5 | Network troubleshooting |

## Day 1: Networking concepts, part 1 (models and addressing)

### 2.1 The OSI model

A 7-layer model that describes how data moves from an application on one machine to an application on another. Memorize it, because security people say "that's a layer 7 attack" or "the problem is at layer 2" all the time.

| # | Layer | What it does | Data unit | Examples |
|---|---|---|---|---|
| 7 | Application | Interface for user apps | Data | HTTP, DNS, SMTP, FTP |
| 6 | Presentation | Encoding, encryption, compression | Data | TLS (partly), JPEG, ASCII |
| 5 | Session | Opens, keeps and closes sessions | Data | NetBIOS, RPC |
| 4 | Transport | End-to-end delivery, ports | Segment (TCP) / Datagram (UDP) | TCP, UDP |
| 3 | Network | Logical addressing, routing | Packet | IP, ICMP, routers |
| 2 | Data Link | Local delivery using MAC addresses | Frame | Ethernet, switches, ARP (between 2 and 3) |
| 1 | Physical | Bits on the wire or air | Bits | Cables, Wi-Fi radio, hubs |

Mnemonic (top to bottom): **A**ll **P**eople **S**eem **T**o **N**eed **D**ata **P**rocessing.

**Encapsulation:** as data goes down the layers, each layer wraps it in a header (and sometimes a trailer). The receiving side unwraps in reverse. In Wireshark you will see exactly this: an Ethernet header, containing an IP header, containing a TCP header, containing HTTP data.

### 2.2 The TCP/IP model

The real-world model, with 4 layers:

| TCP/IP layer | Matches OSI |
|---|---|
| Application | 5, 6, 7 |
| Transport | 4 |
| Internet | 3 |
| Network access (link) | 1, 2 |

### 2.3 MAC addresses vs IP addresses

- **MAC address:** 48-bit hardware identifier, for example `00:1A:2B:3C:4D:5E`. Works only on the local network segment. First half identifies the vendor (OUI).
- **IP address:** logical address that can change. Used to route across networks.
- **ARP (Address Resolution Protocol):** "Who has 192.168.1.10? Tell 192.168.1.5". Maps IP to MAC on a local network. ARP has no authentication, which is why **ARP spoofing** works.

### 2.4 IPv4 addressing

An IPv4 address is 32 bits, written as four decimal octets: `192.168.1.25`.

**Private ranges (RFC 1918)**. These are not routed on the public internet:

| Range | CIDR | Typical use |
|---|---|---|
| 10.0.0.0 - 10.255.255.255 | 10.0.0.0/8 | Large organizations |
| 172.16.0.0 - 172.31.255.255 | 172.16.0.0/12 | Medium networks |
| 192.168.0.0 - 192.168.255.255 | 192.168.0.0/16 | Home and small offices |

**Special addresses:**

- `127.0.0.1` is loopback (the machine itself).
- `169.254.0.0/16` is APIPA. A device that could not get a DHCP address assigns itself one of these.
- `255.255.255.255` is limited broadcast.
- `0.0.0.0` means "any" or "this host" depending on context.

**Classes (historical):** A = 1-126, B = 128-191, C = 192-223, D (multicast) = 224-239, E (reserved) = 240-255. Modern networks use CIDR, but the exam still asks.

### 2.5 Subnetting (the skill people fear, made simple)

A **subnet mask** says which part of the address is the network and which part is the host. CIDR notation `/24` means the first 24 bits are network bits.

| CIDR | Subnet mask | Total addresses | Usable hosts |
|---|---|---|---|
| /30 | 255.255.255.252 | 4 | 2 |
| /29 | 255.255.255.248 | 8 | 6 |
| /28 | 255.255.255.240 | 16 | 14 |
| /27 | 255.255.255.224 | 32 | 30 |
| /26 | 255.255.255.192 | 64 | 62 |
| /25 | 255.255.255.128 | 128 | 126 |
| /24 | 255.255.255.0 | 256 | 254 |
| /16 | 255.255.0.0 | 65,536 | 65,534 |
| /8 | 255.0.0.0 | 16,777,216 | 16,777,214 |

**Formula:** usable hosts = 2^(32 - prefix) - 2. You subtract 2 for the network address (all host bits 0) and the broadcast address (all host bits 1).

**Worked example:** `192.168.10.77/26`

1. /26 means the block size is 64 (256 - 192 = 64 in the last octet).
2. Subnets in the last octet start at 0, 64, 128, 192.
3. 77 falls in the 64 block, so:
   - Network address: `192.168.10.64`
   - First usable: `192.168.10.65`
   - Last usable: `192.168.10.126`
   - Broadcast: `192.168.10.127`

Practice 10 of these by hand. Use `ipcalc` on Linux to check yourself.

### 2.6 IPv6 basics

- 128 bits, written in hex: `2001:0db8:85a3:0000:0000:8a2e:0370:7334`.
- Shortening: drop leading zeros and replace one run of zeros with `::` (`2001:db8:85a3::8a2e:370:7334`).
- `::1` is loopback. `fe80::/10` is link-local. `fc00::/7` is unique local (like private IPv4).
- No broadcast. It uses multicast and anycast. No ARP; it uses **NDP** (Neighbor Discovery Protocol).

## Day 2: Networking concepts, part 2 (ports and protocols)

### 2.7 TCP vs UDP

| | TCP | UDP |
|---|---|---|
| Connection | Connection-oriented | Connectionless |
| Reliability | Acknowledgments, retransmission | None (fire and forget) |
| Ordering | Guaranteed | Not guaranteed |
| Speed | Slower | Faster |
| Used for | Web, email, file transfer, SSH | DNS queries, VoIP, video streaming, DHCP |

**TCP three-way handshake:**

```
Client                     Server
  | ---- SYN ------------>  |    "I want to connect, my sequence number is X"
  | <--- SYN + ACK -------  |    "OK, mine is Y, and I got X"
  | ---- ACK ------------>  |    "Got Y. Connection established"
```

Connection close uses FIN/ACK (graceful) or RST (abrupt reset).
**Security relevance:** a port scanner sending SYN packets and never finishing the handshake is a "SYN scan". A flood of SYNs with no ACKs is a **SYN flood** (DoS).

### 2.8 Ports you must know

Ports 0-1023 are well-known, 1024-49151 registered, 49152-65535 dynamic/ephemeral.

| Port | Protocol | Purpose | Secure? |
|---|---|---|---|
| 20/21 | FTP | File transfer (data/control) | No, cleartext |
| 22 | SSH / SFTP / SCP | Remote shell, secure file transfer | Yes |
| 23 | Telnet | Remote shell | No, cleartext |
| 25 | SMTP | Send email between servers | No (unless STARTTLS) |
| 53 | DNS | Name resolution (UDP and TCP) | Not by default |
| 67/68 | DHCP | Server/client address assignment (UDP) | n/a |
| 69 | TFTP | Trivial file transfer (UDP) | No |
| 80 | HTTP | Web | No |
| 110 | POP3 | Email retrieval | No |
| 123 | NTP | Time sync (UDP) | n/a |
| 143 | IMAP | Email retrieval | No |
| 161/162 | SNMP | Device management / traps (UDP) | v3 is secure |
| 389 | LDAP | Directory services | No |
| 443 | HTTPS | Web over TLS | Yes |
| 445 | SMB | Windows file sharing | Depends on version |
| 465/587 | SMTPS / Submission | Secure email sending | Yes |
| 514 | Syslog | Log forwarding (UDP) | No |
| 636 | LDAPS | LDAP over TLS | Yes |
| 993 | IMAPS | IMAP over TLS | Yes |
| 995 | POP3S | POP3 over TLS | Yes |
| 1433 | MS SQL | Database | Depends |
| 3306 | MySQL | Database | Depends |
| 3389 | RDP | Windows remote desktop | Depends, often attacked |
| 5432 | PostgreSQL | Database | Depends |

The pattern to learn: for each insecure protocol, there is a secure replacement (Telnet to SSH, HTTP to HTTPS, FTP to SFTP, LDAP to LDAPS).

### 2.9 Important protocols in plain language

- **DNS:** the phone book of the internet. Turns `example.com` into an IP. Record types: **A** (IPv4), **AAAA** (IPv6), **CNAME** (alias), **MX** (mail server), **NS** (name server), **TXT** (free text, used for SPF/DKIM), **PTR** (reverse lookup), **SOA** (zone authority).
- **DHCP:** automatically hands out IP, mask, gateway and DNS. Process is **DORA**: Discover, Offer, Request, Acknowledge.
- **ICMP:** control and error messages. `ping` uses echo request/reply. `traceroute` uses TTL expiry messages.
- **NTP:** keeps clocks in sync. Critical for logs, since wrong times make investigations impossible.
- **HTTP:** request/response. Methods: GET, POST, PUT, DELETE, HEAD, OPTIONS. Status codes: 2xx success, 3xx redirect, 4xx client error (401 unauthorized, 403 forbidden, 404 not found), 5xx server error.
- **SNMP:** monitors and manages network devices. v1/v2c use community strings (basically passwords in cleartext). Use v3.
- **SMB:** Windows file/printer sharing. Frequent target (for example EternalBlue exploited SMBv1).

### 2.10 Network address translation (NAT)

Lets many private-IP devices share one public IP.

- **Static NAT:** one-to-one mapping.
- **Dynamic NAT:** pool of public IPs.
- **PAT (Port Address Translation) / NAT overload:** many private IPs share one public IP, distinguished by port. This is what your home router does.

## Day 3: Networking concepts, part 3 (devices and topologies)

### 2.11 Network devices

| Device | Layer | Function |
|---|---|---|
| Hub | 1 | Repeats traffic to all ports (obsolete, insecure) |
| Switch | 2 (L3 for multilayer) | Forwards frames by MAC address using a CAM table |
| Router | 3 | Forwards packets between networks using routing tables |
| Firewall | 3-7 | Allows or blocks traffic based on rules |
| Load balancer | 4-7 | Spreads traffic across servers |
| Access point (AP) | 1-2 | Connects wireless clients to wired network |
| Proxy | 7 | Makes requests on behalf of clients, can filter and cache |
| IDS/IPS | 3-7 | Detects (IDS) or blocks (IPS) malicious traffic |
| Modem | 1 | Converts signals (cable, DSL, fiber) |

**How a switch learns:** it looks at the source MAC of every incoming frame and records which port it came from. Unknown destination? It floods the frame to all ports. **MAC flooding** attacks overflow the table to make the switch behave like a hub.

### 2.12 Topologies

- **Star:** all devices connect to a central switch. Most common.
- **Bus:** single shared cable. Obsolete.
- **Ring:** each device connects to two neighbors.
- **Mesh:** devices connect to many others. Full mesh is resilient and expensive. Partial mesh is common on WANs.
- **Hub-and-spoke:** central site plus branch offices.
- **Three-tier:** core, distribution, access layers in enterprise networks.
- **Spine-and-leaf:** modern data center design.

### 2.13 Cabling and physical basics

- **Copper (twisted pair):** Cat5e (1 Gbps), Cat6 (up to 10 Gbps on short runs), Cat6a. Max run 100 m.
- **Fiber:** single-mode (long distance, laser) and multimode (short distance, LED). Immune to electromagnetic interference and harder to tap.
- **Coax:** cable TV and some broadband.
- **PoE:** power over Ethernet (powers phones, cameras, APs).

## Day 4: Network implementation (routing, switching, wireless)

### 2.14 Routing

- **Default gateway:** the router your host sends non-local traffic to.
- **Static routes:** manually configured. Simple, no overhead, no automatic recovery.
- **Dynamic routing protocols:**
  - **RIP:** distance-vector, hop count, max 15. Old.
  - **OSPF:** link-state, uses cost, fast convergence. Common in enterprises (interior).
  - **EIGRP:** Cisco advanced distance-vector (interior).
  - **BGP:** path-vector, glues the internet together between autonomous systems (exterior). **BGP hijacking** is a real attack class.
- **Administrative distance:** trustworthiness of a route source. Lower wins (connected 0, static 1, OSPF 110, RIP 120).

### 2.15 Switching features

- **VLAN:** logically splits one physical switch into multiple broadcast domains. Improves security and performance. Devices in different VLANs need a router (inter-VLAN routing) to talk.
- **Trunk port:** carries multiple VLANs between switches using 802.1Q tags.
- **Access port:** belongs to one VLAN.
- **STP (Spanning Tree Protocol):** prevents switching loops by blocking redundant links.
- **Port security:** limits MAC addresses per port. Protects against MAC flooding.
- **Link aggregation (LACP):** bundles multiple links into one logical link.
- **VLAN hopping:** attack to reach other VLANs (double tagging, switch spoofing). Prevent by disabling unused ports and not using the native VLAN for user traffic.

### 2.16 Wireless

| Standard | Name | Band | Max speed (theoretical) |
|---|---|---|---|
| 802.11n | Wi-Fi 4 | 2.4 / 5 GHz | 600 Mbps |
| 802.11ac | Wi-Fi 5 | 5 GHz | ~3.5 Gbps |
| 802.11ax | Wi-Fi 6 / 6E | 2.4 / 5 / 6 GHz | ~9.6 Gbps |

- **2.4 GHz:** longer range, more interference, 3 non-overlapping channels (1, 6, 11).
- **5 GHz / 6 GHz:** shorter range, more channels, faster.
- **Security protocols:** WEP (broken) < WPA (weak) < WPA2 (AES-CCMP, solid but crackable with weak passwords) < **WPA3** (SAE handshake, best).
- **Personal vs Enterprise:** Personal uses a pre-shared key. Enterprise uses 802.1X with a RADIUS server (unique credentials per user).
- **Attacks:** evil twin (fake AP with the same SSID), deauthentication, rogue AP, WPS PIN brute force.

## Day 5: Network operations

### 2.17 Documentation and monitoring

- **Network diagrams** (physical and logical), **IP address management (IPAM)**, **baselines** (what normal looks like), **change management**.
- **Monitoring tools:** SNMP polling, NetFlow/IPFIX (who talked to whom, how much), syslog, packet captures.
- **High availability:** redundancy (dual power supplies, dual links), **FHRP** (HSRP/VRRP give a virtual gateway IP), clustering, load balancing.
- **Disaster recovery terms:**
  - **RTO** (Recovery Time Objective): how long can we be down?
  - **RPO** (Recovery Point Objective): how much data can we lose?
  - **Site types:** hot (ready now), warm (partly ready), cold (empty space).
- **QoS:** prioritizes traffic such as voice.

### 2.18 Remote access

- **VPN:** encrypted tunnel over an untrusted network. Types: site-to-site, client-to-site (remote access). Protocols: IPsec, OpenVPN, WireGuard, SSL/TLS VPN.
- **Split tunnel vs full tunnel:** split sends only corporate traffic through the VPN. Full sends everything.
- **Jump host / bastion:** hardened server you go through to reach internal systems.
- **SSH** over Telnet. **RDP** should never be exposed directly to the internet.

## Day 6: Network security

### 2.19 Core security devices and concepts

- **Firewall types:** packet filter (stateless), stateful, next-generation (NGFW; app awareness, IPS, TLS inspection), WAF (protects web apps).
- **ACLs:** ordered rules, first match wins, ends with an implicit deny.
- **DMZ / screened subnet:** buffer zone for public-facing servers.
- **IDS vs IPS:** IDS detects and alerts (passive, out of band). IPS blocks (inline).
- **Zero trust:** never trust, always verify. No implicit trust based on network location.
- **NAC (Network Access Control):** checks device health before allowing access (802.1X).
- **Segmentation:** VLANs, firewalls between zones, microsegmentation. Limits how far an attacker can move.

### 2.20 Common network attacks

| Attack | How it works | Defense |
|---|---|---|
| ARP spoofing | Fake ARP replies redirect traffic through attacker (on-path) | Dynamic ARP Inspection, static ARP for critical hosts |
| DNS poisoning | Fake DNS answers cached by resolver | DNSSEC, patching, randomized source ports |
| DoS / DDoS | Overwhelm a target with traffic | Rate limiting, CDN/scrubbing, upstream filtering |
| SYN flood | Half-open TCP connections exhaust resources | SYN cookies, firewalls |
| Smurf | ICMP broadcast with spoofed source | Disable directed broadcasts |
| MAC flooding | Fill the switch CAM table | Port security |
| VLAN hopping | Escape your VLAN | Disable DTP, unused ports off |
| Rogue DHCP | Fake DHCP server hands attacker gateway | DHCP snooping |
| On-path (MITM) | Attacker sits between two parties | Encryption (TLS), mutual auth |
| Evil twin | Fake Wi-Fi AP | WPA3/Enterprise, user awareness |
| Port scanning | Discover open services | Firewalls, minimize exposed ports |

## Day 7: Troubleshooting and review

### 2.21 Troubleshooting method

1. Identify the problem.
2. Establish a theory of probable cause.
3. Test the theory.
4. Establish a plan of action and identify effects.
5. Implement the solution (or escalate).
6. Verify full system functionality and implement preventive measures.
7. Document findings and actions.

### 2.22 Tools you should be able to use

| Tool | Command | What it tells you |
|---|---|---|
| ping | `ping 8.8.8.8` | Is the host reachable? Latency and loss |
| traceroute / tracert | `traceroute example.com` / `tracert example.com` | The path and where it breaks |
| nslookup / dig | `nslookup example.com` / `dig example.com A` | DNS answers |
| ipconfig / ip | `ipconfig /all` (Win), `ip addr` (Linux) | Local addressing |
| arp | `arp -a` | ARP cache |
| netstat / ss | `netstat -ano` (Win), `ss -tulpn` (Linux) | Open ports and connections |
| nmap | `nmap -sV 192.168.1.0/24` | Hosts and services (only on networks you own or have permission for) |
| curl | `curl -I https://example.com` | HTTP headers and responses |
| tcpdump / Wireshark | see Section 6 | Actual packets |

### 2.23 Symptom quick reference

| Symptom | Likely cause |
|---|---|
| Ping to IP works, ping to name fails | DNS problem |
| Address starts with 169.254 | DHCP failure |
| Can reach local hosts but not the internet | Wrong or missing default gateway |
| Intermittent slow network | Duplex mismatch, congestion, failing cable, loop |
| Only one device can't connect | Cable, port, VLAN, or IP conflict |
| Certificate warnings | Expired cert, wrong hostname, wrong time |

### 2.24 Day 1-7 self-test

Answer without looking:

1. Name the OSI layers and one protocol for each.
2. What are the 3 steps of the TCP handshake?
3. How many usable hosts in a /27?
4. Which ports do SSH, DNS, HTTPS, RDP and SMB use?
5. Why is ARP insecure?
6. What is the difference between IDS and IPS?
7. What does a VLAN do and why does it help security?
8. What are the DORA steps?

---

# 3. Days 8-14: Security+ (security concepts)

**Source in repo:** Professor Messer's SY0-701 playlist (alternative: Pete Zerger's SY0-701 playlist).
**Goal:** learn the language and building blocks of security.

SY0-701 domains and weightings:

| Domain | Topic | Weight |
|---|---|---|
| 1 | General security concepts | 12% |
| 2 | Threats, vulnerabilities and mitigations | 22% |
| 3 | Security architecture | 18% |
| 4 | Security operations | 28% |
| 5 | Security program management and oversight | 20% |

## Day 8: General security concepts

### 3.1 The CIA triad

| Principle | Meaning | Threat | Controls |
|---|---|---|---|
| **Confidentiality** | Only authorized people see data | Data leaks, eavesdropping | Encryption, access control |
| **Integrity** | Data is not altered without authorization | Tampering | Hashing, digital signatures, checksums |
| **Availability** | Systems work when needed | DoS, ransomware, hardware failure | Redundancy, backups, DDoS protection |

Related: **non-repudiation** (you can't deny you did something; digital signatures and logs).

### 3.2 AAA

- **Authentication:** prove who you are.
- **Authorization:** what you are allowed to do.
- **Accounting:** record what you did (logs).

Authentication factors: **something you know** (password), **have** (token, phone, smart card), **are** (biometrics), **somewhere you are** (location), **something you do** (typing pattern). **MFA** means two or more *different* categories. Password plus PIN is not MFA (both are "know").

### 3.3 Control categories and types

Categories: **Technical** (firewall), **Managerial** (risk assessment, policy), **Operational** (guards, training), **Physical** (locks, cameras).

Types by function:

| Type | Purpose | Example |
|---|---|---|
| Preventive | Stop it happening | Firewall, MFA |
| Deterrent | Discourage | Warning signs |
| Detective | Find it | IDS, log review, CCTV |
| Corrective | Fix it after | Backup restore, patching |
| Compensating | Alternative when main control isn't possible | Extra monitoring on an unpatchable system |
| Directive | Tell people what to do | Policies |

### 3.4 Core ideas

- **Zero trust:** verify every request. Uses a control plane (policy engine and administrator) and data plane (policy enforcement points).
- **Defense in depth:** layered controls so one failure doesn't mean compromise.
- **Least privilege:** minimal access needed for the job.
- **Separation of duties:** no one person controls a whole critical process.
- **Change management:** approval, testing, backout plan, documentation.
- **Gap analysis:** compare current state to desired state.

## Day 9: Cryptography

### 3.5 Concepts

- **Symmetric encryption:** same key encrypts and decrypts. Fast. Problem: sharing the key. **AES** (128/192/256) is the standard. 3DES is legacy, DES is broken.
- **Asymmetric encryption:** key pair (public and private). Public encrypts, private decrypts (or private signs, public verifies). Slower. **RSA**, **ECC** (smaller keys, same strength). Solves key exchange.
- **Hybrid:** TLS uses asymmetric crypto to agree a session key, then symmetric for bulk data.
- **Key exchange:** Diffie-Hellman (DH/ECDH). **Perfect forward secrecy** (ephemeral keys): compromising a long-term key doesn't reveal past sessions.

### 3.6 Hashing

One-way function that produces a fixed-size fingerprint. Same input gives same output. Tiny input change gives a completely different output.

| Algorithm | Status |
|---|---|
| MD5 | Broken (collisions). Only for non-security checksums |
| SHA-1 | Deprecated |
| SHA-256 / SHA-3 | Good |
| bcrypt / scrypt / Argon2 / PBKDF2 | Password hashing (slow on purpose) |

- **Salt:** random data added to each password before hashing. Defeats rainbow tables and prevents identical passwords from producing identical hashes.
- **Pepper:** secret value stored separately from the database.
- **Integrity check:** `sha256sum file.iso` and compare to the published hash.

### 3.7 Digital signatures and certificates

- **Digital signature:** hash the message, encrypt the hash with your **private** key. Anyone can verify with your public key. Gives integrity, authentication and non-repudiation.
- **PKI:** system of **Certificate Authorities (CA)**, certificates (X.509), and revocation.
  - **Root CA** signs **intermediate CAs**, which sign end certificates (the *chain of trust*).
  - **CSR:** request you send to a CA to get a certificate.
  - **Revocation:** CRL (list) or OCSP (live check). OCSP stapling has the server include the response.
  - **Wildcard cert:** `*.example.com`. **SAN:** lists multiple names.
  - **Self-signed:** no third-party trust, fine for labs, warning in browsers.
- **TLS handshake (simplified):** client hello, server sends certificate, client verifies chain and name, key exchange, both derive session keys, encrypted traffic begins.

### 3.8 Other crypto terms

- **Steganography:** hiding data inside other files (an image).
- **Obfuscation vs encryption:** hiding is not securing.
- **Data states:** at rest (disk encryption, BitLocker/LUKS), in transit (TLS, IPsec), in use (secure enclaves, memory).
- **HSM / TPM:** hardware that stores keys securely.
- **Tokenization / masking:** replace sensitive data (card numbers) with substitutes.
- **Blockchain:** distributed ledger with hash-linked blocks.

## Day 10: Threats, actors and attack types

### 3.9 Threat actors

| Actor | Motivation | Sophistication |
|---|---|---|
| Nation-state | Espionage, sabotage | Very high, well-funded (APT) |
| Organized crime | Money | High |
| Hacktivist | Ideology | Low to medium |
| Insider | Revenge, money, or accident | Varies, has legitimate access |
| Script kiddie | Fun, reputation | Low, uses others' tools |
| Shadow IT | Convenience | n/a, unmanaged systems |

### 3.10 Malware

| Type | Behavior |
|---|---|
| Virus | Attaches to files, needs user action to spread |
| Worm | Self-replicates across networks |
| Trojan | Looks legitimate, hides malicious function |
| Ransomware | Encrypts data and demands payment |
| Spyware / keylogger | Steals information/keystrokes |
| Rootkit | Hides deep in OS, persistent |
| Backdoor | Secret access |
| Bot / botnet | Compromised machines under remote control |
| Logic bomb | Triggers on a condition or date |
| Fileless malware | Runs in memory using legitimate tools (PowerShell) |
| RAT | Remote access trojan |

### 3.11 Social engineering

- **Phishing** (email), **spear phishing** (targeted), **whaling** (executives), **smishing** (SMS), **vishing** (voice), **pharming** (redirect to fake site).
- **Pretexting:** invented scenario. **Impersonation, tailgating/piggybacking** (following someone through a door), **shoulder surfing**, **dumpster diving**.
- **Business email compromise (BEC):** fake executive or vendor email to redirect payments.
- **Watering hole:** compromise a site the target visits.
- **Typosquatting:** lookalike domains (`examp1e.com`).
- **Principles used:** authority, urgency, scarcity, familiarity, consensus, intimidation.
- **Defense:** training, reporting culture, email filtering (SPF, DKIM, DMARC), MFA.

### 3.12 Application and network attacks

| Attack | What it is |
|---|---|
| SQL injection | Malicious SQL in an input alters a database query |
| XSS (cross-site scripting) | Injected script runs in other users' browsers |
| CSRF | Tricks a logged-in user's browser into making an unwanted request |
| Buffer overflow | Writing beyond memory bounds to crash or hijack execution |
| Directory traversal | `../../etc/passwd` to read files outside the web root |
| Command injection | Input reaches an OS shell |
| SSRF | Server tricked into requesting internal resources |
| Race condition / TOCTOU | Exploit timing gaps |
| Replay | Resend captured valid traffic |
| Downgrade | Force weaker protocol versions |
| Credential attacks | Brute force, dictionary, **password spraying** (one password, many accounts), **credential stuffing** (leaked pairs reused on other sites), rainbow tables |
| Privilege escalation | Gain higher permissions (vertical) or another user's (horizontal) |
| Zero-day | Unknown to the vendor, no patch yet |
| Supply chain | Attack via a vendor, library or update mechanism |

### 3.13 Indicators of compromise (IOCs)

Unusual outbound traffic, logins at odd hours or from impossible locations, new admin accounts, disabled security tools, changes to system files, unexpected spikes in DB reads, many failed logins followed by a success, strange DNS requests.

## Day 11: Vulnerabilities and mitigation

### 3.14 Vulnerability sources

- Unpatched software, default credentials, misconfiguration, weak/outdated crypto, open ports, legacy and end-of-life systems, insecure APIs, third-party code, hardware (firmware) flaws, virtualization escape, mobile jailbreak/sideloading.

### 3.15 Vulnerability management

- **CVE:** unique ID for a vulnerability (`CVE-2021-44228`).
- **CVSS:** severity score 0.0-10.0 (Low 0.1-3.9, Medium 4-6.9, High 7-8.9, Critical 9-10).
- **Scanners:** Nessus, OpenVAS, Qualys. **Credentialed** scans see more than **non-credentialed**.
- **False positive:** scanner says vulnerable, but it isn't. **False negative:** misses a real issue (worse).
- **Penetration test vs vuln scan:** scan finds and lists. Pentest actively exploits to prove impact.
- **Pentest types:** black box (no knowledge), white box (full knowledge), gray box (partial). **Rules of engagement** and written authorization come first.
- **Bug bounty:** rewards for responsibly reported flaws.
- **Threat intelligence:** OSINT, ISACs, dark web monitoring, MITRE ATT&CK (map of attacker tactics and techniques).

### 3.16 Hardening

Remove unused software and services, close ports, change defaults, patch, enforce strong auth, encrypt disks, enable host firewall and logging, use secure baselines (CIS Benchmarks), apply configuration management.

## Day 12: Security architecture

### 3.17 Secure infrastructure

- **Cloud models:** IaaS, PaaS, SaaS. **Shared responsibility.** Public, private, hybrid, community.
- **Virtualization and containers:** hypervisors (Type 1 bare metal, Type 2 hosted), VM escape, container isolation (Docker, Kubernetes), image scanning.
- **Infrastructure as code (IaC)**, **serverless**, **microservices**, **SDN**.
- **Network design:** segmentation, DMZ, air gap, **jump server**, **proxy**, **reverse proxy**, **load balancer**, **sensors and taps**, **port mirroring (SPAN)**.
- **Fail-open vs fail-closed:** on failure, does the device allow all or deny all?
- **Active/passive** and **active/active** redundancy.
- **Embedded/ICS/SCADA/IoT:** hard to patch, often unauthenticated, keep them isolated.

### 3.18 Data protection

- **Classification:** public, internal, confidential, restricted/top secret. **Owner, custodian, controller, processor, steward.**
- **DLP (data loss prevention):** detects and blocks sensitive data leaving.
- **Data sovereignty:** law depends on where data is stored.
- **Backups:** full, incremental (changes since last backup), differential (changes since last full). **3-2-1 rule:** 3 copies, 2 media types, 1 offsite. **Test restores.** Immutable backups defend against ransomware.
- **RAID:** 0 (stripe, no redundancy), 1 (mirror), 5 (stripe + parity), 6 (double parity), 10 (mirror + stripe).

### 3.19 Resilience

Load balancing, clustering, geographic dispersion, UPS and generators, snapshots, and **capacity planning**. Use tabletop exercises to practice failover.

## Day 13: Security operations

### 3.20 Identity and access management (IAM)

- **Models:** DAC (owner decides), MAC (labels/clearances), RBAC (roles), ABAC (attributes), rule-based.
- **SSO:** log in once. **Federation:** trust across organizations. Protocols: **SAML** (XML, enterprise SSO), **OAuth 2.0** (authorization, delegating access), **OpenID Connect** (authentication on top of OAuth), **Kerberos** (tickets, Windows domains), **LDAP**, **RADIUS**, **TACACS+**.
- **PAM:** privileged access management. Vault admin passwords, just-in-time access, session recording.
- **Password best practice (current guidance):** long passphrases, check against breached lists, use a password manager, no forced periodic rotation without cause, MFA everywhere. Prefer **passkeys / FIDO2** for phishing resistance.
- **Account lifecycle:** provisioning, review, deprovisioning (offboarding fast).

### 3.21 Monitoring and logging

- **SIEM:** collects logs from everywhere, correlates them, alerts. (ELK/Splunk/Sentinel/Wazuh.)
- **SOAR:** automates response playbooks.
- **EDR/XDR:** endpoint (and cross-domain) detection and response.
- **NetFlow, syslog, packet capture,** **vulnerability scan results,** **DNS logs**, **authentication logs**.
- **Alert tuning:** reduce false positives without missing true ones.
- **Log management:** central storage, integrity protection, retention policy, synchronized time.

### 3.22 Incident response

Phases (NIST):

1. **Preparation:** plan, tools, training, contacts.
2. **Detection and analysis:** confirm, scope and prioritize.
3. **Containment, eradication, recovery:**
   - Contain (isolate the host, disable accounts).
   - Eradicate (remove malware, close the hole).
   - Recover (restore, monitor).
4. **Post-incident activity:** lessons learned, update controls.

Classic six-step version: **Preparation, Identification, Containment, Eradication, Recovery, Lessons learned.**

**Digital forensics essentials:**

- **Order of volatility:** CPU registers/cache, RAM, network state, running processes, disk, remote logs, backups. Capture the most volatile first.
- **Chain of custody:** documented handling of evidence.
- **Hashing** the image to prove it wasn't changed. Work on copies. **Legal hold** stops deletion.

### 3.23 Automation and secure coding

- **Secure development lifecycle:** requirements, design (threat modeling), code (SAST), test (DAST, fuzzing), deploy, maintain.
- **Input validation, parameterized queries, output encoding, least-privilege service accounts, secrets in a vault (not in code), code signing, dependency scanning.**
- **CI/CD security,** infrastructure-as-code scanning.

## Day 14: Governance, risk and compliance

### 3.24 Risk management

- **Risk = Likelihood x Impact.**
- **Assessment:** qualitative (high/medium/low) or quantitative:
  - **SLE** (single loss expectancy) = asset value x exposure factor.
  - **ARO** (annualized rate of occurrence).
  - **ALE** = SLE x ARO.
- **Risk responses:** **Mitigate**, **Transfer** (insurance, outsourcing), **Accept**, **Avoid**.
- **Residual risk:** what remains after controls. **Risk appetite/tolerance:** how much the organization will accept.
- **Risk register:** tracked list of risks, owners and treatments.
- **BIA (business impact analysis)** identifies critical functions: **MTD, RTO, RPO, MTBF, MTTR**.

### 3.25 Governance and compliance

- **Policies, standards, procedures, guidelines** (in that order of authority). Acceptable use, password, incident response, BYOD, data retention.
- **Frameworks:** NIST CSF and 800-53, ISO/IEC 27001, CIS Controls, COBIT.
- **Regulations:** GDPR (EU privacy), HIPAA (US health), PCI DSS (payment cards), SOX (financial reporting), CCPA/CPRA (California privacy), NIS2 (EU).
- **Privacy:** data minimization, consent, right to erasure, breach notification deadlines (GDPR: 72 hours to the authority).
- **Third-party risk:** vendor assessment, SLA, MOU, NDA, right-to-audit.
- **Audits:** internal and external, attestation. **SOC 2** reports.
- **Awareness training:** phishing simulation, role-based training.

### 3.26 Day 8-14 self-test

1. Give an example of each CIA element being broken.
2. Symmetric vs asymmetric: difference and one use of each.
3. Why do we salt passwords?
4. What separates a virus from a worm?
5. Password spraying vs credential stuffing?
6. Steps of incident response in order?
7. Formula for ALE?
8. What is the difference between a vulnerability scan and a penetration test?
9. What does least privilege mean, with an example?
10. Name three ways to respond to risk.

---

# 4. Days 15-28: Linux

**Sources in repo:** Linux Journey, Cisco NetAcad "Linux Unhatched", LabEx Linux labs.
**Goal:** be comfortable in a terminal. Most servers, security tools (Kali, Parrot) and cloud instances run Linux.

**Setup for hands-on:** use a VM (VirtualBox/VMware) with Ubuntu or Kali, WSL2 on Windows, or a free browser lab such as LabEx. Do not practice destructive commands on your main machine.

## Days 15-16: Getting started

### 4.1 What Linux is

Linux is the **kernel**. A **distribution** (Ubuntu, Debian, Fedora, Arch, Kali) bundles the kernel with tools and a package manager. You talk to the kernel through a **shell** (bash, zsh) in a **terminal**.

Prompt anatomy: `user@hostname:~$`. `$` = normal user, `#` = root.

### 4.2 Filesystem layout

Everything starts at `/` (root). Everything is a file.

| Path | Contents |
|---|---|
| `/` | Root of the whole tree |
| `/home` | User home directories (`~`) |
| `/root` | The root user's home |
| `/etc` | Configuration files |
| `/var` | Variable data: logs (`/var/log`), web roots, mail |
| `/tmp` | Temporary files (world-writable, wiped at reboot) |
| `/bin`, `/sbin`, `/usr/bin` | Programs |
| `/lib` | Shared libraries |
| `/dev` | Device files (`/dev/sda`) |
| `/proc` | Live kernel/process information |
| `/sys` | Kernel and hardware interface |
| `/boot` | Bootloader and kernel |
| `/opt` | Optional/third-party software |
| `/mnt`, `/media` | Mount points |

### 4.3 Navigation and file commands

```bash
pwd                     # where am I?
ls                      # list files
ls -la                  # long format, include hidden (dotfiles)
cd /var/log             # change directory
cd ~                    # home
cd ..                   # up one level
cd -                    # previous directory

mkdir project           # make directory
mkdir -p a/b/c          # make nested directories
touch notes.txt         # create empty file / update timestamp
cp file.txt backup.txt  # copy
cp -r dir1 dir2         # copy a directory
mv old.txt new.txt      # move or rename
rm file.txt             # delete (no recycle bin!)
rm -r dir               # delete a directory recursively
rmdir emptydir          # delete an empty directory
```

Warning: `rm -rf /` style commands destroy systems. Always read the path twice.

### 4.4 Reading files

```bash
cat file.txt            # print whole file
less file.txt           # scroll (q to quit, /word to search)
head -n 20 file.txt     # first 20 lines
tail -n 20 file.txt     # last 20 lines
tail -f /var/log/syslog # follow a log live (great for monitoring)
wc -l file.txt          # count lines
file mystery.bin        # what type of file is this?
stat file.txt           # detailed metadata
```

### 4.5 Getting help

```bash
man ls                  # manual page (q to quit)
ls --help               # short help
apropos "copy files"    # search manuals
type cd                 # is it built in or a program?
```

## Days 17-18: Users, permissions and ownership

### 4.6 Users and groups

```bash
whoami                  # current user
id                      # UID, GID, groups
who                     # who is logged in
last                    # login history
sudo command            # run one command as root
su - username           # switch user
sudo useradd -m alice   # create user with a home directory
sudo passwd alice       # set password
sudo usermod -aG sudo alice   # add to the sudo group
sudo userdel -r alice   # delete user and home
```

Key files:

| File | Contents |
|---|---|
| `/etc/passwd` | User accounts (name, UID, GID, home, shell). Readable by all |
| `/etc/shadow` | Password hashes. Root-only |
| `/etc/group` | Groups |
| `/etc/sudoers` | Who may use sudo (edit with `visudo`) |

UID 0 is root. System accounts usually have UIDs below 1000.

### 4.7 File permissions

`ls -l` shows: `-rwxr-xr-- 1 alice staff 1024 Jan 1 12:00 script.sh`

```
- rwx r-x r--
| |   |   |
| |   |   +-- others (everyone else)
| |   +------ group
| +---------- owner (user)
+------------ type: - file, d directory, l symlink
```

| Permission | On a file | On a directory |
|---|---|---|
| r (4) | Read contents | List contents |
| w (2) | Modify | Create/delete files inside |
| x (1) | Execute | Enter (`cd`) |

Numeric: rwx = 7, rw- = 6, r-x = 5, r-- = 4.

```bash
chmod 755 script.sh        # owner rwx, group r-x, others r-x
chmod 600 id_rsa           # only owner can read/write (SSH keys must be like this)
chmod u+x script.sh        # add execute for owner
chmod go-w file            # remove write from group and others
chown alice file           # change owner
chown alice:staff file     # change owner and group
chgrp staff file           # change group
```

**Special permissions:**

- **SUID (4xxx):** file runs with the owner's privileges. `passwd` uses it. Attackers look for misused SUID binaries: `find / -perm -4000 -type f 2>/dev/null`.
- **SGID (2xxx):** run as group, or new files in a directory inherit its group.
- **Sticky bit (1xxx):** in a shared directory like `/tmp`, only the owner can delete their files.
- **umask:** default permission mask for new files (common: `022`).

## Days 19-20: Finding, filtering and text tools

### 4.8 Searching

```bash
find / -name "*.conf" 2>/dev/null          # by name
find /home -type f -mtime -1               # files modified in the last day
find / -user root -perm -4000 2>/dev/null  # SUID files
find . -size +100M                         # bigger than 100 MB
locate filename                            # fast, uses a database (updatedb)
which python3                              # where is this command
grep "error" logfile                       # lines containing 'error'
grep -i "error" logfile                    # case-insensitive
grep -r "password" /etc 2>/dev/null        # recursive
grep -v "debug" logfile                    # lines NOT matching
grep -c "Failed" /var/log/auth.log         # count matches
grep -E "fail(ed|ure)" logfile             # extended regex
```

### 4.9 Pipes and redirection

- `|` sends output of one command into another.
- `>` writes (overwrites) to a file. `>>` appends. `<` reads input from file.
- `2>` redirects errors. `2>/dev/null` discards errors. `&>` both.

```bash
ls -la /etc | grep conf
cat /var/log/auth.log | grep "Failed password" | wc -l
echo "hello" > out.txt
echo "more" >> out.txt
```

### 4.10 The text-processing toolkit (very useful for logs)

```bash
sort file.txt              # sort lines
sort -n file.txt           # numeric sort
uniq                       # remove adjacent duplicates (sort first)
uniq -c                    # count occurrences
cut -d',' -f1,3 data.csv   # columns 1 and 3, comma-delimited
awk '{print $1}' access.log        # print first field
awk -F: '{print $1,$3}' /etc/passwd  # custom delimiter
sed 's/old/new/g' file     # substitute
tr 'a-z' 'A-Z'             # translate characters
xargs                      # turn input into arguments
tee file.txt               # write to file AND stdout
```

**Classic log one-liner: top IPs hitting a web server**

```bash
awk '{print $1}' access.log | sort | uniq -c | sort -rn | head
```

**Failed SSH logins by source IP**

```bash
grep "Failed password" /var/log/auth.log | awk '{print $(NF-3)}' | sort | uniq -c | sort -rn | head
```

(The exact field number can differ by log format, so check with `head` first.)

### 4.11 Editors

- **nano:** beginner friendly (`Ctrl+O` save, `Ctrl+X` exit).
- **vim:** press `i` to insert, `Esc` to leave insert mode, `:wq` save and quit, `:q!` quit without saving, `/word` search, `dd` delete line, `yy` copy line, `p` paste.

## Days 21-22: Processes, services and packages

### 4.12 Processes

```bash
ps aux                  # all processes
ps -ef | grep ssh       # find a process
top                     # live view (q to quit); htop is nicer
kill 1234               # ask process 1234 to end (SIGTERM)
kill -9 1234            # force kill (SIGKILL)
pkill firefox           # kill by name
jobs; bg; fg            # background/foreground jobs
command &               # run in the background
nohup command &         # survive logout
```

Process states: running, sleeping, stopped, zombie. Each has a **PID** and a parent (**PPID**). PID 1 is `init`/`systemd`.

### 4.13 Services with systemd

```bash
systemctl status ssh
sudo systemctl start ssh
sudo systemctl stop ssh
sudo systemctl restart ssh
sudo systemctl enable ssh      # start at boot
sudo systemctl disable ssh
systemctl list-units --type=service
journalctl -u ssh              # logs for a service
journalctl -xe                 # recent errors
```

Scheduled jobs: `crontab -e` (format: `minute hour day month weekday command`). Attackers abuse cron for persistence, so check `/etc/cron*` and user crontabs when investigating.

### 4.14 Package management

| Family | Tool | Commands |
|---|---|---|
| Debian/Ubuntu/Kali | apt | `sudo apt update`, `sudo apt install nmap`, `sudo apt remove nmap`, `sudo apt upgrade` |
| RHEL/Fedora | dnf/yum | `sudo dnf install nmap` |
| Arch | pacman | `sudo pacman -S nmap` |

Keeping packages updated is one of the highest-value security habits.

### 4.15 Archives and compression

```bash
tar -czvf backup.tar.gz folder/     # create gzip archive
tar -xzvf backup.tar.gz             # extract
zip -r out.zip folder/ ; unzip out.zip
gzip file ; gunzip file.gz
```

## Days 23-25: Networking on Linux and remote access

### 4.16 Network commands

```bash
ip addr                 # IP addresses (older: ifconfig)
ip route                # routing table, default gateway
ip neigh                # ARP table
ping -c 4 8.8.8.8
ss -tulpn               # listening ports and owning processes (older: netstat -tulpn)
dig example.com         # DNS lookup
host example.com
curl -I https://example.com     # HTTP headers
wget https://example.com/file   # download
traceroute example.com
nmap -sn 192.168.1.0/24         # ping sweep (your own network only)
```

Config: `/etc/hosts` (local name overrides), `/etc/resolv.conf` (DNS servers), `/etc/hostname`.

### 4.17 SSH

```bash
ssh user@host                  # connect
ssh -p 2222 user@host          # non-default port
ssh-keygen -t ed25519          # create a key pair
ssh-copy-id user@host          # install your public key on the server
scp file.txt user@host:/tmp/   # secure copy
rsync -avz dir/ user@host:dir/ # efficient sync
ssh -L 8080:localhost:80 user@host   # local port forward (tunnel)
```

Hardening `/etc/ssh/sshd_config`: `PermitRootLogin no`, `PasswordAuthentication no` (once keys work), `AllowUsers alice`, change the port only as minor noise reduction, and use fail2ban. Restart with `sudo systemctl restart ssh`.

### 4.18 Firewalls

```bash
sudo ufw status
sudo ufw default deny incoming
sudo ufw allow 22/tcp
sudo ufw allow from 192.168.1.0/24 to any port 80
sudo ufw enable
```

Under the hood: **iptables** or **nftables**. Concept is the same: chains of rules, first match wins.

## Days 26-28: Shell scripting, logs and hardening

### 4.19 Bash scripting

```bash
#!/bin/bash
# save as check.sh, then: chmod +x check.sh && ./check.sh

name="world"
echo "Hello, $name"

# arguments: $1 $2 ... ; $# count ; $@ all
if [ -z "$1" ]; then
    echo "Usage: $0 <file>"
    exit 1
fi

# conditionals
if [ -f "$1" ]; then
    echo "$1 exists"
elif [ -d "$1" ]; then
    echo "$1 is a directory"
else
    echo "$1 not found"
fi

# loops
for i in 1 2 3; do
    echo "Number $i"
done

for host in 192.168.1.{1..5}; do
    ping -c 1 -W 1 "$host" &> /dev/null && echo "$host is up"
done

while read -r line; do
    echo "Line: $line"
done < "$1"

# functions
check_port() {
    ss -tuln | grep -q ":$1 " && echo "Port $1 open" || echo "Port $1 closed"
}
check_port 22
```

Useful test operators: `-f` file exists, `-d` directory, `-z` string empty, `-eq -ne -gt -lt` numbers, `==` strings. `$?` is the last exit code (0 = success). Always quote variables.

### 4.20 Logs to know

| File | What it holds |
|---|---|
| `/var/log/auth.log` (Debian) or `/var/log/secure` (RHEL) | Logins, sudo, SSH |
| `/var/log/syslog` or `/var/log/messages` | General system messages |
| `/var/log/kern.log` | Kernel |
| `/var/log/apache2/` or `/var/log/nginx/` | Web server access and error logs |
| `/var/log/dpkg.log` | Package installs |
| `journalctl` | systemd journal |

Look for: repeated `Failed password`, `sudo:` entries from unexpected users, new users, service restarts at odd times.

### 4.21 Basic Linux hardening checklist

- [ ] Keep the system updated (`apt update && apt upgrade`).
- [ ] Remove unneeded packages and disable unneeded services.
- [ ] Use SSH keys, disable root SSH login.
- [ ] Enable a firewall (ufw) with default deny.
- [ ] Use sudo instead of logging in as root.
- [ ] Set strong permissions on sensitive files.
- [ ] Enable and centralize logging. Install fail2ban.
- [ ] Encrypt disks (LUKS).
- [ ] Use mandatory access control (AppArmor or SELinux).
- [ ] Audit SUID files and cron jobs regularly.

### 4.22 Linux cheat sheet

| I want to... | Command |
|---|---|
| See disk usage | `df -h`, `du -sh *` |
| See memory | `free -h` |
| Check CPU/system info | `uname -a`, `lscpu`, `cat /etc/os-release` |
| Find a file by name | `find / -name "x" 2>/dev/null` |
| See the last 50 log lines live | `tail -n 50 -f /var/log/syslog` |
| Create a compressed backup | `tar -czvf b.tar.gz dir/` |
| Show open ports | `ss -tulpn` |
| Show who's logged in | `who`, `last` |
| Hash a file | `sha256sum file` |
| Run as root | `sudo <cmd>` |
| See command history | `history` |
| Repeat last command as sudo | `sudo !!` |

### 4.23 Linux self-test

1. What does `chmod 640 file` allow, and to whom?
2. Difference between `>` and `>>`?
3. How would you see which process is listening on port 80?
4. What makes SUID files a privilege-escalation target?
5. Write a one-liner to count failed SSH logins.
6. Where do you look for authentication logs?
7. How do you make a service start at boot?
8. How do you copy a file to another machine securely?

---

# 5. Days 29-42: Python for security

**Sources in repo:** freeCodeCamp Learn Python, Codecademy Learn Python, Python.org, Real Python, Talk Python, Learn Python the Hard Way, HackerRank Python, LabEx Python, TheCyberMentor's Python course.
**Goal:** read and write scripts that automate boring tasks: parsing logs, hashing files, calling APIs, scanning your own lab.

Setup: install Python 3 (`python3 --version`), use VS Code with the **Python** extension, and create a virtual environment per project:

```bash
python3 -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install requests
pip freeze > requirements.txt
```

## Days 29-31: Basics

### 5.1 Variables and types

```python
name = "Alice"          # str
age = 30                # int
pi = 3.14               # float
active = True           # bool
nothing = None          # NoneType

print(type(age))
print(f"{name} is {age} years old")   # f-string: the modern way to format
```

Python is dynamically typed. Names are case-sensitive. Use `snake_case` for variables and functions.

### 5.2 Operators

```python
10 + 3    # 13
10 - 3    # 7
10 * 3    # 30
10 / 3    # 3.333 (always float)
10 // 3   # 3     (floor division)
10 % 3    # 1     (remainder)
2 ** 8    # 256   (power)

5 == 5, 5 != 4, 5 > 3, 5 <= 5      # comparisons
True and False, True or False, not True
"a" in "cat"                         # membership
```

### 5.3 Strings

```python
s = "Hello, World"
s.lower(); s.upper(); s.strip()
s.split(",")              # ['Hello', ' World']
"-".join(["a", "b"])      # 'a-b'
s.replace("World", "Python")
s.startswith("Hello"); s.find("World")
s[0]; s[-1]; s[0:5]       # indexing and slicing
len(s)
```

Strings are immutable. Raw strings for regex and Windows paths: `r"C:\Users\file"`.

### 5.4 Input and output

```python
user = input("Name: ")      # always returns a string
n = int(input("Number: "))
print("a", "b", sep="-", end="!\n")
```

## Days 32-34: Data structures and control flow

### 5.5 Collections

| Type | Example | Notes |
|---|---|---|
| list | `[1, 2, 3]` | Ordered, mutable, allows duplicates |
| tuple | `(1, 2, 3)` | Ordered, immutable |
| dict | `{"ip": "10.0.0.1", "port": 22}` | Key-value pairs |
| set | `{1, 2, 3}` | Unordered, unique items, fast membership and set math |

```python
ports = [22, 80, 443]
ports.append(8080)
ports.remove(80)
ports[0]; ports[-1]; ports[1:3]
len(ports); sorted(ports)

host = {"ip": "10.0.0.1", "open": [22, 443]}
host["os"] = "linux"
host.get("missing", "default")      # no KeyError
for key, value in host.items():
    print(key, value)

a = {1, 2, 3}; b = {3, 4}
a & b      # intersection {3}
a | b      # union
a - b      # difference {1, 2}
```

### 5.6 Control flow

```python
if port == 22:
    print("SSH")
elif port in (80, 443):
    print("Web")
else:
    print("Other")

for p in [22, 80, 443]:
    print(p)

for i in range(1, 6):        # 1..5
    print(i)

for index, value in enumerate(["a", "b"]):
    print(index, value)

count = 0
while count < 3:
    count += 1
    if count == 2:
        continue             # skip to next iteration
    if count == 5:
        break                # leave the loop

# list comprehension
squares = [x * x for x in range(10) if x % 2 == 0]
open_ports = {p: "open" for p in [22, 80]}
```

## Days 35-37: Functions, files, errors, modules

### 5.7 Functions

```python
def is_private(ip: str) -> bool:
    """Return True if the IPv4 address is in a private range."""
    return ip.startswith(("10.", "192.168.")) or ip.startswith("172.16.")

def scan(host, ports=(22, 80), verbose=False):    # default args
    ...

def total(*args, **kwargs):     # variable arguments
    return sum(args)

square = lambda x: x * x         # anonymous function
```

Scope: variables inside a function are local. Return values, don't rely on globals.

### 5.8 Files

```python
with open("log.txt", "r") as f:        # 'with' closes the file automatically
    for line in f:
        print(line.strip())

with open("out.txt", "w") as f:        # 'w' overwrites, 'a' appends
    f.write("hello\n")

import json
with open("data.json") as f:
    data = json.load(f)
with open("out.json", "w") as f:
    json.dump(data, f, indent=2)

import csv
with open("hosts.csv", newline="") as f:
    for row in csv.DictReader(f):
        print(row["ip"])
```

### 5.9 Exceptions

```python
try:
    value = int(input("Number: "))
    result = 10 / value
except ValueError:
    print("Not a number")
except ZeroDivisionError:
    print("Cannot divide by zero")
except Exception as e:
    print(f"Unexpected: {e}")
else:
    print(result)             # runs if no exception
finally:
    print("done")             # always runs

raise ValueError("bad input")
```

Catch specific exceptions. A bare `except:` hides bugs.

### 5.10 Modules and the standard library

```python
import os, sys, re, hashlib, socket, subprocess, argparse, datetime, pathlib, collections, ipaddress
from pathlib import Path
from collections import Counter
```

Third-party packages (via `pip`): `requests` (HTTP), `scapy` (packets), `paramiko` (SSH), `python-nmap`, `beautifulsoup4` (HTML parsing), `pandas` (data), `cryptography`.

## Days 38-39: OOP and better habits

### 5.11 Classes

```python
class Host:
    def __init__(self, ip, os="unknown"):
        self.ip = ip
        self.os = os
        self.open_ports = []

    def add_port(self, port):
        self.open_ports.append(port)

    def __str__(self):
        return f"{self.ip} ({self.os}) ports={self.open_ports}"

h = Host("10.0.0.5", "linux")
h.add_port(22)
print(h)
```

Concepts: class vs object, attributes, methods, `self`, inheritance (`class Server(Host):`), encapsulation. You don't need deep OOP for scripts, but you'll read it in tools.

### 5.12 Good practices

- Use `if __name__ == "__main__":` to make a file both runnable and importable.
- Read command-line arguments with `argparse`.
- Never hard-code secrets. Read them from environment variables (`os.environ["API_KEY"]`).
- Format with `black`, lint with `ruff` or `flake8`, and write small functions.
- **Security warning:** don't use `eval()` or `exec()` on input, don't use `shell=True` with untrusted strings in `subprocess`, and don't use `pickle` on untrusted data.

```python
import argparse

def main():
    parser = argparse.ArgumentParser(description="Hash a file")
    parser.add_argument("path")
    parser.add_argument("--algo", default="sha256")
    args = parser.parse_args()
    print(args.path, args.algo)

if __name__ == "__main__":
    main()
```

## Days 40-42: Security-flavored mini projects

Only run network tools against machines you own or have written permission to test.

### 5.13 Project 1: file hasher (integrity checking)

```python
import hashlib, sys

def sha256_file(path, chunk=65536):
    h = hashlib.sha256()
    with open(path, "rb") as f:
        while block := f.read(chunk):
            h.update(block)
    return h.hexdigest()

if __name__ == "__main__":
    print(sha256_file(sys.argv[1]))
```

### 5.14 Project 2: log parser (failed SSH logins)

```python
import re, sys
from collections import Counter

pattern = re.compile(r"Failed password for (?:invalid user )?(\S+) from (\d+\.\d+\.\d+\.\d+)")
ips = Counter()
users = Counter()

with open(sys.argv[1]) as f:
    for line in f:
        m = pattern.search(line)
        if m:
            users[m.group(1)] += 1
            ips[m.group(2)] += 1

print("Top attacking IPs:")
for ip, n in ips.most_common(10):
    print(f"  {ip:<16} {n}")
print("Top targeted users:")
for user, n in users.most_common(10):
    print(f"  {user:<16} {n}")
```

### 5.15 Project 3: simple TCP port checker (for your own lab)

```python
import socket, sys

def check(host, port, timeout=1.0):
    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
        s.settimeout(timeout)
        return s.connect_ex((host, port)) == 0     # 0 means connected

host = sys.argv[1]
for port in (21, 22, 23, 25, 53, 80, 110, 139, 443, 445, 3306, 3389):
    if check(host, port):
        print(f"{host}:{port} open")
```

This is what tools like Nmap do at their simplest: try to connect and see whether it works. Nmap adds speed, stealth options, service detection and much more.

### 5.16 Project 4: HTTP status checker with `requests`

```python
import requests

urls = ["https://example.com", "https://example.org"]
for url in urls:
    try:
        r = requests.get(url, timeout=5)
        print(url, r.status_code, r.headers.get("Server"))
    except requests.RequestException as e:
        print(url, "error:", e)
```

Extension idea: check for missing security headers (`Strict-Transport-Security`, `Content-Security-Policy`, `X-Content-Type-Options`, `X-Frame-Options`).

### 5.17 Project 5: password strength and breach awareness (concept)

Write a function that scores passwords by length and character variety, and rejects the top-100 common passwords from a text file. Then read about how **k-anonymity** lets you check a password against the Have I Been Pwned range API without sending the full password or hash.

### 5.18 Where to practice

Use HackerRank's Python domain for syntax drills, LabEx for guided labs, and Codecademy for a structured path. After that, rebuild the projects above from scratch without looking.

### 5.19 Python self-test

1. Difference between a list and a tuple? Between a list and a set?
2. What does `with open(...)` guarantee?
3. Why is `except:` alone a bad idea?
4. How do you read a CSV into a list of dictionaries?
5. Write code that counts word frequency in a file.
6. Why must you never run `eval()` on user input?

---

# 6. Days 43-56: Traffic analysis

**Sources in repo:** Wireshark University, guru99 Wireshark tutorial, Daniel Miessler's tcpdump tutorial, Suricata quickstart, and two YouTube series.
**Goal:** read raw network traffic and recognize normal versus suspicious patterns.

Legal note: only capture on networks you own or are authorized to monitor.

## Days 43-46: Wireshark

### 6.1 What Wireshark is

A GUI packet analyzer. It captures frames from a network interface (or opens a `.pcap`/`.pcapng` file) and decodes them protocol by protocol.

**Interface layout:** packet list (top), packet details tree (middle, layered like OSI), packet bytes (bottom, hex and ASCII).

**Getting traffic to look at:** capture on your own interface, or download sample captures from the Wireshark wiki, or use PCAP exercises from malware-traffic-analysis.net. For a lab, use VMs on a host-only network.

### 6.2 Capture filters vs display filters

- **Capture filter (BPF syntax):** applied *before* capture. Reduces what is saved. Cannot be changed mid-capture. Example: `host 192.168.1.10 and port 80`.
- **Display filter:** applied to already captured packets. Much more powerful. Example: `http.request`.

### 6.3 Display filters you'll use constantly

```
ip.addr == 192.168.1.10                  # source or destination
ip.src == 10.0.0.5 && ip.dst == 10.0.0.9
tcp.port == 443
udp.port == 53
dns                                       # all DNS
dns.qry.name contains "example"
http                                      # all HTTP
http.request.method == "POST"
http.response.code == 404
http.host contains "login"
tls.handshake.type == 1                   # Client Hello
tls.handshake.extensions_server_name      # SNI: the site name even in HTTPS
tcp.flags.syn == 1 && tcp.flags.ack == 0  # SYN only (scan/handshake starts)
tcp.flags.reset == 1                      # resets
tcp.analysis.retransmission               # retransmissions
icmp
arp
arp.duplicate-address-detected            # possible ARP spoofing
frame contains "password"                 # search raw bytes
!(arp || dns)                             # exclude noise
```

Operators: `==`, `!=`, `>`, `<`, `contains`, `matches` (regex), `&&`/`and`, `||`/`or`, `!`/`not`. A green filter bar means valid, red means a syntax error.

### 6.4 Core Wireshark techniques

- **Follow stream:** right-click a packet, then *Follow > TCP Stream* to see the whole conversation reassembled. Best way to read HTTP, FTP or Telnet, and to see credentials sent in cleartext.
- **Statistics menu:**
  - *Conversations* and *Endpoints*: who talks most.
  - *Protocol Hierarchy*: what protocols make up the traffic.
  - *IO Graphs*: traffic over time (spikes = events).
  - *Capinfos*: time span, packet counts.
- **Export objects:** *File > Export Objects > HTTP* extracts files (images, executables) transferred over plain HTTP. Hash them and check on VirusTotal.
- **Coloring rules:** black/red is often problems (resets, errors). Learn them.
- **Time display:** switch to UTC date and time (*View > Time Display Format*) when correlating with logs.
- **Name resolution:** off for speed and privacy unless you need it.
- **Decrypting TLS:** needs session keys (set `SSLKEYLOGFILE` in your own browser) since you can't decrypt without them. This shows *why* encryption works.

### 6.5 What normal traffic looks like

- **DNS lookup:** UDP/53 query, response with A record, then the connection to the resolved IP.
- **Web request:** TCP handshake, (TLS handshake), HTTP request/response, FIN/ACK teardown.
- **DHCP:** Discover, Offer, Request, ACK (UDP 67/68), often broadcast.
- **ARP:** broadcast "who has x?", unicast reply.

### 6.6 Suspicious patterns

| Pattern | What it suggests |
|---|---|
| Many SYNs to many ports from one host, mostly RST/no reply | Port scan |
| Many SYNs to one port, few ACKs | SYN flood |
| Same IP answering ARP for many different IPs, or two MACs claiming one IP | ARP spoofing |
| DNS queries with very long, random-looking names | DNS tunneling or DGA malware |
| Regular, evenly spaced small connections to one external host | Beaconing (C2) |
| Big outbound data transfer at odd hours | Exfiltration |
| Lots of ICMP echo to a range | Ping sweep |
| Cleartext credentials in FTP/Telnet/HTTP POST | Insecure protocol in use |
| Many failed logins (SSH 22, RDP 3389, SMB 445) from one source | Brute force |
| User-Agent strings that look like scripts (`python-requests`, `curl`, `sqlmap`) hitting login pages | Automated probing |
| SMB traffic between workstations | Possible lateral movement |
| DNS to an unusual resolver | DNS hijack or malware |

### 6.7 Analysis workflow (use every time)

1. **Get context:** what is the capture? Time range? Which hosts are yours?
2. **Protocol Hierarchy** and **Conversations**: what dominates?
3. **Find odd ones out:** rare protocols, unusual ports, unknown external IPs.
4. **Filter** and **Follow Stream** into the interesting flows.
5. **Extract IOCs:** IPs, domains, URLs, file hashes, User-Agents.
6. **Write down** the timeline of what happened.

## Days 47-50: tcpdump

### 6.8 What tcpdump is

The command-line packet capturer. Runs on servers where there is no GUI. Typical flow: capture on the server with tcpdump, save to a file, copy it home, analyze in Wireshark.

```bash
sudo tcpdump -D                        # list interfaces
sudo tcpdump -i eth0                   # capture on eth0
sudo tcpdump -i any -nn                # all interfaces, no name/port resolution
sudo tcpdump -i eth0 -c 100            # stop after 100 packets
sudo tcpdump -i eth0 -w capture.pcap   # write to a file
sudo tcpdump -r capture.pcap           # read from a file
sudo tcpdump -i eth0 -A port 80        # show payload as ASCII
sudo tcpdump -i eth0 -X port 80        # hex + ASCII
sudo tcpdump -i eth0 -s 0              # capture full packets (default on modern versions)
sudo tcpdump -i eth0 -vv               # more verbose
```

Common flags: `-n` (no DNS resolution), `-nn` (no DNS or port names), `-e` (show Ethernet headers), `-tttt` (readable timestamps), `-q` (quieter output).

### 6.9 BPF filters (capture and read filters)

```bash
host 192.168.1.10                     # to or from that host
src host 10.0.0.5
dst net 192.168.0.0/16
port 22
portrange 1-1024
tcp / udp / icmp / arp
tcp port 443
not port 22                            # avoid capturing your own SSH session
host 10.0.0.5 and tcp port 80
'tcp[tcpflags] & (tcp-syn) != 0'       # SYN packets
'tcp[tcpflags] & (tcp-syn|tcp-ack) == tcp-syn'   # SYN without ACK
```

Combine with `and`, `or`, `not`, and use parentheses (quote them for the shell).

### 6.10 Reading a tcpdump line

```
14:03:22.415 IP 192.168.1.5.51234 > 93.184.216.34.443: Flags [S], seq 1234, win 64240, length 0
```

- Time, protocol, `source.port > destination.port`.
- **Flags:** `[S]` SYN, `[S.]` SYN-ACK, `[.]` ACK, `[P.]` PSH-ACK (data), `[F.]` FIN, `[R]` RST.
- `seq`, `ack` = sequence numbers. `win` = window size. `length` = payload size.

### 6.11 Handy examples

```bash
# All DNS traffic
sudo tcpdump -i any -nn port 53

# Watch HTTP requests to a server
sudo tcpdump -i eth0 -nn -A 'tcp port 80 and (((ip[2:2] - ((ip[0]&0xf)<<2)) - ((tcp[12]&0xf0)>>2)) != 0)'

# Who is pinging me?
sudo tcpdump -i eth0 -nn icmp

# Rotate captures: 100 MB files, keep 5
sudo tcpdump -i eth0 -w cap_%H%M.pcap -C 100 -W 5

# Count top talkers from a saved file
tcpdump -nr capture.pcap | awk '{print $3}' | cut -d. -f1-4 | sort | uniq -c | sort -rn | head
```

### 6.12 Related tools worth knowing

- **tshark:** Wireshark's command-line twin. Example: `tshark -r cap.pcap -Y "http.request" -T fields -e ip.src -e http.host -e http.request.uri`
- **Zeek (formerly Bro):** turns traffic into structured logs (`conn.log`, `dns.log`, `http.log`, `ssl.log`). Excellent for hunting.
- **NetworkMiner, Scapy, ngrep, Nmap.**

## Days 51-56: Suricata (network IDS/IPS)

### 6.13 What Suricata does

Suricata inspects network traffic in real time against a set of **rules**. Modes:

- **IDS:** passive. Sees a copy of traffic (span port/tap) and alerts.
- **IPS:** inline. Can drop malicious packets.
- **NSM:** logs metadata (DNS, HTTP, TLS, flows) via EVE JSON output even when no rule fires.

Key files on Linux:

| Path | Purpose |
|---|---|
| `/etc/suricata/suricata.yaml` | Main config (HOME_NET, interfaces, outputs) |
| `/var/lib/suricata/rules/suricata.rules` | Merged rules (managed by `suricata-update`) |
| `/etc/suricata/rules/` or `local.rules` | Your custom rules |
| `/var/log/suricata/fast.log` | One-line alerts |
| `/var/log/suricata/eve.json` | Rich JSON logs (feed to ELK) |
| `/var/log/suricata/suricata.log` | Engine log |

### 6.14 Quickstart flow

```bash
sudo apt install suricata
sudo suricata-update                         # download the Emerging Threats Open ruleset
sudo suricata -T -c /etc/suricata/suricata.yaml   # test the configuration
sudo suricata -c /etc/suricata/suricata.yaml -i eth0    # run live
sudo suricata -r capture.pcap -l /tmp/out/    # analyze a pcap offline
tail -f /var/log/suricata/fast.log
```

Edit `HOME_NET` in the config to match your network (for example `192.168.1.0/24`).

### 6.15 Anatomy of a rule

```
alert http $HOME_NET any -> $EXTERNAL_NET any (msg:"LAB Suspicious User-Agent"; flow:established,to_server; http.user_agent; content:"BadBot"; nocase; sid:1000001; rev:1;)
```

| Part | Meaning |
|---|---|
| `alert` | Action: `alert`, `drop`, `reject`, `pass` |
| `http` | Protocol (`tcp`, `udp`, `icmp`, `dns`, `http`, `tls`...) |
| `$HOME_NET any` | Source address and port |
| `->` | Direction |
| `$EXTERNAL_NET any` | Destination address and port |
| `msg` | Text of the alert |
| `flow` | Connection state and direction |
| `http.user_agent` | Sticky buffer: match inside this field |
| `content` | Pattern to match, with modifiers like `nocase` |
| `sid` | Unique rule ID (use 1,000,000+ for your own) |
| `rev` | Revision number |

More keywords: `pcre` (regex), `threshold` (limit alert rate), `classtype`, `reference`, `flowbits` (track state across rules), `dsize` (payload size), `dns.query`, `tls.sni`.

**Example rules for the lab:**

```
# Detect an ICMP echo request to your network
alert icmp any any -> $HOME_NET any (msg:"LAB ICMP echo"; itype:8; sid:1000002; rev:1;)

# Detect DNS lookups for a domain you want to watch
alert dns $HOME_NET any -> any any (msg:"LAB DNS lookup for watched domain"; dns.query; content:"example-bad.test"; nocase; sid:1000003; rev:1;)

# Possible SSH brute force: 5 SYNs to port 22 from one source within 60s
alert tcp any any -> $HOME_NET 22 (msg:"LAB SSH brute-force attempt"; flags:S; threshold:type both, track by_src, count 5, seconds 60; sid:1000004; rev:1;)
```

Add your rule file to the `rule-files:` list in `suricata.yaml`, restart, then trigger the traffic and confirm it appears in `fast.log`.

### 6.16 Tuning and workflow

- **False positives** are normal. Suppress noisy rules (`threshold.config`), or disable a SID in `disable.conf`.
- Keep rules updated (`suricata-update` in cron).
- Forward `eve.json` to your SIEM (the ELK stack in the next section) with Filebeat.
- Test your IDS: replay a pcap with `suricata -r`, or use `tcpreplay`.

### 6.17 IDS families (know the vocabulary)

| Type | Watches | Examples |
|---|---|---|
| NIDS / NIPS | Network traffic | Suricata, Snort, Zeek (analysis) |
| HIDS / HIPS | A single host (files, logs, processes) | Wazuh, OSSEC, AIDE |
| Signature-based | Known bad patterns | Fast, accurate for known threats, misses new ones |
| Anomaly-based | Deviations from a baseline | Finds new threats, more false positives |

### 6.18 Traffic analysis self-test

1. Difference between a capture filter and a display filter?
2. What Wireshark filter shows only SYN packets with no ACK?
3. What does Follow TCP Stream do?
4. What would a port scan look like in a pcap?
5. What are the three Suricata modes?
6. What does `sid` mean, and why must it be unique?
7. Why can't you read HTTPS payloads in Wireshark by default?

---

# 7. Days 57-63: Git

**Sources in repo:** Codecademy Git, Git Immersion, Try Git, Learn Git Branching.
**Goal:** track changes, collaborate, and publish your work (your study notes, scripts, and lab write-ups become a portfolio that recruiters can see).

## Days 57-58: Concepts and first commits

### 7.1 Key ideas

- **Repository (repo):** a project folder plus its full history (stored in the hidden `.git` folder).
- **Commit:** a snapshot of your files with a message and author.
- **Working directory, staging area, repository:** files move from your working folder, to the staging area (`git add`), to a commit (`git commit`).
- **Git vs GitHub:** Git is the tool on your machine. GitHub/GitLab/Bitbucket are websites that host remote repos.
- **Distributed:** every clone has the whole history.

### 7.2 First-time setup

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global init.defaultBranch main
git config --list
```

### 7.3 The basic loop

```bash
git init                         # start a repo in this folder
git status                       # what changed?
git add file.py                  # stage one file
git add .                        # stage everything
git commit -m "Add port scanner"
git log                          # history
git log --oneline --graph --all  # compact graph view
git diff                         # unstaged changes
git diff --staged                # staged changes
```

Write commit messages in the imperative: "Fix login bug", "Add hash script".

### 7.4 `.gitignore`

A file listing what Git must not track:

```
.venv/
__pycache__/
*.pyc
.env
*.pcap
secrets.txt
```

**Never commit secrets** (API keys, passwords, private keys, `.env`). Bots scan GitHub for them within minutes. If you commit one by mistake, revoke and rotate it immediately. Deleting it in a later commit does not remove it from history.

## Days 59-60: Undoing and branching

### 7.5 Undoing things

```bash
git restore file.py              # discard unstaged changes to a file
git restore --staged file.py     # unstage
git commit --amend               # fix the last commit message/contents (only if not pushed)
git revert <commit>              # make a new commit that undoes an old one (safe for shared history)
git reset --soft HEAD~1          # undo commit, keep changes staged
git reset --hard HEAD~1          # undo commit and DELETE changes (dangerous)
git stash ; git stash pop        # shelve work temporarily
```

### 7.6 Branches

A branch is a movable pointer to a commit. Branches let you work on a feature without touching `main`.

```bash
git branch                       # list
git branch feature-scanner       # create
git switch feature-scanner       # move to it (older: git checkout)
git switch -c feature-scanner    # create and switch
git merge feature-scanner        # merge into the branch you are on
git branch -d feature-scanner    # delete after merge
```

**Merge conflicts:** when two branches change the same lines, Git marks the file:

```
<<<<<<< HEAD
your version
=======
their version
>>>>>>> feature-scanner
```

Edit the file to the final result, remove the markers, then `git add file` and `git commit`. VS Code shows "Accept Current / Incoming / Both" buttons on conflicts.

**Rebase** rewrites your commits on top of another branch for a linear history. Don't rebase commits that others already have.

## Days 61-63: Remotes and collaboration

### 7.7 Working with GitHub

```bash
git clone https://github.com/user/repo.git
git remote -v
git remote add origin https://github.com/you/repo.git
git push -u origin main          # first push, sets upstream
git push
git pull                         # fetch + merge
git fetch                        # download without merging
```

**Authentication:** use SSH keys (`ssh-keygen -t ed25519`, add the public key to GitHub) or the GitHub CLI / credential manager. Turn on 2FA.

### 7.8 Pull request workflow

1. **Fork** the repo (your own copy on GitHub) or create a branch if you have access.
2. Clone, create a branch, make changes, commit.
3. Push the branch.
4. Open a **pull request (PR)** on GitHub. Reviewers comment, you update.
5. Maintainer merges.

Good first contributions: fix a broken link or typo (this very repo welcomes those).

### 7.9 Git for security work

- **Version your scripts, notes and lab write-ups.** A public GitHub with clean READMEs of your CTF write-ups and tools is proof of skill.
- **Secret scanning:** tools like `gitleaks` and `trufflehog`; GitHub has push protection.
- **Signed commits** (GPG/SSH) prove authorship.
- **Investigating repos:** `git log -p`, `git blame`, and `git log --all -S "password"` find sensitive data in history.

### 7.10 Git cheat sheet

| Task | Command |
|---|---|
| Start | `git init` / `git clone URL` |
| See state | `git status` |
| Stage and commit | `git add . && git commit -m "msg"` |
| History | `git log --oneline` |
| New branch | `git switch -c name` |
| Merge | `git merge name` |
| Upload | `git push` |
| Download | `git pull` |
| Undo staged | `git restore --staged file` |
| Undo commit (keep work) | `git reset --soft HEAD~1` |

### 7.11 Git self-test

1. What are the three areas a file moves through?
2. Difference between `git fetch` and `git pull`?
3. Why is deleting a leaked secret in a new commit not enough?
4. How do you resolve a merge conflict?
5. What does `.gitignore` do?

---

# 8. Days 64-70: ELK stack

**Sources in repo:** Logz.io "Complete Guide to the ELK Stack" and the official Elastic Stack getting-started docs.
**Goal:** understand how logs are collected, parsed, stored and searched. This is the core of a **SIEM** and of a SOC analyst's daily work.

## Days 64-65: Concepts

### 8.1 Why log management matters

Every system produces logs. Without central collection, an investigation means logging into 50 machines one by one, and attackers delete local logs. A central system gives you search across everything, correlation (login failure on host A + new admin on host B), dashboards, alerts and long-term retention.

### 8.2 The components

| Component | Role |
|---|---|
| **Elasticsearch** | Distributed search and analytics engine. Stores data as JSON **documents** in **indices**. REST API on port 9200 |
| **Logstash** | Data-processing pipeline: **input** > **filter** > **output**. Parses and enriches data. Beats input on port 5044 |
| **Kibana** | Web UI (port 5601): search (Discover), dashboards, visualizations, alerts, security app |
| **Beats** | Lightweight shippers on endpoints: **Filebeat** (log files), **Winlogbeat** (Windows events), **Packetbeat** (network), **Metricbeat** (metrics), **Auditbeat** (audit data) |
| **Elastic Agent + Fleet** | Newer unified agent, centrally managed |

Typical flow:

```
Servers / firewalls / Suricata
        |  (Filebeat / Elastic Agent / syslog)
        v
   Logstash (optional: parse, enrich)
        v
  Elasticsearch  <---->  Kibana (search, dashboards, alerts)
```

Many modern setups skip Logstash and send Beats or Elastic Agent straight to Elasticsearch, using **ingest pipelines** for parsing.

### 8.3 Elasticsearch vocabulary

- **Document:** one JSON record (one log line becomes one document).
- **Index:** a collection of documents (for example `filebeat-2026.09.24`, often organized as **data streams** now).
- **Field:** a key in the document (`source.ip`, `event.action`).
- **Mapping:** field types (keyword, text, ip, date, long).
- **Shard / replica:** pieces of an index spread across nodes for scale and redundancy.
- **Node / cluster:** a running instance / a group of nodes.
- **ILM (Index Lifecycle Management):** hot > warm > cold > delete, to control storage cost.
- **ECS (Elastic Common Schema):** naming convention for fields so different log sources look the same (`source.ip`, `destination.port`, `user.name`).

## Days 66-67: Hands-on setup (lab)

### 8.4 Options to run it

- **Docker Compose** (easiest way to start on a single machine).
- **Elastic Cloud free trial.**
- **Manual install** (deb/rpm/tar) on a VM: at least 4 GB RAM, 8 GB is more comfortable.

Recent versions (8.x and later) enable **security by default**: TLS and authentication are on, and the first start prints a password for the `elastic` user and an enrollment token for Kibana. Follow the official "get started" docs for the exact steps of the version you install, because commands change.

### 8.5 Sanity checks

```bash
curl -k -u elastic:YOUR_PASSWORD https://localhost:9200            # cluster info
curl -k -u elastic:YOUR_PASSWORD https://localhost:9200/_cat/indices?v
```

Then open `http://localhost:5601`, log in, and go to **Discover**.

### 8.6 Ship your first logs with Filebeat

Sample `filebeat.yml` (simplified):

```yaml
filebeat.inputs:
  - type: filestream
    id: auth-logs
    paths:
      - /var/log/auth.log

output.elasticsearch:
  hosts: ["https://localhost:9200"]
  username: "elastic"
  password: "${ES_PASSWORD}"     # use the keystore or env variable, never hard-code
```

Enable modules (they include parsers and dashboards): `sudo filebeat modules enable system`, then `sudo filebeat setup` and `sudo systemctl start filebeat`.

### 8.7 A tiny Logstash pipeline

```
input {
  beats { port => 5044 }
}

filter {
  grok {
    match => { "message" => "%{SYSLOGTIMESTAMP:ts} %{HOSTNAME:host} sshd\[%{NUMBER:pid}\]: Failed password for %{USERNAME:user} from %{IP:src_ip} port %{NUMBER:src_port}" }
  }
  geoip { source => "src_ip" }                 # add location data
  date  { match => ["ts", "MMM dd HH:mm:ss", "MMM  d HH:mm:ss"] }
}

output {
  elasticsearch {
    hosts => ["https://localhost:9200"]
    index => "ssh-failed-%{+YYYY.MM.dd}"
  }
}
```

**Grok** is regex with named patterns like `%{IP:src_ip}`. Test patterns in Kibana's Grok Debugger before deploying.

## Days 68-69: Searching and dashboards

### 8.8 KQL (Kibana Query Language) in Discover

```
event.outcome : "failure"
source.ip : "203.0.113.5"
user.name : "admin" and event.action : "logon-failed"
http.response.status_code >= 500
url.path : "/wp-login.php"
not source.ip : "10.0.0.0/8"
destination.port : (22 or 3389)
process.name : "powershell.exe" and process.args : *encoded*
```

Time filter (top right) matters as much as the query. Set it first.

### 8.9 Visualizations that a SOC actually uses

- **Bar:** top 10 source IPs by failed logins.
- **Line/area:** events per hour (a spike means investigate).
- **Map:** GeoIP of connections.
- **Data table:** users with the most failures.
- **Metric:** count of critical alerts today.
- Combine them into a **dashboard**: "SSH Attack Overview", "Web Server Errors", "Suricata Alerts".

### 8.10 Elasticsearch query DSL (API)

```json
GET filebeat-*/_search
{
  "query": {
    "bool": {
      "must":   [ { "match": { "event.action": "logon-failed" } } ],
      "filter": [ { "range": { "@timestamp": { "gte": "now-1h" } } } ]
    }
  },
  "aggs": {
    "top_ips": { "terms": { "field": "source.ip", "size": 10 } }
  }
}
```

Run in Kibana **Dev Tools**. Aggregations (`aggs`) are how dashboards count and group.

## Day 70: Security use (SIEM)

### 8.11 Detection with Elastic Security

- **Detection rules:** query-based rules that run on a schedule and create alerts. Elastic ships prebuilt rules mapped to **MITRE ATT&CK**.
- **Example detection ideas:**
  - 10+ failed logins from one IP in 5 minutes followed by a success.
  - New user added to the Administrators group.
  - PowerShell running with an encoded command.
  - Outbound connection from a server to an IP with no prior history.
  - Suricata alert of severity 1.
- **Triage flow:** alert > investigate timeline > find related events > decide (true positive, false positive, benign) > escalate or close > document.

### 8.12 Logs worth collecting

| Source | Why |
|---|---|
| Authentication (Linux `auth.log`, Windows Security event log) | Brute force, privilege abuse |
| Windows events: 4624 logon, 4625 failed logon, 4688 process creation, 4720 user created, 4732 added to group, 1102 log cleared | Core Windows detections |
| Sysmon | Deep Windows process, network and file telemetry |
| Firewall / proxy / DNS | Network activity, C2 |
| Web server access logs | Web attacks |
| Suricata `eve.json` | Network alerts and metadata |
| Cloud audit logs (CloudTrail, GCP Audit, Azure Activity) | Cloud abuse |
| EDR alerts | Endpoint threats |

### 8.13 Operating an ELK stack safely

- Never expose 9200 or 5601 to the internet without authentication and TLS. Open Elasticsearch instances leaking data is a classic breach cause.
- Enable role-based access, use API keys instead of admin passwords for shippers.
- Watch disk and JVM memory. Use ILM. Back up with snapshots.
- Keep time in sync (NTP) so timestamps match.
- Alternatives worth knowing by name: **Splunk**, **Microsoft Sentinel**, **Graylog**, **Wazuh** (open source SIEM/XDR built on the same ideas), **Security Onion**, **Grafana Loki**.

### 8.14 ELK self-test

1. What does each of E, L, K do, and what are Beats?
2. What is an index and what is a document?
3. What is grok used for?
4. Write a KQL query for failed logins from one IP.
5. Why is ECS useful?
6. Why must Elasticsearch never be internet-exposed without auth?

---

# 9. Days 71-77: Cloud platforms (GCP, AWS, Azure)

**Sources in repo:** pick **one** of GCP, AWS or Azure. Google Cloud Skills Boost, AWS Getting Started / Cloud Quest, Microsoft Learn Azure Fundamentals.
**Goal:** understand cloud concepts that are the same everywhere, then learn one provider's names.

## 9.1 Core concepts

- **Cloud computing:** renting compute, storage and services over the internet, pay-as-you-go.
- **Service models:**

| Model | You manage | Provider manages | Example |
|---|---|---|---|
| IaaS | OS, apps, data, network config | Hardware, virtualization | EC2, Compute Engine, Azure VMs |
| PaaS | Apps and data | OS, runtime, infra | App Engine, Elastic Beanstalk, Azure App Service |
| SaaS | Your data and user access | Everything else | Microsoft 365, Google Workspace, Salesforce |

- **Deployment models:** public, private, hybrid, multi-cloud.
- **Regions and availability zones:** geographic areas and isolated data centers inside them. Spread across zones for resilience. Data location matters for law (sovereignty).
- **Elasticity and scalability:** grow and shrink with demand.

### The shared responsibility model (most tested cloud concept)

The provider secures **the cloud** (physical data centers, hardware, hypervisor). You secure **what you put in the cloud** (data, identities, configuration, guest OS in IaaS, network rules). Most cloud breaches are **customer misconfiguration**, not provider failure.

## 9.2 Equivalent services

| Need | AWS | Azure | GCP |
|---|---|---|---|
| Virtual machines | EC2 | Virtual Machines | Compute Engine |
| Object storage | S3 | Blob Storage | Cloud Storage |
| Identity and access | IAM | Entra ID (Azure AD) + RBAC | Cloud IAM |
| Virtual network | VPC | VNet | VPC |
| Firewall rules | Security Groups, NACLs | NSGs, Azure Firewall | VPC firewall rules |
| Audit logs | CloudTrail | Activity Log / Monitor | Cloud Audit Logs |
| Monitoring | CloudWatch | Azure Monitor | Cloud Monitoring |
| Threat detection | GuardDuty | Defender for Cloud | Security Command Center |
| Key management | KMS | Key Vault | Cloud KMS |
| Serverless functions | Lambda | Functions | Cloud Functions / Cloud Run |
| Kubernetes | EKS | AKS | GKE |
| Config compliance | Config, Security Hub | Policy | Organization Policy |
| Secrets | Secrets Manager | Key Vault | Secret Manager |

## 9.3 Cloud security essentials

### Identity is the new perimeter

- **Root/global admin accounts:** lock down with hardware MFA, do not use for daily work, no access keys for root.
- **Least privilege IAM:** grant roles narrowly. Avoid wildcard (`*`) permissions.
- **Roles and temporary credentials** over long-lived access keys. Rotate any keys you keep.
- **MFA** for every human user. Use SSO/federation.
- **Service accounts / managed identities** for workloads. Never embed keys in code.

Simplified AWS IAM policy (read-only on one bucket):

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Action": ["s3:GetObject", "s3:ListBucket"],
    "Resource": ["arn:aws:s3:::my-lab-bucket", "arn:aws:s3:::my-lab-bucket/*"]
  }]
}
```

### Storage

- Public buckets are a leading source of data leaks. Enable **block public access**, encrypt at rest, enable versioning and access logging.

### Network

- Put resources in private subnets. Expose only load balancers or bastions. Default-deny security groups. Restrict SSH/RDP to known IPs or use a managed session service instead of open ports.

### Logging and monitoring

- Turn on audit logging (CloudTrail etc.) in **all regions**. Send logs to a separate, protected account/project. Alert on root login, policy changes, disabled logging, and unusual API calls.

### Data protection

- Encrypt with provider-managed or customer-managed keys. Secrets go in a secrets manager. Back up and test restores.

### Cost = security

- Set **budgets and billing alerts**. A leaked key used for crypto-mining shows up first as an unexpected bill.

## 9.4 Common cloud attacks and misconfigurations

| Issue | Description |
|---|---|
| Exposed storage | Public bucket or blob |
| Leaked keys | Access keys in GitHub, Docker images, front-end code |
| Over-permissive IAM | Everyone is admin |
| Open management ports | SSH/RDP 0.0.0.0/0 |
| SSRF to metadata service | Attacker reaches the instance metadata endpoint (`169.254.169.254`) to steal credentials (use IMDSv2 on AWS) |
| Unpatched images, vulnerable containers | Old software running in the cloud |
| Disabled logging | No visibility |
| Cryptojacking | Attackers use your compute |
| Privilege escalation via IAM | Chaining permissions to become admin |

## 9.5 Free ways to learn (choose one provider)

- **AWS:** create a free-tier account, follow the Getting Started tutorials, and try AWS Cloud Quest (gamified). Lab ideas: launch an EC2 instance, create a bucket with a restrictive policy, enable CloudTrail, create an IAM user with MFA.
- **Azure:** Microsoft Learn Azure Fundamentals (AZ-900 path) with free sandbox labs.
- **GCP:** Google Cloud Skills Boost hands-on labs and the "Getting Started" resources.
- **Always set a budget alert and delete resources when the lab ends.**

Certification ladders (optional): AWS Cloud Practitioner > Security Specialty. Azure AZ-900 > SC-900 > AZ-500. GCP Cloud Digital Leader > Professional Cloud Security Engineer.

### 9.6 Cloud self-test

1. Explain shared responsibility with an example.
2. Why are long-lived access keys risky?
3. What is IaaS vs PaaS vs SaaS?
4. Name the audit-logging service on your chosen provider.
5. Give three common cloud misconfigurations.

---

# 10. Days 78-84: Review and practice

**Sources in repo:** go back through days 1-77, TryHackMe, a home lab (VirtualBox/VMware), and a small combined project.

## 10.1 Week plan

| Day | Focus |
|---|---|
| 78 | Review Network+ (subnetting, ports, OSI) and Security+ (crypto, attacks, IR). Write flashcards of what you missed |
| 79 | Linux day: complete a TryHackMe Linux room or repeat LabEx labs without notes |
| 80 | Python day: rebuild two mini projects from scratch |
| 81 | Traffic day: analyze a pcap from malware-traffic-analysis.net and write a short report |
| 82 | Home lab setup (below) |
| 83 | Build the combined project (below) |
| 84 | Write up and publish the project on GitHub with a clear README |

## 10.2 TryHackMe

Free, browser-based, guided rooms. Good order for beginners:

1. **Pre Security** path (networking, web, Linux, Windows basics).
2. **Complete Beginner** or **Jr Penetration Tester** path.
3. **SOC Level 1** path (if you lean towards defence).
4. Rooms: *OpenVPN* (connect), *Linux Fundamentals 1-3*, *Wireshark 101*, *Nmap*, *Intro to SIEM*, *Splunk/Elastic basics*, *OWASP Top 10*, *Burp Suite Basics*.
5. Keep a **streak** and write a short summary of every room.

## 10.3 Home lab

**Hardware:** any machine with 16 GB RAM is comfortable (8 GB is workable with 2 VMs). **Hypervisor:** VirtualBox (free), VMware Workstation Player, or Proxmox if you have spare hardware.

**Suggested VMs:**

| VM | Role |
|---|---|
| Kali Linux | Attacker machine |
| Ubuntu Server | Target with web server, SSH, and a log shipper |
| Windows 10/11 evaluation | Windows target and event-log practice |
| Metasploitable 2/3, DVWA, OWASP Juice Shop | Intentionally vulnerable targets |
| Security Onion or ELK VM | Defender/monitoring machine |

**Network design:** use a **host-only** or **internal network** so vulnerable VMs are never reachable from the internet or your home LAN. Use NAT only for updates on a trusted VM. Take **snapshots** before every experiment so you can roll back.

```
[Kali] ---+
          |--- host-only network 192.168.56.0/24 ---+--- [Ubuntu target + Filebeat]
[Metasploitable / DVWA] --------------------------+--- [ELK / Security Onion (monitor)]
```

## 10.4 Project idea: "Attack and detect"

Combine every skill in the plan.

1. **Network:** design the lab with subnets and a static IP plan.
2. **Linux:** harden the Ubuntu target (SSH keys, ufw, fail2ban).
3. **Attack (in your lab only):** run an Nmap scan from Kali, and attempt SSH password guessing against a test account.
4. **Traffic:** capture with tcpdump and inspect in Wireshark. Note the scan signature.
5. **Detect:** run Suricata with a custom rule, ship `eve.json` and `auth.log` to ELK.
6. **Python:** write a script that parses the failed logins and prints the top offenders (or blocks them with a firewall command).
7. **Git:** commit everything, with a README including a network diagram, screenshots of Kibana alerts and lessons learned.
8. **Security+ tie-in:** map each finding to a control type and to MITRE ATT&CK (for example T1046 Network Service Discovery, T1110 Brute Force).

## 10.5 Retention techniques

- **Feynman method:** explain a topic in simple words, as if to a beginner.
- **Spaced repetition:** Anki flashcards for ports, terms and commands.
- **Write-ups:** one page per lab: goal, steps, what you learned, what failed.
- **Teach:** post a short explanation or answer questions in a community.

---

# 11. Days 85-90: Ethical hacking

**Sources in repo:** Hack The Box, VulnHub, and TheCyberMentor's Ethical Hacking videos.
**Goal:** learn the attacker's workflow so you can defend better, in legal environments only.

## 11.1 Rules first

- **Only test systems you own or have explicit written permission to test.** Unauthorized access is illegal almost everywhere (for example under the CFAA in the US, the Computer Misuse Act in the UK, and equivalent laws in the EU/Spain).
- Practice on labs: HackTheBox, TryHackMe, VulnHub VMs, DVWA, Juice Shop, PortSwigger Web Security Academy, OverTheWire (Bandit), PicoCTF.
- Stay inside the **scope**. Report vulnerabilities responsibly. Never touch real user data.

## 11.2 The methodology

| Phase | Goal | Typical tools |
|---|---|---|
| 1. Reconnaissance | Learn about the target | `whois`, `nslookup/dig`, Google dorking, theHarvester, Shodan, LinkedIn (OSINT) |
| 2. Scanning and enumeration | Find live hosts, open ports, services, versions | Nmap, Nikto, gobuster/ffuf, enum4linux, smbclient |
| 3. Vulnerability analysis | Match services and versions to weaknesses | Searchsploit, Nessus/OpenVAS, manual research, CVE databases |
| 4. Exploitation | Gain access | Metasploit, manual exploits, Burp Suite, sqlmap, Hydra |
| 5. Post-exploitation | Escalate privileges, explore, persist (in scope only) | LinPEAS/WinPEAS, sudo/SUID checks, credential hunting |
| 6. Lateral movement | Reach other systems | Pivoting, pass-the-hash, tunnels |
| 7. Reporting | Explain findings and fixes | Notes, screenshots, risk ratings, remediation advice |

The **report** is the product. Clients pay for clear findings and fixes, not for a shell.

Related frameworks: **PTES**, **OWASP Testing Guide**, **MITRE ATT&CK**, **Cyber Kill Chain** (Recon, Weaponization, Delivery, Exploitation, Installation, C2, Actions on objectives).

## 11.3 Day 85: Recon and scanning

**Passive recon** (never touches the target): whois, DNS records, certificate transparency logs, search engines, public code repos.
**Active recon** (touches the target, needs permission): port scans, banner grabbing.

**Nmap essentials (lab targets only):**

```bash
nmap 192.168.56.101                     # top 1000 ports
nmap -p- 192.168.56.101                 # all 65535 ports
nmap -sV 192.168.56.101                 # service/version detection
nmap -sC 192.168.56.101                 # default scripts
nmap -O 192.168.56.101                  # OS detection (needs root)
nmap -A 192.168.56.101                  # aggressive: -sV -sC -O + traceroute
nmap -sn 192.168.56.0/24                # ping sweep, no port scan
nmap -sS 192.168.56.101                 # SYN ("half-open") scan
nmap -sU --top-ports 20 192.168.56.101  # UDP scan
nmap -Pn 192.168.56.101                 # skip host discovery (if ICMP is blocked)
nmap -oA scan_results 192.168.56.101    # save in all formats
nmap --script vuln 192.168.56.101       # vulnerability scripts
```

Port states: **open**, **closed** (reachable but no service), **filtered** (firewall drops it, can't tell).

**Web enumeration:** directory brute-forcing with `gobuster dir -u http://target -w wordlist.txt` or `ffuf`. Look for `/admin`, backups, `robots.txt`, `.git`, config files.

## 11.4 Day 86: Enumeration of common services

| Service | What to check |
|---|---|
| FTP (21) | Anonymous login, file listing, version |
| SSH (22) | Version, weak/default creds, key reuse |
| SMB (445) | Null sessions, shares, signing, version (`smbclient -L //host -N`) |
| HTTP/HTTPS | Tech stack, hidden paths, login forms, headers, input handling |
| DNS (53) | Zone transfer attempts (`dig axfr`), subdomains |
| SNMP (161) | Default community strings (`public`) |
| MySQL/MSSQL/Postgres | Exposed to the network, default or weak creds |
| RDP (3389) | Exposure, NLA, weak creds |
| SMTP (25) | User enumeration (`VRFY`, `RCPT`) |

Take notes constantly: every credential, path and version. Use a note tool such as Obsidian, CherryTree or plain Markdown (like this file).

## 11.5 Day 87: Web application vulnerabilities (OWASP Top 10 concepts)

| Category | Idea | Defense |
|---|---|---|
| Broken access control | Users access data or functions they shouldn't (IDOR: change `?id=5` to `?id=6`) | Server-side authorization checks on every request |
| Cryptographic failures | Weak or missing encryption, sensitive data exposed | TLS, strong algorithms, proper key handling |
| Injection (SQLi, command, LDAP) | Untrusted input interpreted as code | Parameterized queries, input validation |
| Insecure design | Missing security requirements | Threat modeling, secure design patterns |
| Security misconfiguration | Defaults, verbose errors, open admin panels | Hardening, minimal features, config review |
| Vulnerable and outdated components | Known-vulnerable libraries | SBOM, dependency scanning, patching |
| Identification and authentication failures | Weak passwords, no MFA, session flaws | MFA, rate limiting, secure session handling |
| Software and data integrity failures | Unsigned updates, insecure CI/CD | Signing, integrity checks |
| Logging and monitoring failures | Attacks go unnoticed | Centralized logging, alerting |
| SSRF | Server fetches attacker-chosen URLs | Allow-lists, network segmentation |

Note: the OWASP list is revised every few years, so check owasp.org for the current edition.

**Concept examples (for understanding, practice only in DVWA/Juice Shop/PortSwigger):**

- **SQL injection:** the app builds `SELECT * FROM users WHERE name = '` + input + `'`. Input like `' OR '1'='1` changes the logic. The fix is a **parameterized query**: `cursor.execute("SELECT * FROM users WHERE name = %s", (name,))`.
- **XSS:** an app reflects `<script>` from a search box into the page. The fix is **output encoding** and a Content-Security-Policy.
- **CSRF:** the fix is anti-CSRF tokens and `SameSite` cookies.

**Burp Suite (Community edition) is the standard proxy:** set the browser to use `127.0.0.1:8080`, intercept and modify requests, use Repeater to resend and edit them.

## 11.6 Day 88: Exploitation concepts

- **Vulnerability > exploit > payload.** The vulnerability is the flaw, the exploit is the code that uses it, the payload is what runs afterwards.
- **Shells:** *bind shell* (target listens, attacker connects) vs *reverse shell* (target connects back to the attacker; gets past inbound firewalls). Knowing the difference helps you spot them in traffic.
- **Metasploit framework:** modular; typical flow is `search`, `use <module>`, `show options`, `set RHOSTS`, `run`. Good for learning and for the OSCP-style "know what the tool is doing" mindset. Learn manual exploitation too.
- **Password attacks:** online (Hydra against a service; noisy and lockout-prone) vs offline (Hashcat/John on stolen hashes). Know why long unique passwords, salting and MFA matter.
- **Public exploits:** read the code before running anything from Exploit-DB. Never run unreviewed exploit code, since some are malicious.

## 11.7 Day 89: Post-exploitation and privilege escalation

**Linux checks (in your lab):** `id`, `sudo -l`, SUID files, writable cron jobs, world-writable scripts run by root, kernel version, credentials in config files and history, running services on localhost. (Resource: GTFOBins lists how normal binaries can be abused when misconfigured with sudo/SUID.)

**Windows checks:** `whoami /priv`, unquoted service paths, weak service permissions, stored credentials, AlwaysInstallElevated, token privileges, missing patches. (Resource: LOLBAS project.)

**Defender's view of the same:** minimal sudo rights, patch management, service hardening, credential hygiene, monitoring for enumeration commands and new persistence (scheduled tasks, cron, registry Run keys, new accounts).

**Active Directory (later specialty):** Kerberoasting, AS-REP roasting, pass-the-hash, BloodHound to map attack paths. Study after the basics.

## 11.8 Day 90: Reporting and next steps

A useful finding report contains:

1. **Title** and severity (CVSS or High/Medium/Low).
2. **Description:** what and where.
3. **Impact:** what an attacker gains.
4. **Steps to reproduce.**
5. **Evidence:** screenshots, requests, output.
6. **Remediation:** concrete fix.
7. **References:** CVE, CWE, OWASP.

Also write an **executive summary** for non-technical readers.

**Next steps after day 90:**

- Continue on **HackTheBox** (Starting Point, then Easy boxes), **PortSwigger Academy**, **OverTheWire Bandit**, **PicoCTF**.
- Choose a lane: **SOC/blue team**, **penetration testing/red team**, **cloud security**, **GRC**, **DFIR**, **AppSec**.
- Certifications to consider: Security+ (baseline), CySA+, eJPT / PNPT / PenTest+ (entry pentest), OSCP (advanced), AZ-500/AWS Security (cloud), BTL1 (blue team), GSEC/GCIH (SANS, expensive).

### 11.9 Hacking self-test

1. Name the phases of a pentest in order.
2. What does `nmap -sV -sC` do?
3. What is the difference between a bind and a reverse shell?
4. What is IDOR? What is a parameterized query?
5. Why must a pentester have written authorization?
6. What do the states open, closed and filtered mean in Nmap?

---

# 12. Bonus: Landing the job (Days 91-95)

## 12.1 Days 91-92: One-page resume

**Repo resources:** the BowTiedCyber resume article, Indeed's cybersecurity resume guide, Resume-Now templates.

**Rules:**

- **One page**, clean, ATS-friendly (simple layout; no tables, columns or images that parsers choke on).
- Order: **Header** (name, email, LinkedIn, GitHub) > **Summary** (2-3 lines, target role) > **Skills** (grouped) > **Experience / Projects** > **Certifications** > **Education**.
- **Quantify** achievements: "Detected and documented 12 attack patterns in a home-lab SIEM" beats "did security monitoring".
- **List your hands-on work as experience:** TryHackMe/HTB rankings, home lab, ELK project, Python tools, GitHub repos. Link them.
- Tailor keywords to each job description (SOC analyst, SIEM, Splunk/Elastic, Wireshark, incident response, MITRE ATT&CK, vulnerability management).
- No photo or personal data beyond what's needed. No full home address.
- Proofread. Have someone else read it. Save as PDF for applications.
- **Also save a plain Markdown copy (`cv.md`)**, because the plan reuses it with career-ops.

**Skeleton (`cv.md`):**

```markdown
# Your Name
email | LinkedIn | GitHub | City, Country

## Summary
Entry-level security analyst candidate with hands-on experience in log analysis, network traffic analysis and Linux hardening through a 90-day structured program and home-lab projects.

## Skills
- **Security:** MITRE ATT&CK, incident response, vulnerability scanning, OWASP Top 10
- **Tools:** Wireshark, tcpdump, Suricata, ELK, Nmap, Burp Suite
- **Systems:** Linux (Ubuntu/Kali), Windows, bash, Python, Git
- **Cloud:** AWS (IAM, S3, CloudTrail)

## Projects
### Attack & Detect Home Lab
- Built an isolated lab (Kali, Ubuntu, ELK, Suricata); detected simulated SSH brute force and port scans with custom rules.
- Wrote a Python parser that ranks failed logins by source IP.

## Certifications
- CompTIA Security+ (in progress / date)

## Education
- Degree, school, year
```

## 12.2 Days 93-95: Where and how to apply

**Job boards:** Indeed, LinkedIn Jobs, plus company career pages, local job boards, and **CyberSeek** (maps roles, skills and career pathways, and shows job counts by region).

**Entry-level titles to search:** SOC analyst (Tier 1), security analyst, junior security engineer, vulnerability analyst, IT support with security duties, GRC/compliance analyst, junior penetration tester, cloud security associate, incident response analyst.

**Other tactics:**

- **Network:** LinkedIn, local security meetups, BSides conferences, Discord/Reddit communities. Many jobs are filled through referrals.
- **Public proof:** GitHub with clean READMEs, blog write-ups of CTFs and labs.
- **Interview prep:** explain CIA, OSI, TCP handshake, DNS, common attacks, how you'd investigate a phishing email or a suspicious login, and walk through your lab project. Use the **STAR** method (Situation, Task, Action, Result).
- **Follow up** politely after applying and after interviews.

## 12.3 The career-ops tool (Days 93-95)

**What it is (from the repo):** a free, open-source job-search system that runs inside an AI coding CLI (Claude Code, Codex, OpenCode, GitHub Copilot CLI, Antigravity CLI). You paste a job URL and it scores the posting against your `cv.md` (1-5), generates an ATS-friendly tailored PDF, and tracks applications. **It never applies for you.** You review and submit.

**Three-day flow:**

| Day | Task |
|---|---|
| 93 | Install Node.js, read the setup guide, run `npx @santifer/career-ops init`, open your AI CLI in the created folder, follow onboarding, and state your target roles (SOC analyst, security analyst, junior pentester) |
| 94 | Paste 10-20 postings from Indeed/LinkedIn. **Apply only to roles scoring 4.0/5 or higher.** Treat it as a filter, not a spray-and-pray tool |
| 95 | Generate tailored CVs for your shortlist, apply, and use interview-prep mode to build STAR stories |

**Security habits the repo itself recommends (and you should follow, since you're studying security):**

- **Redact sensitive personal data** (address, phone) in `cv.md` before pasting into any AI tool. Check the provider's privacy and data-retention settings.
- `npx` downloads and runs code from the internet. **Read the release notes first** and **pin a version** (`npx @santifer/career-ops@<version> init`) for a reproducible install.
- General principle: treat any tool that runs code on your machine with the same suspicion you'd apply in a pentest. That is supply-chain security in practice.

---

# 13. Resource index

Every link the repo lists, grouped by section.

## Network+ (days 1-7)
- Professor Messer, Network+ N10-009 playlist (YouTube; search "Professor Messer N10-009")

## Security+ (days 8-14)
- Professor Messer, Security+ SY0-701 playlist (YouTube)
- Pete Zerger, SY0-701 playlist (YouTube alternative)

## Linux (days 15-28)
- Linux Journey: https://linuxjourney.com/
- Cisco NetAcad, Linux Unhatched: https://www.netacad.com/courses/linux-unhatched
- LabEx Linux labs: https://labex.io/free-labs/linux

## Python (days 29-42)
- freeCodeCamp, Learn Python Full Course for Beginners (YouTube, about 4.5 h)
- Codecademy Learn Python: https://codecademy.com/learn/learn-python
- Python.org: https://www.python.org/
- Real Python: https://realpython.com/
- Talk Python to Me: https://talkpython.fm/
- Learn Python the Hard Way: https://learnpythonthehardway.org
- HackerRank Python: https://www.hackerrank.com/domains/python
- LabEx Python labs: https://labex.io/free-labs/python
- TheCyberMentor Python course: https://www.youtube.com/watch?v=egg-GoT5iVk

## Traffic analysis (days 43-56)
- Wireshark education: https://www.wireshark.org/#educationalContent
- guru99 Wireshark tutorial: https://guru99.com/wireshark-tutorial.html
- Daniel Miessler tcpdump tutorial: https://danielmiessler.com/study/tcpdump/
- Suricata quickstart: https://docs.suricata.io/en/latest/quickstart.html
- Wireshark Tutorial for Beginners (YouTube): https://www.youtube.com/watch?v=NjvR4LmwcMU
- Suricata Network IDS/IPS (YouTube): https://www.youtube.com/watch?v=S0-vsjhPDN0

## Git (days 57-63)
- Codecademy Learn Git: https://codecademy.com/learn/learn-git
- Git Immersion: http://gitimmersion.com
- Try Git: https://try.github.io
- Learn Git Branching (interactive simulator)

## ELK (days 64-70)
- Logz.io Complete Guide to the ELK Stack: https://logz.io/learn/complete-guide-elk-stack/
- Elastic official get-started docs: https://www.elastic.co/docs/get-started

## Cloud (days 71-77)
- GCP getting started: https://cloud.google.com/getting-started/
- GCP docs: https://cloud.google.com/docs/
- Google Cloud Skills Boost (hands-on challenges)
- AWS getting started: https://aws.amazon.com/getting-started/
- AWS tutorials: https://aws.amazon.com/tutorials/
- AWS Cloud Quest (gamified labs)
- Azure training: https://learn.microsoft.com/en-us/training/azure/

## Practice and hacking (days 78-90)
- TryHackMe: https://tryhackme.com
- Hack The Box: https://hackthebox.com
- VulnHub: https://vulnhub.com
- TheCyberMentor Ethical Hacking Part 1: https://www.youtube.com/watch?v=3FNYvj2U0HM
- TheCyberMentor Ethical Hacking Part 2: https://www.youtube.com/watch?v=sH4JCwjybGs

## Job search (days 91-95)
- BowTiedCyber resume: https://bowtiedcyber.substack.com/p/killer-cyber-resume-part-ii
- Indeed cybersecurity resume: https://www.indeed.com/career-advice/resumes-cover-letters/cybersecurity-resume
- Resume-Now template: https://www.resume-now.com/templates/cyber-security-resume
- Indeed: https://indeed.com
- LinkedIn: https://linkedin.com
- CyberSeek pathways: https://www.cyberseek.org/pathway.html
- career-ops: `npx @santifer/career-ops init`

## Extra free resources worth adding
- PortSwigger Web Security Academy (web hacking, free)
- OverTheWire Bandit (Linux and security games)
- PicoCTF (beginner CTF)
- malware-traffic-analysis.net (pcap practice)
- MITRE ATT&CK: https://attack.mitre.org
- OWASP: https://owasp.org
- CyberDefenders and LetsDefend (blue team labs)
- Professor Messer's free study notes and practice exams
- NIST Cybersecurity Framework and Incident Response guide (SP 800-61)

---

# 14. Glossary

| Term | Meaning |
|---|---|
| **ACL** | Access control list: ordered allow/deny rules |
| **AES** | Advanced Encryption Standard, symmetric cipher |
| **APT** | Advanced persistent threat: long-term, sophisticated attacker |
| **ARP** | Maps IP addresses to MAC addresses on a LAN |
| **BPF** | Berkeley Packet Filter, capture-filter syntax |
| **C2 / C&C** | Command and control: channel attackers use to direct malware |
| **CIA** | Confidentiality, Integrity, Availability |
| **CIDR** | Notation like `/24` for subnet size |
| **CVE / CVSS** | Vulnerability ID / severity score |
| **DDoS** | Distributed denial of service |
| **DFIR** | Digital forensics and incident response |
| **DLP** | Data loss prevention |
| **DMZ** | Screened subnet for public-facing servers |
| **EDR / XDR** | Endpoint / extended detection and response |
| **ECS** | Elastic Common Schema |
| **GRC** | Governance, risk and compliance |
| **HIDS / NIDS** | Host / network intrusion detection system |
| **IAM** | Identity and access management |
| **IDS / IPS** | Intrusion detection / prevention system |
| **IOC** | Indicator of compromise |
| **IPsec** | Suite for encrypted IP traffic (VPNs) |
| **MFA** | Multi-factor authentication |
| **MITRE ATT&CK** | Knowledge base of attacker tactics and techniques |
| **NAC** | Network access control |
| **NAT / PAT** | Address translation / with ports |
| **OSINT** | Open-source intelligence |
| **PCAP** | Packet capture file |
| **PKI** | Public key infrastructure |
| **RBAC** | Role-based access control |
| **RPO / RTO** | Max data loss / max downtime |
| **SIEM** | Security information and event management |
| **SOAR** | Security orchestration, automation and response |
| **SOC** | Security operations center |
| **SSO** | Single sign-on |
| **TLS** | Transport Layer Security (successor to SSL) |
| **TTP** | Tactics, techniques and procedures |
| **VLAN** | Virtual LAN, a logical network segment |
| **VPN** | Virtual private network |
| **WAF** | Web application firewall |
| **XSS** | Cross-site scripting |
| **Zero-day** | Vulnerability with no available patch |
| **Zero trust** | "Never trust, always verify" security model |

---

# 15. Progress tracker

Toggle the boxes as you go.

## Foundation
- [ ] **Days 1-7:** Network+ (read section 2, pass the self-test, subnet 10 examples)
- [ ] **Days 8-14:** Security+ (read section 3, pass the self-test, make flashcards)
- [ ] **Days 15-28:** Linux (do every command, write one bash script, harden a VM)

## Skills
- [ ] **Days 29-42:** Python (finish 5 mini projects)
- [ ] **Days 43-56:** Traffic analysis (analyze 2 pcaps, write 3 Suricata rules)
- [ ] **Days 57-63:** Git (push a repo, resolve a merge conflict, open a PR)
- [ ] **Days 64-70:** ELK (ingest auth.log, build one dashboard, one alert)
- [ ] **Days 71-77:** Cloud (one provider: IAM + storage + logging lab, set a budget alert)

## Application
- [ ] **Days 78-84:** Review + home lab + "Attack and detect" project on GitHub
- [ ] **Days 85-90:** Hacking (3 easy machines/rooms + one written report)

## Career
- [ ] **Days 91-92:** One-page resume + `cv.md`
- [ ] **Days 93-95:** Applications sent (aim for quality: 4.0/5+ matches)

## Daily log template

Copy this block for every day:

```markdown
### Day N: <topic>
- Date:
- Time spent:
- What I learned (3 lines):
- What confused me:
- Commands/terms to memorize:
- Lab done? (yes/no, link):
```

---

