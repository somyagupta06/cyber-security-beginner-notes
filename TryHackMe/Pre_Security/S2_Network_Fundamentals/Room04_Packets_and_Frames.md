# 📦 Packets vs Frames 

Packets and frames are both **small pieces of data**, but they belong to **different OSI layers**.

---

# 🟥 Packet (Layer 3 – Network Layer)

A **packet** contains:
- IP header (Source IP, Destination IP)
- Payload (actual data)

Packets are used when we talk about **IP addresses**.

**Layer:** Network Layer (Layer 3)

---

# 🟦 Frame (Layer 2 – Data Link Layer)

A **frame** wraps the packet and adds:
- Source MAC address  
- Destination MAC address  

Frames are used when sending data **inside a local network** (LAN).

**Layer:** Data Link Layer (Layer 2)

---

# 💌  Example – Letter and Envelope

Think of sending a letter by post:

- **Packet = The letter inside**  
- **Frame = The envelope**  

The envelope (frame) helps transport the letter (packet) to the receiver.  
When the envelope is opened, the actual message inside (packet) is delivered.

This wrapping process = **Encapsulation**.

---

# 🔄 Encapsulation Quick Recap
- Layer 2 adds MAC addresses → makes a frame  
- Layer 3 adds IP addresses → makes a packet  
- When layers remove this extra info → **decapsulation**

---

# 🧩 Why Packets Are Useful

Data is not sent as one big chunk.  
It is broken into **small packets** so the network doesn’t become slow.

Example:
A cat image on a website is sent as multiple packets (#1, #2, #3).  
Your computer puts them together to rebuild the final image.

This prevents:
- Network overload  
- Bottlenecks  
- Crashes  

---

# 🧱 Packet Structure (Important Headers)

Different packets have different formats depending on the protocol.  
Let’s look at common headers in **Internet Protocol (IP) packets**.

| Header | Description |
|--------|-------------|
| **Time to Live (TTL)** | A timer that stops packets from living forever. If it reaches 0, the packet is deleted. Prevents network clogging. |
| **Checksum** | Used for error checking. If the data is damaged or changed, the checksum will not match. |
| **Source Address** | The IP address of the sender (where the packet came from). |
| **Destination Address** | The IP address of the receiver (where the packet needs to go). |

---

# 🎯 Simple Rule to Remember

- If we talk about **IP** → we are talking about **Packets**  
- If we talk about **MAC address** → we are talking about **Frames**  

---

# 🧠 Super Simple Summary Table

| Layer | Data Unit | Contains |
|-------|-----------|----------|
| **Layer 3** | Packet | IP addresses |
| **Layer 2** | Frame | MAC addresses |

---

# 🌐 TCP/IP Model & TCP Explained (Super Simple Notes)

TCP/IP is another networking model similar to the OSI model.  
But OSI has **7 layers**, and TCP/IP has only **4 layers**:

1. Application  
2. Transport  
3. Internet  
4. Network Interface  

Data moves through these layers using **encapsulation** (adding info)  
and **decapsulation** (removing info).

---

# 🔵 What is TCP?

TCP = Transmission Control Protocol  
It is a **connection-based** protocol.

Meaning:
- Before sending data, TCP creates a **connection** between two devices.
- It guarantees data will reach safely and correctly.
- It keeps data **in order**.

This is why TCP is used for:
- Web browsing  
- Emails  
- File transfer  
(All these need perfect, complete data.)

---

# 👍 Advantages vs 👎 Disadvantages of TCP

| Advantages | Disadvantages |
|-----------|---------------|
| Guarantees correct, complete data | Needs a stable connection |
| Prevents flooding (syncs devices) | Slow connection can delay everything |
| Has many reliability checks | Slower than UDP because of extra work |

---

# 📦 TCP Packet Headers (Important Fields)

When TCP creates a packet, it adds several **headers**:

| Header | Meaning |
|--------|---------|
| **Source Port** | The port number used by sender (picked randomly). |
| **Destination Port** | The port on the receiving device running a service (e.g., port 80 for web). |
| **Source IP** | Sender's IP address. |
| **Destination IP** | Receiver's IP address. |
| **Sequence Number** | A random number that marks the first piece of data. |
| **Acknowledgement Number** | Confirms the next expected data (seq + 1). |
| **Checksum** | Error-checking to confirm data integrity. |
| **Data** | Actual content being sent. |
| **Flags** | Control how packets behave (SYN, ACK, FIN, RST). |

---

# 🤝 TCP Three-Way Handshake (Super Simple)

This is how TCP **establishes a connection**.
<img width="768" height="662" alt="Screenshot 2025-12-07 at 5 28 24 PM" src="https://github.com/user-attachments/assets/0a610078-b376-4425-a191-b3a8f503a374" />

Steps:

1️⃣ **SYN**  
Client → “I want to connect. Here is my sequence number.”

2️⃣ **SYN/ACK**  
Server → “I accept. Here is *my* sequence number, and I acknowledge yours.”

3️⃣ **ACK**  
Client → “I acknowledge your sequence number. Let’s start sending data.”

After this, **data** can be sent safely.

---

# 🔢 Sequence Numbers (Why They Matter)

Every TCP connection starts with two *random* numbers called **ISN**  
(Initial Sequence Number).

Example:

- Client ISN = 0  
- Server ISN = 5000  

Data order:

| Device | ISN | Next Sequence |
|--------|-----|----------------|
| Client | 0 | 1 |
| Client | 1 | 2 |
| Client | 2 | 3 |

The sequence increases by **1** for each part of data.  
This helps reconstruct data in the correct order.

---

# 🛑 TCP Closing a Connection (FIN + ACK)

TCP doesn’t leave connections open forever.
<img width="700" height="672" alt="Screenshot 2025-12-07 at 5 28 45 PM" src="https://github.com/user-attachments/assets/c2b0f4e0-7e20-4d99-a55f-e6b83c4d92a7" />

To close properly:

1️⃣ Device sends a **FIN** packet:  
→ “I’m finished sending data.”

2️⃣ Other device sends **ACK**:  
→ “I got your request.”

3️⃣ Other device also sends **FIN**:  
→ “I also want to close the connection.”

4️⃣ Final **ACK**  
→ “Connection closed cleanly.”

This ensures no data is left unsent.

---

# 🟥 RST Packet (Reset)

If something goes wrong:
- service not running  
- device crashes  
- low system resources  

TCP stops everything immediately using **RST**  
→ “Stop now. Something is wrong!”

---

# 🎯 Quick Summary

- TCP/IP has 4 layers (simple version of OSI).  
- TCP is **connection-based** and guarantees safe delivery.  
- Uses many headers (ports, IPs, sequence numbers, checksum).  
- Starts with a **3-way handshake** (SYN → SYN/ACK → ACK).  
- Ends with **FIN → ACK → FIN → ACK**.  
- Sequence numbers keep data in order.  

---

# 🟠 UDP (User Datagram Protocol) – Simple Notes

UDP is another protocol used to send data between devices.  
But unlike TCP, UDP is **stateless** and **connectionless**.

This means:
- No three-way handshake  
- No guarantee the data will reach  
- No synchronisation  
- No checking for errors or order  

UDP simply **sends the data and hopes for the best**.

---

# 🚀 When is UDP Used?

UDP is used when:
- Speed is more important than accuracy  
- Some data loss is acceptable  

Examples:
- Video streaming (small pixel errors don't matter)
- Voice calls (small glitches are okay)
- Online gaming
- Discovery protocols (ARP, DHCP)

---

# 👍 Advantages vs 👎 Disadvantages of UDP

| Advantages of UDP | Disadvantages of UDP |
|-------------------|----------------------|
| Much faster than TCP | Does not guarantee delivery |
| Very flexible for software developers | No error checking or recovery |
| Does not reserve a long connection | Unstable networks → poor experience |

---

# 📦 UDP Packet Headers (Much Simpler than TCP)

UDP packets have fewer headers, which is why they are fast.

| Header | Description |
|--------|-------------|
| **Time To Live (TTL)** | Stops packets from living too long on network |
| **Source Address** | Sender’s IP address |
| **Destination Address** | Receiver’s IP address |
| **Source Port** | Random port chosen by sender |
| **Destination Port** | Port of the service receiving the data |
| **Data** | The actual bytes being sent |

---

# 🔄 How UDP Communication Works

UDP is **stateless** → No need to set up a connection.

Meaning:
- No SYN  
- No SYN/ACK  
- No ACK  
- Just DATA  

It works like throwing a message over the wall:
- If the other side catches it → good  
- If not → UDP doesn’t care  

---

# 🎯 Simple Example (Alice → Bob)

Alice → sends data  
Bob → receives data (if it arrives)  

There is **no acknowledgement**, no sequence number, no handshake.

This makes UDP:
- Fast  
- Lightweight  
- But not reliable  
<img width="641" height="663" alt="Screenshot 2025-12-07 at 5 29 43 PM" src="https://github.com/user-attachments/assets/486cb467-efda-43f2-993f-1771ff218fd7" />

---

# 🧠 Quick Summary Table

| Feature | TCP | UDP |
|---------|-----|-----|
| Connection? | Yes (3-way handshake) | No |
| Guarantees delivery? | Yes | No |
| Speed | Slow | Fast |
| Error checking | Yes | Minimal |
| Order maintained? | Yes | No |
| Use cases | Web, email, files | Streaming, gaming, calls |

---

# 🚢 What Are Ports? (Simple Notes)

Ports in networking are like **harbour ports for ships**.

- A harbour = your computer  
- Ships = data packets  
- Ports = specific docking spots  

Just like a cruise ship cannot park at a fishing boat’s port,  
**data can only enter a computer through the correct port**.

Ports make sure:
- The right data goes to the right application  
- Unwanted data cannot enter the wrong place  

Ports are numbers between **0 and 65,535**.

---

# 🎯 Why Do We Use Ports?

Imagine a computer running:
- A browser  
- A game  
- A video call  
- A download  

All these need a way to receive data **without getting mixed up**.  
Ports help separate this data.

Example:
- Web browsing → port **80**  
- Secure browsing → port **443**  
- SSH login → port **22**

Each application has its own “parking spot”.

---

# 🟦 Common Ports (0–1024)

These are **standard** and used by popular network services.

| Protocol | Port | Simple Meaning |
|----------|------|----------------|
| **FTP** | 21 | Downloading files from a server |
| **SSH** | 22 | Secure text-based remote login |
| **HTTP** | 80 | Normal website browsing |
| **HTTPS** | 443 | Secure website browsing (encrypted) |
| **SMB** | 445 | Sharing files & printers on a network |
| **RDP** | 3389 | Remote Desktop (full visual login) |

These are called **well-known ports**.

---

# 📝 Important Note

Even though these ports have standard meanings,  
**you can run a service on ANY port**.

For example:
- A web server **normally runs** on port 80  
- But you can run it on port 8080  

However, users must type the port manually:

http://website.com:8080


Without the port, browsers assume default port 80.

---

# 🎯Summary

- Ports are like parking spots for data  
- Port numbers range from **0–65535**  
- Ports help multiple apps communicate at the same time  
- Ports 0–1024 = well-known standard ports  
- You *can* change default ports, but users must specify them  

---

# 🧪 Practical Challenge (Explanation)

Task:  
Connect to IP **8.8.8.8** on port **1234** using the attached site’s tool.

Steps (general idea):
1. Go to the site attached in your task.  
2. Enter IP → `8.8.8.8`  
3. Enter Port → `1234`  
4. Click connect  
5. You will receive the flag  

(This is normally done using tools like `nc` or browser consoles,  
but since the site already gives you a tool, just follow its interface.)

---

