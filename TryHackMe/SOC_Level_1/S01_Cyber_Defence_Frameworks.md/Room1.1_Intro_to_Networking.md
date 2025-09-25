# 🌐 OSI Model Summary

Mnemonic: **Anxious Pale Shakespeare Treated Nervous Drunks Patiently**  
(Layer 7 → Layer 1)



## 7️⃣ Application Layer
- Interface for programs to use network.
- Passes data to Presentation layer.
- Examples: HTTP, FTP, SMTP, DNS

## 6️⃣ Presentation Layer
- Translates data into standard format.
- Handles encryption, compression.
- Passes data to Session layer.

## 5️⃣ Session Layer
- Establishes, maintains, terminates sessions between computers.
- Ensures multiple requests don’t mix up.
- Passes data to Transport layer.

## 4️⃣ Transport Layer
- Chooses protocol: TCP (reliable) or UDP (fast).
- Splits data into segments (TCP) or datagrams (UDP).
- Ensures delivery & order (TCP) or speed (UDP).

## 3️⃣ Network Layer
- Determines **logical addressing** (IP addresses).
- Finds best route for data.
- Examples: IPv4, IPv6, routing protocols

## 2️⃣ Data Link Layer
- Adds **physical (MAC) address** to packet.
- Ensures data is formatted for transmission.
- Performs error detection for transmission.
- Examples: Ethernet, Switches, NIC

## 1️⃣ Physical Layer
- Transmits raw **electrical/optical/radio signals**.
- Converts binary data ↔ signals.
- Examples: Cables, Hubs, Fiber, Wi-Fi

---

# 🌐 OSI Model: Encapsulation & De-encapsulation

## 1️⃣ Encapsulation (Sending Side)
Data flows **top → bottom** through the layers.  
At each layer, **headers (and sometimes trailers)** are added.

| OSI Layer        | Name of Data             | Info Added (Header/Trailer)                        |
|-----------------|------------------------|--------------------------------------------------|
| 7,6,5           | Data                   | Application/Presentation/Session info           |
| 4 (Transport)    | Segment / Datagram     | Source/Dest Port, protocol info (TCP/UDP)       |
| 3 (Network)      | Packet                 | Source/Dest IP addresses                         |
| 2 (Data Link)    | Frame                  | Source/Dest MAC addresses, CRC trailer          |
| 1 (Physical)     | Bits                   | Electrical/Optical/Radio signals                |

> **Tip:** Frame = packet + data link header + trailer

---

## 2️⃣ De-encapsulation (Receiving Side)
Data flows **bottom → top** through the layers.  
At each layer, the added info is **stripped off** until the application finally sees the original data.

| OSI Layer        | Action                             |
|-----------------|----------------------------------|
| 1 (Physical)     | Receive bits                       |
| 2 (Data Link)    | Check MAC, remove trailer/header  |
| 3 (Network)      | Check IP, remove header           |
| 4 (Transport)    | Check port, reassemble segment    |
| 5,6,7           | Process session/presentation/application data |

---

## ✅ Key Points
1. **Encapsulation** = Adding headers/trailers to data as it goes down the layers.  
2. **De-encapsulation** = Removing headers/trailers as it goes up the layers.  
3. Standardized method = Any device can communicate reliably with any other network device.
4. Each layer has a **specific role** in the process (addressing, reliability, formatting, etc.).
---

# 🌐 TCP/IP Model Overview

## TCP/IP Layers (Original 4-Layer Model)
| Layer               | Role / Function                          | Corresponding OSI Layers |
|-------------------|----------------------------------------|-------------------------|
| Application       | Provides network services to apps       | OSI 7,6,5               |
| Transport         | Reliable or fast delivery (TCP/UDP)    | OSI 4                   |
| Internet          | Logical addressing & routing (IP)       | OSI 3                   |
| Network Interface | Physical + data link layer functions    | OSI 2,1                 |

> Note: Some sources split Network Interface into Data Link & Physical, like OSI.

---

## Encapsulation / De-encapsulation in TCP/IP
Exactly like OSI:

1. **Encapsulation (Sender)**
   - Application → Transport header (TCP/UDP) → Internet header (IP) → Network Interface header/trailer → Bits on the wire
2. **De-encapsulation (Receiver)**
   - Bits → Network Interface → Internet → Transport → Application

---

## TCP Three-Way Handshake (Connection Setup)

**TCP = Connection-Oriented Protocol**

1. **SYN:** Your computer sends a SYN packet to the server (saying "Hi, I want to connect").  
2. **SYN-ACK:** Server replies with SYN + ACK (acknowledging your request and asking to sync).  
3. **ACK:** Your computer replies with ACK (confirming the connection is established).  

✅ Once handshake completes, reliable data transfer starts. Lost/corrupt packets are re-sent.

---

## Why TCP/IP & OSI Models?
- **Before standardization:** Every manufacturer had its own networking method → Incompatibility.  
- **TCP/IP (1982, DoD):** Standardized suite for all manufacturers → Real-world networking basis.  
- **OSI (ISO):** More theoretical and detailed → Great for learning networking concepts.  

> **Summary:** OSI = Learning tool. TCP/IP = Actual standard used in networks today.
---

# Networking Tool: Ping

## What is Ping?

- Ping is a tool used to **check if a device or website is reachable** over a network.
- It helps you know if your computer can **connect to another computer or server**.
- Ping uses the **ICMP protocol** (Internet Control Message Protocol).
- ICMP works at the **Network Layer** in the OSI model and at the **Internet Layer** in TCP/IP model.

---

## How Ping Works

1. Your computer sends a small message called a **ping request** to the target (website or computer).
2. The target receives it and replies with a **ping reply**.
3. If your computer receives the reply, the connection is working. If not, there is a network issue.

Think of it like **sending a letter to your friend** and waiting for their reply. If they reply, you know they got your letter.

---

## Basic Syntax
```
ping <target>
```
**Examples:**
```
ping google.com
ping 192.168.1.1
```
- `google.com` → website
- `192.168.1.1` → IP of a local device (like your router)

---

## What Ping Shows

| Field                   | Meaning                                                                 |
|-------------------------|-------------------------------------------------------------------------|
| Target IP               | Shows the IP address of the website or device you are pinging          |
| Time (ms)               | Time taken for the ping request to reach the target and come back       |
| Packet Loss             | Percentage of ping messages that didn’t get a reply                     |
| TTL (Time To Live)      | Number of hops the ping message can take before being stopped           |

---

## Why Ping is Useful

1. **Check if a website is online**  
```
ping google.com
```
If you get replies → Google is online.

2. **Check local network devices**  
```
ping 192.168.1.1
```
If you get replies → Your router is working.

3. **Find IP address of a website**  
```
ping example.com
```
Output shows the server’s IP address. Handy if you need the IP.

4. **Ubiquitous tool**  
- Works on **almost every device** with network connectivity.  
- Windows, Linux, Mac, even embedded devices like printers support ping.

---

## Example Ping Output
```
Pinging google.com [142.250.190.78] with 32 bytes of data:
Reply from 142.250.190.78: bytes=32 time=20ms TTL=115
Reply from 142.250.190.78: bytes=32 time=21ms TTL=115
Reply from 142.250.190.78: bytes=32 time=19ms TTL=115
Reply from 142.250.190.78: bytes=32 time=20ms TTL=115
```
**Explanation of Output:**

- `142.250.190.78` → IP address of Google
- `time=20ms` → Round-trip time
- `TTL=115` → Maximum hops remaining
- No lost packets → connection is healthy

---

## Summary (Simple Version)

- Ping = **"Are you there?"** message over network.
- Helps **check connectivity** and **find IP addresses**.
- Works everywhere → Linux, Windows, Mac, embedded devices.
- Simple command: `ping <target>`  
- Read results: IP address, response time, packet loss, TTL.

---

# Networking Tool: Traceroute

## What is Traceroute?

- Traceroute is a tool to **see the path your network request takes** from your computer to a target server or device.
- Unlike ping, which only tells you **if the target is reachable**, traceroute tells you **how your request travels through the internet**.
- Each server or router your request passes through is called a **hop**.

Think of it like sending a package:  
Your package doesn’t go directly to the final destination — it passes through multiple post offices. Traceroute shows you each post office your package passes.

---

## How Traceroute Works

1. Your computer sends a special packet to the target.
2. Each router along the way replies with information about itself.
3. Traceroute shows **all the routers (hops) your packet passes through** until it reaches the destination.

---

## Basic Syntax
```
traceroute <destination>
```
**Examples:**
```
traceroute google.com
traceroute 8.8.8.8
```
- `google.com` → website
- `8.8.8.8` → IP of Google DNS server

**Windows Equivalent:**  
```
tracert google.com
```
> Note: Windows `tracert` uses ICMP like ping, Linux `traceroute` uses UDP by default.

---

## Example Traceroute Output
```
traceroute to google.com (216.58.205.46), 30 hops max:
1 192.168.1.1 (192.168.1.1) 1 ms 1 ms 1 ms
2 10.10.0.1 (10.10.0.1) 10 ms 12 ms 11 ms
3 203.0.113.1 (203.0.113.1) 20 ms 21 ms 22 ms
...
13 216.58.205.46 (216.58.205.46) 50 ms 49 ms 51 ms
```
**Explanation of Output:**

| Column | Meaning |
|--------|---------|
| Hop #  | Number of the server/router along the path |
| IP Address | IP of the hop |
| Time (ms) | Time taken for packet to reach this hop and reply |
| `*`     | Timeout or unreachable hop |

- In this example, it took **13 hops** to reach Google.  
- Each row shows one intermediate server/router between your computer and Google.

---

## Why Traceroute is Useful

1. **See where delays happen**  
   - If a website is slow, traceroute shows **which hop is causing the delay**.
2. **Understand network paths**  
   - Shows **how data travels through the internet**.
3. **Debug network issues**  
   - Helps identify **where connections are failing**.
4. **Network learning**  
   - Great way to understand how internet routing works.

---

## Summary (Simple Version)

- Ping = "Are you there?" → checks connectivity.  
- Traceroute = "How did my request travel?" → shows every hop along the path.  
- Linux command: `traceroute <target>`  
- Windows command: `tracert <target>`  
- Output shows **hop number, IP address, and response time**.
---

# Domain Names and Whois

## What are Domain Names?

- Every website has an **IP address** (e.g., 142.250.190.78), but remembering these numbers is hard.
- **Domain names** are human-friendly addresses that map to IPs.
  - Example: `tryhackme.com` → maps to an IP like `52.214.100.1`.
- Domains are leased from **Domain Registrars**, companies that manage the registration.
  - You register a domain for a certain time period (1 year, 2 years, etc.).
  
Think of it like your home address:  
- IP = exact GPS coordinates  
- Domain = easy-to-remember street address

---

## What is Whois?

- **Whois** is a tool to check **who owns a domain**.
- You can see:
  - Who registered the domain
  - When it was registered and renewed
  - Nameservers linked to the domain
- Personal information may be hidden in some regions (like Europe).

---

## How to Use Whois

**Command Syntax:**
```
whois <domain>
```
**Example:**
```
whois bbc.co.uk
```
**Output (simplified example):**

| Field                  | Example / Meaning                                     |
|------------------------|------------------------------------------------------|
| Domain Name            | bbc.co.uk                                            |
| Registrar              | Company that manages the domain registration        |
| Registration Date      | When the domain was first registered                |
| Expiry Date            | When the domain needs renewal                        |
| Nameservers            | Servers that manage domain traffic (DNS servers)    |

---

## Why Whois is Useful

1. **Check ownership** – Find out who owns a website.
2. **Investigate domains** – Useful in cybersecurity investigations.
3. **Learn registration info** – Dates, nameservers, registrar, etc.
4. **Quick lookup online** – Some web-based tools exist if you don’t want to use command line.

---

## Notes for Linux Users

- On Debian/Ubuntu systems, you may need to install whois first:
```
sudo apt update && sudo apt-get install whois
```
- Then run:
```
whois example.com
```
- Output will give you **all public information about the domain**.

---

## Summary (Simple Version)

- Domain names = easy names for hard-to-remember IP addresses.
- Whois = tool to **see domain registration information**.
- Command: `whois <domain>`  
- Example: `whois bbc.co.uk` → shows registrar, registration date, nameservers, etc.
---

# DNS (Domain Name System) and dig Tool

## What is DNS?

- DNS = Domain Name System  
- Converts **human-friendly domain names** into **IP addresses** that computers understand.  
- Without DNS, you would have to remember the IP of every website (imagine remembering `142.250.190.78` instead of `google.com` 😅).

---

## How DNS Works (Step by Step)

1. **Check Hosts File**  
   - Your computer first checks a local file (`Hosts File`) for a manual IP → domain mapping.
   - Rarely used today, but takes **priority over DNS** if present.

2. **Check Local DNS Cache**  
   - Your computer stores previously visited domain IPs in a cache.
   - If found → use it. If not → move to next step.

3. **Query Recursive DNS Server**  
   - Usually provided by your **ISP** or public servers (Google DNS, OpenDNS).  
   - Recursive server checks its own cache. If not found → queries root servers.

4. **Query Root Name Servers**  
   - Root servers direct the query to the correct **TLD (Top-Level Domain) server**.  
   - Example: `.com`, `.co.uk`, `.org`.

5. **Query TLD Servers**  
   - TLD servers know which **Authoritative Name Server** stores the domain information.
   - Example: Request for `tryhackme.com` → `.com` TLD server → correct authoritative server.

6. **Query Authoritative Name Servers**  
   - Authoritative servers store the actual **DNS records** for the domain.  
   - Returns the **IP address** to your computer.  

7. **Computer Connects to IP**  
   - Your computer now uses the IP to connect to the website.

---

## Visual Comparison (Simplified)

| Step | Server Type                  | Role                                   | Example                  |
|------|-----------------------------|---------------------------------------|--------------------------|
| 1    | Hosts File                  | Local mapping                          | 127.0.0.1 mysite.com    |
| 2    | Local Cache                 | Stores recent queries                  | google.com → 142.250.0.1|
| 3    | Recursive DNS Server        | First external query point             | ISP or Google DNS        |
| 4    | Root Name Server            | Directs to TLD server                  | 13 global IPs            |
| 5    | TLD Server                  | Directs to Authoritative server        | `.com`, `.org`, `.uk`    |
| 6    | Authoritative Name Server   | Stores actual domain DNS records       | tryhackme.com → 52.214.0.1 |

---

## The dig Tool

- **dig** = Domain Information Groper  
- Lets you **manually query DNS servers** to get information about a domain.  
- Useful for **network troubleshooting**.

**Syntax:**

dig <domain> @<dns-server-ip>

**Example:**
```
dig google.com @8.8.8.8
```
---

## dig Output (Simplified)

;; ANSWER SECTION:
google.com. 157 IN A 142.250.190.78

| Field | Meaning |
|-------|---------|
| `google.com.` | The domain queried |
| `157` | TTL (Time To Live) in seconds |
| `A` | Type of DNS record (IPv4 address) |
| `142.250.190.78` | IP address returned |

- TTL tells your computer **how long to cache this record**.  
- In this example, TTL = 157 seconds (~2 minutes 37 seconds).

---

## Why DNS + dig is Useful

1. **See IP of a domain**  
dig example.com
2. **Check TTL for caching**  
- Helps know **when cached info will expire**.
3. **Test different DNS servers**  
dig example.com @1.1.1.1
- Queries Cloudflare DNS directly.
4. **Troubleshoot network issues**  
- Check if domain resolves correctly.

---

## Summary (Simple Version)

- DNS = translates **domain → IP** automatically.  
- Process: Hosts file → Cache → Recursive → Root → TLD → Authoritative → IP.  
- dig = tool to **query DNS manually** and get domain info.  
- TTL = **time before cached record expires**.
---

# Networking Basics: Summary & Next Steps

## What We've Learned

- Networking connects computers, devices, and servers so they can **communicate**.
- Key tools we covered:
  1. **Ping** – checks if a device or website is reachable.
  2. **Traceroute** – shows the path your request takes through the network.
  3. **Domains & Whois** – domains = easy-to-remember names; Whois = check ownership info.
  4. **DNS & dig** – DNS converts domain names to IPs; dig queries DNS manually for troubleshooting.

---

## Key Points

- **Ping**: “Are you there?” → checks connectivity.
- **Traceroute**: “How did my request travel?” → shows each hop.
- **Domains**: human-readable addresses → map to IPs.
- **Whois**: find out who owns a domain.
- **DNS**: translates domain → IP.
- **dig**: manually check DNS info and TTL.

---

## How to Continue Learning

1. **Practice**  
   - Use ping, traceroute, dig on different websites.
   - Check TTL values, trace hops, and explore domain info.

2. **Online Communities**  
   - TryHackMe Discord → ask questions, get help.

3. **Books & Theory**  
   - Recommended: *CISCO Self Study Guide* by Steve McQuerry  
     - Designed for CCNA exam, but excellent for a **solid networking foundation**.  
     - Covers networking principles, IP addressing, routing, and more.

---

## Summary (Final Takeaway)

- Networking basics are simple in theory, but **practical experience is key**.
- Tools like ping, traceroute, dig, and Whois help **understand network behavior**.
- Keep experimenting, reading, and practicing to **build a strong foundation**.

