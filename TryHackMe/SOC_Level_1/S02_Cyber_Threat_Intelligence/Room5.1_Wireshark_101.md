# 🧠 Wireshark – Simple Notes for Beginners

## 🌐 What is Wireshark?

Wireshark is a tool used to **see what’s happening in your network**.

When you use internet, many small pieces of data (called **packets**) travel from your computer to other devices or websites.  
Wireshark helps us **watch, record, and analyze** these packets.

It’s like a CCTV camera for your internet traffic.

---

## 💾 What is a PCAP file?

PCAP = **Packet Capture file**

When Wireshark records packets, it saves them in a file called `.pcap`.  
You can open this file later to check what data was sent or received.

So, PCAP = recording of your network activity.

---

## 💻 Installing Wireshark

### On Windows or macOS:
1. Go to the **Wireshark website**.
2. Download the installer.
3. Run it and click **Next > Next > Finish** — just like installing any normal software.

### On Linux:
Type this command in terminal:
apt install wireshark

If you are using **Kali Linux** or **TryHackMe AttackBox**, Wireshark is already installed!

---

## 🏠 First Look (Main Screen)

When you open Wireshark, the first page shows:
- A list of **network interfaces** (like Wi-Fi, Ethernet, etc.)
- A small **graph** next to each interface (shows network activity)

👉 Tip:  
If the graph is **flat (no movement)**, it means no data is going on that interface — so it’s useless to capture there.

You can:
- Start a **live capture** (to see current traffic)
- Or open an **old PCAP file** (to study saved traffic)
<img width="1059" height="287" alt="Screenshot 2025-10-07 at 5 23 47 PM" src="https://github.com/user-attachments/assets/fd64d698-e021-44a0-986a-519c03ac047f" />

---

## 🎬 Live Packet Capture

If you want to record packets that are coming and going in real time:

1. Click on the **green shark fin icon** (top-left)
2. Choose your **interface** (like Wi-Fi)
3. Click **Start Capture**

Now you’ll see packets coming in quickly (or slowly if less network activity).

When you’re done:
- Click the **red square** (Stop button) to stop recording.
<img width="1021" height="302" alt="Screenshot 2025-10-07 at 5 24 02 PM" src="https://github.com/user-attachments/assets/91df5c13-8ef0-4bc8-b0f6-0d00b1ac51ce" />

---

## 🧩 Capture Filters

Filters help you see **only what you want**.

Example:
- You only want to see packets from one IP or one type of protocol.
- You can add a **capture filter** before starting the capture.

To manage filters:
- Go to **Manage Capture Filters** from the green ribbon.

You can still capture without filters — filters just help organize things better.
<img width="987" height="527" alt="Screenshot 2025-10-07 at 5 24 22 PM" src="https://github.com/user-attachments/assets/feb7e7c5-4734-4b44-8086-93373505bfb5" />

---

## 📂 Opening a PCAP file

If you already have a `.pcap` file:
- Go to **File > Open**
- Choose your PCAP file

Wireshark will show a list of packets — like this:

| Packet No. | Time | Source | Destination | Protocol | Length | Info |
|-------------|------|---------|--------------|-----------|---------|------|
| 1 | 0.000000 | 192.168.1.2 | 8.8.8.8 | DNS | 78 | Standard query |
| 2 | 0.001200 | 8.8.8.8 | 192.168.1.2 | DNS | 94 | Standard query response |

---

## 🎨 Color Codes in Wireshark

Wireshark **color codes** packets to make things easy:
- Green = Normal / Safe packets
- Black or Red = Possible problem or suspicious activity
- Blue = Common protocols like DNS, HTTP, TCP

So, colors help you quickly find what’s interesting or dangerous.
<img width="1069" height="364" alt="Screenshot 2025-10-07 at 5 24 52 PM" src="https://github.com/user-attachments/assets/cbad6099-f0fe-450c-b4db-024a3653ea73" />

---

## 🔍 Packet Details

Each packet shows:
- **Packet Number** – order of packets
- **Time** – when the packet was captured
- **Source** – where it came from
- **Destination** – where it is going
- **Protocol** – which type of message (like TCP, HTTP, DNS, etc.)
- **Length** – size of the packet
- **Info** – short description

You can click on any packet to see **full details** below.

---

## 🎮 Practice Tip

Don’t worry too much about remembering everything.  
Just:
- Try opening a PCAP file
- Click packets
- Read info
- Watch how data moves

Play around and explore menus.  
You’ll slowly understand how the internet traffic looks behind the scenes!

---

## 🏁 Summary

| Topic | Meaning |
|--------|----------|
| Wireshark | Tool for watching and analyzing network traffic |
| PCAP | File that stores captured packets |
| Interface | Connection you’re capturing from (Wi-Fi, Ethernet, etc.) |
| Capture Filter | Shows only selected type of packets |
| Color Codes | Help find protocols and danger levels |
| Live Capture | Captures packets happening right now |
| Stop Capture | Click red square to stop recording |

---

## 💡 Final Tip

Think of Wireshark like:
> “A microscope for your network.”

It helps you **see the tiny details** of data moving around your computer —  
like which website your computer is talking to, what protocols are used, and what type of traffic is flowing.

---


# Wireshark — Easy Notes (Simple English, for 9th-grade level)

## Quick intro — what we will learn here
Before we look at each protocol in a PCAP, first we must know **how to get** a PCAP file.  
Getting PCAPs can be simple or tricky — there are different ways (collection methods). Here we explain the theory of each method in simple words.

---

## ✅ Things to check before collecting packets
Before you start sniffing traffic, make sure:

1. Start with a **small sample capture** first to check everything works.
2. Your computer has enough **CPU power** (big networks make lots of packets).
3. You have enough **disk space** to save PCAP files.
4. Pick a collection method (tap, port mirror, MAC flood, ARP poison) — know the risks and get permission.

---

## Collection Methods (short, simple)

### 1) Network Taps
- A **tap** is a physical device placed on a cable to copy traffic.
- Two common types:
  - **Vampire tap** — an older style clamp that clips a wire and reads traffic.
  - **Inline tap** — a small device placed between two devices; it replicates packets for you to capture.
- Who uses taps? DFIR teams, threat hunters, red teams.
- Pro: Reliable and less risky for network health.
- Con: Requires physical access to the cable.

### 2) Port Mirroring (Switch SPAN)
- Not explained deeply in the room, but quick note:
- You configure a switch to **copy (mirror)** traffic from one port or VLAN to another port where your capture machine sits.
- Safe and common in labs and data centers.

### 3) MAC Floods
- A **MAC Flood** attacks the switch's CAM table (the table that maps MAC addresses to ports).
- If you flood many fake MAC addresses, the switch’s table fills up.
- Then the switch starts **flooding traffic to all ports** (like a hub), letting an attacker sniff traffic.
- **Danger:** This can degrade the network. Use only with explicit permission for testing.

### 4) ARP Poisoning (ARP Spoofing)
- ARP (Address Resolution Protocol) maps IP addresses to MAC addresses on the LAN.
- **ARP Poisoning** tricks a victim’s ARP table to send traffic to your machine (you pretend to be the gateway or another host).
- Result: traffic from the victim is redirected to you, you capture it, then forward it on.
- **Less destructive** than MAC flood but still intrusive — use only with consent.

---

## Combining methods
- You can combine taps, port mirrors, MAC floods, and ARP poisoning if needed.
- Best practice: prefer taps or port mirroring first (they are safer and more stable).
- Only use MAC flood / ARP poison when you have permission and no other options.

---

## Capture Filters vs Display Filters — quick difference
- **Capture filters**: applied *before* collecting packets. They decide which packets get saved to the PCAP.
  - Use when you want to **reduce disk usage** and only collect a subset.
- **Display filters**: applied *after* capture, inside Wireshark GUI. They let you **view only the packets you care about** from a large PCAP.
  - More flexible and powerful for analysis.

---

## Basic Filtering Operators (Wireshark display filters)
Wireshark uses simple logical operators — think of normal boolean logic:

- and / &&        → logical AND  
- or  / ||        → logical OR  
- eq / ==         → equals  
- ne / !=         → not equal  
- gt              → greater than  
- lt              → less than

Other useful operators exist (contains, matches, bitwise_and) — those are more advanced.

---

## Common display filter examples (write them exactly as filters)

**Filter by any packet with IP 192.168.1.10**
```

ip.addr == 192.168.1.10

```
<img width="1145" height="161" alt="Screenshot 2025-10-07 at 5 26 02 PM" src="https://github.com/user-attachments/assets/2cb2481c-7a93-4aa2-8909-c4de48f135fb" />

**Filter by packets where source is 192.168.1.10 and destination is 8.8.8.8**
```

ip.src == 192.168.1.10 and ip.dst == 8.8.8.8

```
<img width="1135" height="146" alt="Screenshot 2025-10-07 at 5 26 17 PM" src="https://github.com/user-attachments/assets/e4ff63ee-d76c-4b30-b4fd-d0202a99edd8" />

**Filter by TCP port 80 (HTTP) — matches source or destination port**
```

tcp.port eq 80

```
<img width="1198" height="201" alt="Screenshot 2025-10-07 at 5 26 29 PM" src="https://github.com/user-attachments/assets/bcd79d7a-e785-4d13-833d-7153e1faf4b5" />

**Filter by UDP port 53 (DNS)**
```

udp.port eq 53

```

**Filter only source IP 10.0.0.5**
```

ip.src == 10.0.0.5

```

**Filter only destination IP 10.0.0.5**
```

ip.dst == 10.0.0.5

```

---

## Small examples & explanation (easy)

- If you see lots of packets but you only want traffic from your phone (192.168.1.15), use:
```

ip.addr == 192.168.1.15

```
This shows both packets sent from and to your phone.

- If you only want to see web traffic between two hosts (192.168.1.5 → 172.217.10.14):
```

ip.src == 192.168.1.5 and ip.dst == 172.217.10.14

```

- To look for DNS queries (common when troubleshooting name problems):
```

udp.port eq 53

```

---

## Tips for large captures
- Use **display filters** to narrow results (do not edit the original file).
- Save filtered results into a new smaller PCAP (File → Export Specified Packets).
- Use timestamps and packet numbers to find sequences.
- Practice with small PCAPs first — learn how filters reduce noise.

---

## Safety & Ethics (very important)
- Never sniff a network you do not own or have permission to test.
- Techniques like MAC floods and ARP poisoning can disrupt or break networks — only use in labs or with explicit permission.
- If you're practicing, use a safe lab (virtual machines / isolated network / TryHackMe labs).

---

## Quick comparison table

| Method | What it does | Risk level | When to use |
|--------|---------------|------------|-------------|
| Network Tap | Physically copies wire traffic | Low (if installed correctly) | Best for accurate capture |
| Port Mirroring | Switch copies traffic to capture port | Low | Common in datacenters/labs |
| MAC Flood | Forces switch to broadcast traffic | High | Only in controlled tests |
| ARP Poison | Redirects host traffic to you | Medium-High | Use when no tap/mirror available |

---

## Final small checklist before capturing
- Sample capture OK? ✔  
- CPU & disk space OK? ✔  
- Do you have permission? ✔  
- Collection method chosen? ✔

---

Good! Now you know how PCAPs are gathered, the risks of each method, and the basic Wireshark filters you will use when analyzing packets.  

---


# 🧠 Wireshark — Understanding Packets Using OSI Layers 

## 🌍 What are we learning here?
In this section, we’ll learn **how Wireshark breaks packets into OSI layers** and how these layers help us understand what’s happening inside a captured packet.

If you already know a little about the **OSI model (7 layers)** — this will be super easy!

---

## ⚙️ What happens when you open a packet?

When you **double-click any packet** in Wireshark, you will see a detailed breakdown — like a stack of layers.  
Each layer tells you something about how that packet moved through the network.

Usually, a packet can have **5 to 7 layers**, depending on what kind of data it carries.

Wireshark helps you see these layers clearly and match them to the OSI model.
<img width="824" height="147" alt="Screenshot 2025-10-07 at 5 28 55 PM" src="https://github.com/user-attachments/assets/e7bc5ad7-4440-4250-aba1-1f0db043022b" />

---

## 🧩 The 7 Layers (explained simply with Wireshark view)

Let’s understand each part one by one using an **HTTP packet** example.

---

### 1️⃣ Frame (Layer 1 – Physical Layer)
- Shows the **frame/packet number**, when it was captured, and how big it is.
- This is the **Physical Layer**, so it includes things like how data moves through cables or wireless signals.
- Example: frame number, arrival time, packet length.

🧠 Think of it as: “The envelope that holds the message.”

<img width="597" height="312" alt="Screenshot 2025-10-07 at 5 29 12 PM" src="https://github.com/user-attachments/assets/2dddf844-8ea3-4033-bd8d-7c5a1853b093" />

---
### 2️⃣ Source [MAC] (Layer 2 – Data Link Layer)
- Shows the **Source MAC Address** and **Destination MAC Address**.
- MAC = Media Access Control address → the unique hardware ID of a device’s network card.
- Used by switches to send data within a local network (LAN).

🧠 Think of it as: “The local delivery address inside your building.”

<img width="793" height="92" alt="Screenshot 2025-10-07 at 5 29 32 PM" src="https://github.com/user-attachments/assets/7f5a2c95-889c-493a-8642-0bb82a24990f" />
---

### 3️⃣ Source [IP] (Layer 3 – Network Layer)
- Shows **Source IP Address** and **Destination IP Address**.
- Comes from the **Network Layer**.
- Used to move data from one network to another (like sending data from one city to another over the internet).

🧠 Think of it as: “The postal address on the envelope — helps find the right destination city.”
<img width="577" height="279" alt="Screenshot 2025-10-07 at 5 29 47 PM" src="https://github.com/user-attachments/assets/cc3d453d-6ccd-4a6e-a65e-6d3fe28eba1f" />

---

### 4️⃣ Protocol (Layer 4 – Transport Layer)
- Shows **Transport Protocol** (usually TCP or UDP).
- Includes **source port** and **destination port numbers**.
- Handles data delivery and checks if packets arrive properly.

🧠 Think of it as: “The service used to send your parcel — like choosing between courier types (TCP for reliable, UDP for faster).”
<img width="825" height="395" alt="Screenshot 2025-10-07 at 5 30 05 PM" src="https://github.com/user-attachments/assets/09881408-5d48-4cce-866d-74b551a25e41" />

---

### 5️⃣ Protocol Errors (Still Layer 4)
- If any packets are missing or broken, you’ll see info about **reassembly** or **segment errors**.
- Wireshark shows which TCP segments needed fixing or reordering.

🧠 Think of it as: “The tracking updates showing if something went wrong during delivery.”
<img width="632" height="133" alt="Screenshot 2025-10-07 at 5 30 14 PM" src="https://github.com/user-attachments/assets/4f29b05d-7cbe-483d-8a5d-0b2666a4f20b" />

---

### 6️⃣ Application Protocol (Layer 5 / 7 – Application Layer)
- Shows which **application-level protocol** is being used.
- Examples: HTTP (websites), FTP (file transfer), SMTP (email), SMB (file sharing).
- Lets you see what type of data communication is happening.

🧠 Think of it as: “The type of message inside the parcel — letter, form, or invoice.”
<img width="970" height="314" alt="Screenshot 2025-10-07 at 5 30 27 PM" src="https://github.com/user-attachments/assets/f3af2498-78f4-48a9-8ded-4dcbdb49c528" />

---

### 7️⃣ Application Data (Extension of Application Layer)
- Shows the **actual data** being transferred by the application protocol.
- For HTTP, it could show web requests like `GET /index.html`.
- Sometimes, you can even see the text of the web page or API call here.

🧠 Think of it as: “Reading the message inside the envelope.”
<img width="614" height="96" alt="Screenshot 2025-10-07 at 5 30 38 PM" src="https://github.com/user-attachments/assets/4e776c67-809e-441a-9d4e-53241a1bda35" />

---

## 📊 Summary Table

| OSI Layer | Wireshark Section | What it shows | Example |
|------------|------------------|----------------|----------|
| Layer 1 | Frame | Basic info about packet/frame | Frame number, time, length |
| Layer 2 | Source [MAC] | MAC addresses | 00:1A:2B:3C:4D:5E |
| Layer 3 | Source [IP] | IP addresses | 192.168.1.5 → 8.8.8.8 |
| Layer 4 | Protocol | TCP/UDP and ports | TCP src port 443, dst port 5050 |
| Layer 4 (extra) | Protocol Errors | Segment reassembly or lost packets | “TCP previous segment not captured” |
| Layer 5/7 | Application Protocol | Protocol type | HTTP, FTP, DNS, SMTP |
| Layer 5/7 (extra) | Application Data | Actual transferred content | HTTP GET /index.html |

---

## 🧠 Easy Way to Remember
Think of a packet like a **parcel** being sent:

1. **Frame** – the outer package  
2. **MAC** – local delivery info (building)  
3. **IP** – street address (city/country)  
4. **Protocol** – which courier you use (TCP/UDP)  
5. **Protocol Errors** – delivery tracking  
6. **Application Protocol** – what’s inside (letter/email/web request)  
7. **Application Data** – actual message or content  

---

## ✅ What to do next
Now that you understand what each layer shows, you can:
- Click packets in Wireshark and expand the arrows (▶) to explore each layer.
- Notice how each layer gives a different kind of information.
- You’re ready to move on and learn **specific protocols** (HTTP, DNS, TCP, etc.) in more detail!

---

# 🧠 Wireshark — ARP and ICMP Analysis 

## 🌍 What we are learning
In this part, we’ll learn about **ARP (Address Resolution Protocol)** and **ICMP (Internet Control Message Protocol)** — both are important when analyzing network traffic in Wireshark.

They help devices **talk** and **test communication** inside a network.

---

## ⚡ ARP Overview (Layer 2 Protocol)

### What is ARP?
**ARP (Address Resolution Protocol)** helps connect an **IP Address (Layer 3)** with a **MAC Address (Layer 2)**.  
Basically, it helps your computer find the physical address (MAC) of another computer using its IP.

🧠 Example:
You know your friend’s name (IP Address), but you don’t know their phone number (MAC Address).  
So you ask, “Who has IP 192.168.1.5?”  
The owner replies, “That’s me! My MAC is 00:11:22:33:44:55.”

That’s ARP in action!

---

## 📦 ARP Packet Types

ARP packets have **two operation codes** in their header:

| Opcode | Meaning | Description |
|---------|----------|-------------|
| 1 | Request | Asking “Who has this IP?” |
| 2 | Reply | Answering “I have that IP; here’s my MAC.” |
<img width="925" height="170" alt="Screenshot 2025-10-07 at 5 32 22 PM" src="https://github.com/user-attachments/assets/92eb4965-9db3-4f22-bd26-4dfb4478a7da" />

---

## 🔍 Viewing ARP in Wireshark

When you capture traffic:
- You’ll see lines like **ARP Request** and **ARP Reply**.

- Devices are often labeled (like `Intel_78` or `Cisco_12`) so you can identify them easily.
- If you see **many ARP requests** from a strange or unknown source → could be **suspicious activity**.

### ✅ Tip:
To show MAC address names in Wireshark:
Go to → **View > Name Resolution > Check “Resolve Physical Addresses.”**

Now Wireshark will show vendor names like *Intel* or *Cisco* instead of raw MAC addresses.

---

## 🧩 ARP Traffic Overview

### 1️⃣ ARP Request Packet
- Opcode: **1** (Request)
- Message: Asking “Who has [IP Address]?”
- Target: Usually sent to **broadcast** (everyone on the network)
- Example:  
  “Who has 192.168.1.10? Tell 192.168.1.5.”

In Wireshark:
- Check the **Opcode** (should say “Request”).
- Check the **Target IP** (the IP being asked for).
- Check **Destination MAC** (usually ff:ff:ff:ff:ff:ff → broadcast).
<img width="738" height="290" alt="Screenshot 2025-10-07 at 5 33 11 PM" src="https://github.com/user-attachments/assets/bc15b0cc-5af5-4a7b-8abd-16db92cf29a5" />

---

### 2️⃣ ARP Reply Packet
- Opcode: **2** (Reply)
- Message: “192.168.1.10 is at 00:11:22:33:44:55.”
- Contains:
  - Source MAC (the responder’s MAC)
  - Source IP (the responder’s IP)
- This is **unicast**, sent only to the original requester.

🧠 Simple rule:
- **Opcode 1** → Request  
- **Opcode 2** → Reply  

That’s it! ARP is one of the easiest protocols to read.
<img width="566" height="291" alt="Screenshot 2025-10-07 at 5 33 20 PM" src="https://github.com/user-attachments/assets/f3e80d1e-bd90-48f8-9f8f-3b5bf75b07d1" />

---

## 🚨 Suspicious ARP Activity
Watch for:
- Many ARP requests from one unknown source.
- Fake replies (possible **ARP Spoofing**).
- Same IP but different MAC addresses (another ARP attack sign).

---

## 🌐 ICMP Overview (Layer 3 Protocol)

### What is ICMP?
**ICMP (Internet Control Message Protocol)** helps test if devices are reachable and how fast the connection is.  
It’s the protocol behind **ping** and **traceroute** commands.

🧠 Example:
You “ping” a website → your computer sends an **ICMP Request**, the site sends an **ICMP Reply** back.
<img width="1114" height="80" alt="Screenshot 2025-10-07 at 5 33 38 PM" src="https://github.com/user-attachments/assets/371a0631-b48b-478e-9020-ef5279f911b7" />

---

## 🧾 ICMP Packet Types

ICMP uses **Type** and **Code** numbers to describe messages.

| Type | Code | Meaning |
|------|------|----------|
| 8 | 0 | Echo Request (Ping request) |
| 0 | 0 | Echo Reply (Ping reply) |

When `Type = 8` → your computer is asking  
When `Type = 0` → it’s replying

---

## 📊 ICMP Traffic Overview

### 1️⃣ ICMP Request Packet
- Type = **8**
- Code = **0**
- Sent from your device → to another device/server
- Includes:
  - **Timestamp** (time ping was sent)
  - **Data section** (random bytes of data)

In Wireshark:
- Expand **Internet Control Message Protocol** in the details pane.
- Look for **Type: 8 (Echo Request)**.
- Note **Source IP** (you) and **Destination IP** (server).
<img width="791" height="405" alt="Screenshot 2025-10-07 at 5 34 24 PM" src="https://github.com/user-attachments/assets/d597a64a-5e90-4bce-a8dc-83f772ac33cb" />

---

### 2️⃣ ICMP Reply Packet
- Type = **0**
- Code = **0**
- Sent back from the server → to your device
- Confirms that the other device is reachable.
- Contains:
  - Same **data string** (should match the request)
  - Timestamp (helps calculate response time)

🧠 If a reply is missing or slow → might mean:
- The server is down.
- There’s network delay.
- Or ICMP is blocked by a firewall.
<img width="778" height="417" alt="Screenshot 2025-10-07 at 5 34 38 PM" src="https://github.com/user-attachments/assets/8fa7fd92-ce78-4ce1-aa78-d5df719ce85d" />
---

## 🚨 Suspicious ICMP Activity
ICMP is also used by attackers for:
- **Ping floods** (too many pings = DoS attack)
- **ICMP tunnels** (hiding secret data inside ICMP packets)
So, if you see weird or constant ICMP traffic — be alert!


---

## ✅ Summary Table

| Protocol | Layer | Purpose | Key Fields | Normal Use | Suspicious Sign |
|-----------|--------|----------|-------------|--------------|----------------|
| ARP | Layer 2 | Connect IP ↔ MAC | Opcode, MAC, IP | Address mapping | Many unknown requests |
| ICMP | Layer 3 | Test connectivity | Type, Code, Timestamp | Ping, traceroute | Wrong codes, too many pings |

---

## 🧠 Quick Recap
- ARP helps devices **find each other inside a LAN**.
- ICMP helps devices **check if they can reach each other**.
- Both are easy to read in Wireshark using the **Opcode (for ARP)** and **Type/Code (for ICMP)**.
- Always watch for unusual or repetitive patterns — they could hint at attacks.

---

Now that you understand ARP and ICMP, you’re ready to analyze more complex protocols like **TCP**, **UDP**, and **HTTP** next!

---


# ⚙️ Wireshark — TCP and DNS Analysis (Simple English)

## 🌍 What we are learning
In this section, we’ll explore **TCP (Transmission Control Protocol)** and **DNS (Domain Name System)** — two of the most important protocols you’ll see in Wireshark.

TCP ensures **reliable delivery of data**, while DNS helps **translate domain names into IP addresses**.

---

## 🚀 TCP Overview (Layer 4 Protocol)

### What is TCP?
**TCP (Transmission Control Protocol)** is responsible for making sure that data sent between devices arrives safely, in the right order, and without errors.

🧠 Think of TCP as a postal worker who:
- Puts numbers on each letter (sequencing)
- Makes sure every letter arrives
- Asks for confirmation (acknowledgment) after delivery

---

## 📊 TCP in Wireshark

Wireshark color-codes TCP packets based on their type and importance.  
When scanning a network (like with **Nmap**), you might see different responses such as:

- **SYN, SYN-ACK, ACK** → Normal connection setup  
- **RST, ACK** → Port is **closed** (like in an Nmap scan)

🧩 Example:
If you scan port **80** (HTTP) or **443** (HTTPS) and see **RST, ACK**, that means those ports are not open.
<img width="1092" height="106" alt="Screenshot 2025-10-07 at 5 36 24 PM" src="https://github.com/user-attachments/assets/5adb2e28-24d8-4eb6-b218-b85037884385" />

---

## 🧩 TCP Handshake (Connection Setup)

The **TCP handshake** is a 3-step process that establishes a connection:

| Step | Flag | Description |
|------|------|--------------|
| 1️⃣ | SYN | Client says, “I want to connect.” |
| 2️⃣ | SYN + ACK | Server says, “Okay, I’m ready.” |
| 3️⃣ | ACK | Client says, “Let’s begin!” |

Once these three steps complete, the connection is **established**.

### 🔎 What to Watch For
If the handshake is:
- **Out of order**
- **Missing**
- Or contains extra flags (like **RST**)

→ It could signal **suspicious activity**, scanning, or a failed connection.

---<img width="1195" height="78" alt="Screenshot 2025-10-07 at 5 36 40 PM" src="https://github.com/user-attachments/assets/f8ededdf-387e-437a-a3ab-d3c7afb017e8" />


## 📦 TCP Packet Analysis

When analyzing TCP packets in Wireshark, focus on:

- **Sequence Number** – Identifies the order of the data segment.  
- **Acknowledgment Number** – Confirms the data received.  
- **Flags** – Such as SYN, ACK, RST, FIN, etc., which show the packet’s purpose.

🧠 Example:
- In the **first SYN packet**, there’s **no acknowledgment number** because the client hasn’t received any data yet.
- In later packets, the **acknowledgment number** tells how many bytes have been successfully received.
<img width="1132" height="475" alt="Screenshot 2025-10-07 at 5 37 17 PM" src="https://github.com/user-attachments/assets/828cec52-8127-4d25-baa7-84106b68a659" />

### ⚙️ Wireshark Tip
To view **original sequence numbers**:
Go to **Edit > Preferences > Protocols > TCP**  
→ Uncheck **“Relative Sequence Numbers”**

<img width="793" height="573" alt="Screenshot 2025-10-07 at 5 37 56 PM" src="https://github.com/user-attachments/assets/ddaa2937-0c83-4be7-83e3-0252bcfdd6e3" />
<img width="1118" height="416" alt="Screenshot 2025-10-07 at 5 38 05 PM" src="https://github.com/user-attachments/assets/b1791ad8-2c94-404c-ba5b-863eb256ac91" />

---

## 📚 How to Analyze TCP Traffic
Unlike ARP or ICMP, **TCP needs to be viewed as a series of packets**, not individually.  
Each packet is part of a **conversation**, so analyzing them together tells the full story of what happened.

### 🧠 Example Scenarios
- **SYN flood** → Too many SYN packets without ACKs → Possible DoS attack  
- **RST, ACK from server** → Port closed  
- **Out-of-sequence packets** → Possible network congestion or tampering

---

## 🌐 DNS Overview (Layer 7 Protocol)

### What is DNS?
**DNS (Domain Name System)** converts **domain names** (like *www.google.com*) into **IP addresses** (like *142.250.72.206*).  
It’s like a phonebook for the internet.

🧠 Example:
When you type *www.example.com* into your browser:
1. Your computer asks a DNS server, “What is the IP for www.example.com?”
2. The DNS server replies with the correct IP address.
<img width="1147" height="151" alt="Screenshot 2025-10-07 at 5 38 25 PM" src="https://github.com/user-attachments/assets/57699833-0c26-4fe9-9600-dafce3d8df3a" />

---

## 📦 DNS Packet Basics

DNS typically uses:
- **UDP port 53** (faster, no handshake)
- Occasionally **TCP port 53** (used for large queries or zone transfers)

If DNS packets **don’t follow this rule**, it might be **suspicious**.

---

## 🔍 DNS Traffic Overview

### 1️⃣ DNS Query
When your device wants to visit a website, it sends a **DNS query** to a DNS server.

In Wireshark, check:
- **Protocol:** DNS (over UDP)
- **Source port:** Random
- **Destination port:** 53 (standard DNS port)
- **Query name:** The domain being requested

🧠 Example:
If you see a DNS query for an unknown or strange domain (like `abc123.xyz.ru`), it could be malware or beaconing activity.
<img width="921" height="310" alt="Screenshot 2025-10-07 at 5 38 43 PM" src="https://github.com/user-attachments/assets/365ece69-fd18-4a01-90f7-18195452b596" />

---

### 2️⃣ DNS Response
The DNS server sends a **response packet** back, containing the answer to the query.

In Wireshark:
- Check **Flags: Standard query response**
- Look at **Answers section** → it shows the IP address for the domain.

🧩 Example:
```

Query: [www.google.com](http://www.google.com)
Answer: 142.250.72.206

```

If the **response doesn’t match** the query or points to an **unfamiliar IP**, it might indicate **DNS spoofing**.
<img width="917" height="390" alt="Screenshot 2025-10-07 at 5 38 52 PM" src="https://github.com/user-attachments/assets/b82b9403-5ed2-4906-81ff-e42d374fc742" />

---

## 🚨 Suspicious DNS Behavior
Be cautious if you see:
- DNS using **TCP** instead of UDP (except for zone transfers)
- **Unusual domains** or frequent lookups
- **High number of queries** to unknown servers
- DNS replies with **unexpected IPs**

These can indicate malware, tunneling, or command-and-control (C2) traffic.

---

## ✅ Summary Table

| Protocol | Layer | Purpose | Key Fields | Normal Use | Suspicious Sign |
|-----------|--------|----------|-------------|--------------|----------------|
| TCP | Layer 4 | Reliable data delivery | Seq #, Ack #, Flags | Normal communication | Out-of-order, RST flood |
| DNS | Layer 7 | Domain ↔ IP resolution | Query, Response, UDP 53 | Web browsing | TCP 53, strange domains |

---

## 🧠 Quick Recap
- **TCP** ensures data is sent and received correctly using a handshake and sequence numbers.  
- **DNS** translates human-readable names into IPs.  
- Both can reveal a lot about **network health or attacks** when analyzed carefully in Wireshark.

---


# Wireshark — HTTP & HTTPS (Simple English, for 9th-grade level)

## Quick intro
This note explains **HTTP** (plain web traffic) and **HTTPS** (encrypted web) in a very simple way. We'll also cover how to **analyze HTTP in a PCAP** and how to **decrypt HTTPS** in Wireshark using an RSA key (when you have the key).

---

## 🔹 HTTP (Hypertext Transfer Protocol)

### What is HTTP?
- HTTP is the older, **unencrypted** protocol for web pages.
- Browsers use it to send **GET** and **POST** requests to web servers and receive pages or data.
- Because it is not encrypted, you can **see the full request and response** in Wireshark.

### Why it matters
- In HTTP you can read the **Request URI**, **headers** (like User-Agent), and **response body**.
- This makes it easy to spot **bad stuff** like SQL injection strings, web shells, or suspicious POST data.
<img width="1132" height="366" alt="Screenshot 2025-10-07 at 5 41 35 PM" src="https://github.com/user-attachments/assets/1fc5b7a7-1b17-49c6-9a1c-66bbf2f55816" />
### Example: what an HTTP packet shows
- Host
- User-Agent
- Request URI (example: GET /index.php?id=1)
- Response (HTML, JSON, etc.)
<img width="1113" height="329" alt="Screenshot 2025-10-07 at 5 42 36 PM" src="https://github.com/user-attachments/assets/934b9ea6-4df7-40d2-a722-598de2313327" />


### Wireshark features useful for HTTP
- **Protocol Hierarchy**: Statistics > Protocol Hierarchy — shows which protocols are in the capture.
<img width="1131" height="206" alt="Screenshot 2025-10-07 at 5 42 50 PM" src="https://github.com/user-attachments/assets/15436c12-41a4-4a83-9be1-ecfc076debe5" />


- **Export HTTP Objects**: File > Export Objects > HTTP — saves requested files and pages seen in the capture.

<img width="850" height="136" alt="Screenshot 2025-10-07 at 5 43 06 PM" src="https://github.com/user-attachments/assets/1990774a-0d37-41f2-b766-caa33cb5c97a" />

- **Endpoints**: Statistics > Endpoints — lists all IPs/endpoints in the capture.
<img width="804" height="211" alt="Screenshot 2025-10-07 at 5 43 23 PM" src="https://github.com/user-attachments/assets/e2a3d4cb-cf9c-4e34-a85b-a3aba94f113f" />

### Practical tip
- Open a small HTTP PCAP (like `task11.pcap`) and click a packet: expand the `Hypertext Transfer Protocol` section to read headers and body.
- Use Export HTTP Objects to pull all pages and files for easier review.

---

## 🔒 HTTPS (HTTP Secure / TLS)

### What is HTTPS?
- HTTPS is HTTP over **TLS (secure encryption)**. Almost all modern websites use HTTPS.
- Before any real HTTP data is sent, the client and server run a **TLS handshake** to agree on keys and algorithms.

### TLS handshake (simple steps)
1. **Client Hello** — Browser says: here are the TLS versions and ciphers I support.
  <img width="1198" height="344" alt="Screenshot 2025-10-07 at 5 43 56 PM" src="https://github.com/user-attachments/assets/4e2d6ebc-ccfc-43e7-86aa-4c9b24faffed" />


2. **Server Hello** — Server chooses version/cipher and sends certificate.
<img width="1120" height="533" alt="Screenshot 2025-10-07 at 5 44 48 PM" src="https://github.com/user-attachments/assets/7d82478d-ac6b-4725-9894-93737b18f1cb" />


3. **Client Key Exchange** — Client sends the key material (or encrypted pre-master) to agree on session key.
<img width="1155" height="390" alt="Screenshot 2025-10-07 at 5 45 07 PM" src="https://github.com/user-attachments/assets/1e497920-fb55-4f10-a93a-17d8bb837b57" />


4. **Finished / Server Finished** — Both sides confirm and then start sending **encrypted application data** (HTTPS).
<img width="1107" height="374" alt="Screenshot 2025-10-07 at 5 45 38 PM" src="https://github.com/user-attachments/assets/ac19a9ff-a748-4c71-8b1d-3ed5b0b51ba7" />

After the handshake, **everything is encrypted**, so you cannot read HTTP headers or body unless you can decrypt.

### What you see in Wireshark before decryption
- Client Hello, Server Hello, Certificate, Client Key Exchange
- After that: `Application Data` — encrypted bytes (no readable HTTP)

---

## 🔑 Decrypting HTTPS in Wireshark (when you have the key)

> You can only decrypt TLS if you have the server's private RSA key (or use other methods like SSLKEYLOGFILE for browsers that support it).

### How to load an RSA private key into Wireshark
1. Edit > Preferences > Protocols > TLS (or SSL in old versions)
2. Click `+` to add a key entry
3. Fill fields exactly like:
```

IP Address: 127.0.0.1
Port: start_tls
Protocol: http
Keyfile: /path/to/rsa_key.pem

```
4. Click OK and reload the capture (or re-open the PCAP).

**If key matches**, Wireshark will decrypt TLS and show the HTTP requests/responses in plain text.

### Example lab (snakeoil2)
- In the AttackBox: extract `task12.zip` and open `snakeoil2` PCAP.
- Load the provided RSA key in Wireshark as above.
- After loading, packets that were `Application Data` should now show `Hypertext Transfer Protocol` content.
- You can then use Export HTTP Objects to save these files.
<img width="857" height="303" alt="Screenshot 2025-10-07 at 5 46 35 PM" src="https://github.com/user-attachments/assets/dc9545d4-c31c-4839-8841-f993314322a4" />

---

## 🔎 Quick checks for suspicious behavior
- **HTTP**: unusual URIs, base64 payloads in POST, forms that look like shells, SQLi patterns.
- **HTTPS**: you can’t read it unless decrypted — watch for unusual servers, odd TLS versions, expired certs, or clients using weak ciphers.
- **Handshake oddities**: Client Hello or Server Hello missing steps, mismatched cipher suites, or repeated failed handshakes can be suspicious.

---

## ✅ Short summary
- **HTTP**: easy to read in Wireshark — you can see GET/POST, headers, and content.
- **HTTPS**: encrypted; you must decrypt (with RSA key or session keys) to view content.
- **Wireshark tools**: Protocol Hierarchy, Export HTTP Objects, Endpoints help analyze captures quickly.

---

# Zerologon PCAP 

## Quick summary
We have a PCAP that shows an attack using **Zerologon (CVE-2020-1472)**.  
- **Domain Controller (DC)** IP: `192.168.100.6`  
- **Attacker** IP: `192.168.100.128`  

We will learn how to spot the attacker, what traffic to look for, and a short hypothesis of what happened.

---

## 1) First look — what jumps out
When you open the PCAP, Wireshark may show normal traffic (OpenVPN, ARP) and then some **unusual protocols** like `dcerpc` and `epm`.  
If one IP (here `192.168.100.128`) is sending many of these suspicious requests, that IP is likely the **attacker**.
<img width="1056" height="502" alt="Screenshot 2025-10-07 at 5 47 43 PM" src="https://github.com/user-attachments/assets/033e09fc-a827-47bd-bd9a-5dedfc2603eb" />

---

## 2) Useful display filters to start your hunt
Type these filters in the Wireshark filter bar exactly as shown:

- Show all traffic from the suspected attacker:
```

ip.src == 192.168.100.128

```

- Show DCERPC traffic:
```

dcerpc

```

- Show SMB2/SMB3 traffic:
```

smb2 || smb

```

- Show DRSUAPI calls (used by secretsdump):
```

drsuapi

```

Combine filters to narrow faster:
```

ip.src == 192.168.100.128 and dcerpc

```

---

## 3) How we identified the attacker
- We saw `192.168.100.128` making many RPC/DCERPC/EPM connection attempts to the DC.  
- The volume and pattern (many repeated RPC calls) are abnormal for normal user behavior.  
- So we mark `192.168.100.128` as **suspicious / likely attacker**.

---

## 4) Zerologon artifacts to look for (high level)
If you know Zerologon, its PCAP artifacts include:
- Many short RPC/DCERPC connections to the Domain Controller.
- Attempts to change the machine account password via RPC.
- After success, follow-up actions such as using SMB or other RPC APIs to extract secrets.

(Threat intelligence about specifics is out of scope here — but these high-level signs are the starting point.)
<img width="1046" height="508" alt="Screenshot 2025-10-07 at 5 49 35 PM" src="https://github.com/user-attachments/assets/444098b5-24c8-494a-a3bf-8c0db7ce9a4f" />

---

## 5) Secretsdump / credential dumping signs
In this PCAP we see **SMB2/3** and **DRSUAPI** traffic after the initial exploit.  
`secretsdump` often abuses SMB/DRSUAPI to read NTDS or dump hashes. Signs include:
- SMB sessions with long data transfers after the RPC activity.
- DRSUAPI requests/responses (look for `drsuapi` protocol in Wireshark).

Filter examples:
```

ip.src == 192.168.100.128 and (smb2 || drsuapi)

```
<img width="1079" height="413" alt="Screenshot 2025-10-07 at 5 48 53 PM" src="https://github.com/user-attachments/assets/013ffeaf-4c83-43f7-b49d-2ccf28e16cf6" />

---

## 6) Telling the story (simple hypothesis)
1. Attacker (`192.168.100.128`) sends many DCERPC calls to the DC (`192.168.100.6`).  
2. One or more Zerologon exploit attempts succeed (machine account password change).  
3. Using the new access, the attacker runs `secretsdump` behavior — communicates over SMB/DRSUAPI to extract credentials/hashes.  
4. Attacker collects sensitive info and possibly prepares for lateral movement.

This order is inferred from the timing and types of packets in the capture.

---

## 7) Quick analysis steps you can follow
1. Open the PCAP in Wireshark: `File > Open > <pcap file>`.  
2. Apply filter for attacker IP: `ip.src == 192.168.100.128`.  
3. Inspect first unusual packets — look at `dcerpc` / `epm` fields.  
4. Note any sequence where RPC calls are followed by SMB/DRSUAPI traffic.  
5. Use `Statistics > Endpoints` and `Statistics > Protocol Hierarchy` to see which hosts/protocols are most active.  
6. Save suspicious packets: `File > Export Specified Packets` for reporting or deeper tools.

---

## 8) What to do after confirming attack (high-level response)
- **Isolate** the attacker machine from network immediately.  
- **Quarantine** the affected Domain Controller if possible and safe to do so (follow your org incident playbook).  
- **Collect logs** (Windows event logs, DC logs) and the PCAP for investigation.  
- **Report** to your security team/manager and start an incident response workflow.  
- **Patch / remediate**: Zerologon is a known vulnerability — ensure DCs are patched and machine account passwords are rotated.

---

## 9) Helpful Wireshark tips for this case
- Use `Follow > TCP Stream` on suspicious SMB or RPC sessions to see the full conversation context.  
- Use `Statistics > Conversation` to find heavy talkers and timeline ordering.  
- Mark important packets (right-click → Mark Packet) to build a clear timeline for your report.

---

## 10) Final notes (ethics & learning)
- Always have permission before capturing or testing networks.  
- For more practice, check official Wireshark sample captures and DFIR challenge PCAPs (like DFIR Madness).  
- Use this PCAP analysis to learn how exploits leave network traces — that helps during hunting and response.




