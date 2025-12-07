# 🌐 LAN Topologies 

**Topology** means the *shape or design* of a network — how devices are connected to each other.

---

# ⭐ Star Topology

### 🌟 Simple Idea
All devices connect to **one central device** (like a switch).  
It looks like a star.

### ✔ Advantages
- If one device’s cable breaks, **only that device** stops.
- Very **easy to add more devices**.
- Works fast and is reliable.

### ✖ Disadvantages
- Needs **more cables**, so it is **expensive**.
- If the **central device fails**, the **whole network stops**.
- Larger networks need more maintenance.
<img width="843" height="483" alt="Screenshot 2025-12-07 at 5 03 11 PM" src="https://github.com/user-attachments/assets/eb8e967e-00d8-4a4b-a1fc-61f4c6917a44" />

---

# 🚌 Bus Topology

### 🌟 Simple Idea
All devices connect to **one long main cable** called the backbone.

### ✔ Advantages
- **Cheap**, uses less cable.
- **Easy to install**.

### ✖ Disadvantages
- All data travels on the same cable → can become **slow** or **bottlenecked**.
- **Hard to troubleshoot**, because everyone uses the same path.
- If the **main cable breaks**, the **entire network stops**.


<img width="770" height="502" alt="Screenshot 2025-12-07 at 5 03 35 PM" src="https://github.com/user-attachments/assets/5a13ab2a-15b3-4b3f-a6ba-26cf9f2017b8" />

---
# 🔁 Ring Topology

### 🌟 Simple Idea
Devices are connected in a **circle**.  
Data travels **round and round** until it reaches the correct device.

### ✔ Advantages
- Uses **less cable**.
- Easy to find problems because data moves in one direction.
- Less traffic compared to Bus topology.

### ✖ Disadvantages
- Data may take a long path → can be **slow**.
- If **one device or cable breaks**, the **whole ring stops**.
<img width="621" height="516" alt="Screenshot 2025-12-07 at 5 04 06 PM" src="https://github.com/user-attachments/assets/addef632-60f8-4780-af88-1681306a8bf1" />

---

# 🔌 What is a Switch?

A **switch** connects many devices in a network.  
It works like a **smart traffic controller**.

### 🌟 What it does:
- Has many ports (4, 8, 16, 24, 48, etc.)
- Knows **which device is on which port**
- Sends data **only to the correct device**, not to everyone

This makes the network **faster and cleaner**.

### Hub vs Switch
- **Hub:** sends data to everyone (not smart)
- **Switch:** sends data only where needed (smart)
<img width="1363" height="599" alt="Screenshot 2025-12-07 at 5 04 20 PM" src="https://github.com/user-attachments/assets/d57fd892-9927-4929-b07c-936646e49a37" />

---

# 🌍 What is a Router?

A **router connects different networks** together.

### 🌟 Simple Idea
A router chooses the **best path** for data to travel.  
Just like Google Maps chooses the best route for you.

### What routers do:
- Connect Network A to Network B
- Connect your LAN to the **internet**
- Decide the best direction for data to move
<img width="1217" height="371" alt="Screenshot 2025-12-07 at 5 04 34 PM" src="https://github.com/user-attachments/assets/21af8d6d-5fac-4df2-be45-4931079b7dec" />

---

# 🔄 Redundancy (Backup Paths)

When routers or switches are connected in more than one way:
- The network becomes **more reliable**
- If one path fails, another path still works
- Speed may become a little slow, but **the network never fully stops**

---

# 🌐 What is Subnetting? 

Networks come in many sizes — small, medium, and very big.  
**Subnetting** means dividing one big network into many small networks.  

### 🎂 Easy Example (Cake Example)
Imagine you have **one cake**.  
Your friends all want a piece, but the cake is limited.  
So you cut the cake into slices and give each person their part.

Subnetting works exactly like this:
- Big network = Cake  
- Small networks (subnets) = Cake slices  
- Network admin = The person who decides who gets which slice
<img width="1044" height="746" alt="Screenshot 2025-12-07 at 5 04 58 PM" src="https://github.com/user-attachments/assets/783bb572-e759-46cc-8dfb-8fa5290318ac" />

---

# 🏢 Real-Life Example: A Business

A company has different departments like:
- Accounting  
- Finance  
- Human Resources  

In real life, you know which department should receive which document.  
In a network, devices also need to know **where data should go**.

So admins use **subnetting** to divide the network for each department.

This helps with:
- Better organisation  
- Better security  
- Less confusion

---

# 📘 How Subnetting Works

Subnetting is done using something called a **subnet mask**.  
A subnet mask tells:
- How many hosts/devices can fit in the network  
- Where the network part is  
- Where the device part is  

### 🧩 IP Address Structure
An IP address has **4 parts (octets)**.  
Example: `192.168.1.100`
<img width="735" height="289" alt="Screenshot 2025-12-07 at 5 05 15 PM" src="https://github.com/user-attachments/assets/076beb73-5800-469a-a384-4c84d6d06b65" />

A subnet mask also has **4 octets** (like `255.255.255.0`).

---

# 🧠 Subnets Use IP Addresses in 3 Main Ways

| Type | Purpose | Simple Explanation | Example |
|------|---------|--------------------|---------|
| **Network Address** | Shows the start of the network | Every device inside this network will share this network ID | `192.168.1.0` |
| **Host Address** | Identifies a specific device | Every device gets a unique number inside the network | `192.168.1.100` |
| **Default Gateway** | Sends data outside the network | If data needs to go to another network, it is sent to the gateway | `192.168.1.254` |

### 👉 Small Explanation
- **Network Address**: Like the colony name  
- **Host Address**: Your house number  
- **Default Gateway**: The main gate of the colony (everything outside goes through this)

---

# 🏠 Small Networks (Like Home)

At home, you usually have:
- 1 WiFi router  
- Limited devices (phones, laptops)

So you **don’t need many subnets**.  
One subnet is enough because you rarely have more than 254 devices.

---

# 🏢 Large Networks (Like Offices)

Businesses have:
- PCs  
- Printers  
- Cameras  
- Sensors  

They have **many devices**, so subnetting is needed to keep things neat and controlled.

---

# 🎯 Why Subnetting is Useful?

Subnetting gives:
1. **Efficiency** – Network becomes faster and cleaner  
2. **Security** – One department can’t easily access another  
3. **Control** – Admins can manage traffic better  

---

# 🧋 Example: A Café Network

A café usually has **two networks**:

1. **Employee Network**  
   - Cash register  
   - Staff devices  
   - Business systems  

2. **Public WiFi**  
   - For customers  

Why two networks?  
→ Because employees need secure access, and customers should not reach sensitive devices.

This separation is done through **subnetting**.

---

# 🔌 What is ARP? 

Devices on a network have **two main identifiers**:
1. **IP Address** → like your home address  
2. **MAC Address** → like your unique fingerprint  

**ARP (Address Resolution Protocol)** helps match these two together.  
ARP basically tells the network:  
👉 "Which MAC address belongs to this IP address?"

Every device keeps a small list (a log) of MAC addresses of other devices.  
This list is called the **ARP Cache**.

---

# 🧠 Why ARP is Needed?

When one device wants to talk to another device:
- It knows the **IP address**, but
- It needs the **MAC address** to actually send data

ARP helps find that MAC address.

---

# 🔍 How ARP Works
Every device has a **cache** — a small memory where it stores known IP–MAC pairs.

ARP uses **two types of messages**:
<img width="704" height="640" alt="Screenshot 2025-12-07 at 5 05 51 PM" src="https://github.com/user-attachments/assets/5502d285-5bc5-42e0-9022-fe1df8e3e379" />

## 1️⃣ ARP Request
This is a **broadcast message** sent to everyone on the network:
> “Who has this IP address? Please send me your MAC address!”

Everyone receives this request, but **only the device whose IP matches** will answer.

## 2️⃣ ARP Reply
The device that owns that IP will reply:
> “This is my MAC address.”

After receiving the reply, the asking device:
- Saves the IP–MAC pair  
- Stores it in the **ARP Cache**  
- Uses it for future communication  

---

# 🎯 Example 

Device A wants to talk to Device B.

1. Device A knows B’s **IP**, but not B’s **MAC**.  
2. Device A sends ARP Request:  
   “Who has IP 192.168.1.5?”  
3. Everyone hears it, but only Device B responds.  
4. Device B sends ARP Reply:  
   “I have that IP. My MAC is 00:AB:32:11:22:FF.”  
5. Device A saves this in its ARP Cache.

Now Device A can talk to Device B easily.

---

# 🗂 ARP Cache

The ARP Cache stores:
- IP address  
- MAC address  
- How long this entry is valid  

Devices don’t need to send ARP Requests again and again — they just check the cache.

---

# 🌐 DHCP (Dynamic Host Configuration Protocol)
DHCP is a system that **automatically gives IP addresses** to devices on a network.

Devices can get an IP in two ways:
1. **Manually** → You type the IP yourself  
2. **Automatically** → Using **DHCP** (most common)

---

# 🤝 How DHCP Works 

When a device connects to a network (like WiFi), it usually does **not** have an IP address.  
So it asks the network:  
👉 “Can someone give me an IP address?”

This full process happens in **4 steps**, known as **DORA**:
<img width="679" height="755" alt="Screenshot 2025-12-07 at 5 06 26 PM" src="https://github.com/user-attachments/assets/632645af-cc23-47e2-a874-3280f11496d3" />

---

# 🔄 The 4 Steps of DHCP (DORA)

## 1️⃣ DHCP Discover
The device says:  
> “Is there any DHCP server here? I need an IP!”

This is a **broadcast message** sent to everyone.

---

## 2️⃣ DHCP Offer
A DHCP server replies:  
> “Yes, I am here. You can use this IP address.”

It offers:
- An IP address  
- Subnet mask  
- Default gateway  
- DNS server  

---

## 3️⃣ DHCP Request
The device replies to the server:  
> “I want to use the IP address you offered me.”

This shows the device accepts the offer.

---

## 4️⃣ DHCP ACK (Acknowledgement)
The DHCP server says:  
> “Okay! You can now use this IP address.”

The device is now fully connected to the network with a working IP.

---

# 🎯 Summary 
- Device joins network  
- Asks for IP → Discover  
- Server offers IP → Offer  
- Device accepts → Request  
- Server confirms → ACK  
- Device starts using the IP address

---

# 💡 Why DHCP is Useful?
- No need to manually assign IPs  
- Faster connecting  
- Avoids IP conflicts  
- Easy for large networks

---

