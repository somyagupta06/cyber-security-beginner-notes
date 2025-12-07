# 🌐 Port Forwarding & Firewalls 

# 🔵 What is Port Forwarding?

Port forwarding allows **inside services** (like a web server in your home network)  
to be accessed from the **outside world (Internet)**.

Without port forwarding:
- Only devices **inside the same network** can access the service.

Example:
- Server: `192.168.1.10`
- Web server running on port **80**
- Only local devices can open the website → *intranet*
<img width="701" height="739" alt="Screenshot 2025-12-07 at 5 33 54 PM" src="https://github.com/user-attachments/assets/5408d00b-03d6-431d-a5e6-a6b0daeadb6d" />

---

# 🌍 Why Do We Need Port Forwarding?

If we want people on the **Internet** to access that server,  
we must forward a **public port** on the router → to the server inside the network.

Example:
- Public IP: `82.62.51.70`
- Port forwarding enabled on port **80**
- Now outside users can type:

http://82.62.51.70

and the router will forward traffic to:

192.168.1.10:80
<img width="1222" height="547" alt="Screenshot 2025-12-07 at 5 34 07 PM" src="https://github.com/user-attachments/assets/85353b82-3ee8-4f52-b343-b47faeaded0b" />

---

# 🧠 Important Difference: Port Forwarding vs Firewall

### 🔵 Port Forwarding
- **Opens a port**  
- Allows outside traffic to reach a specific device  
- Configured on the **router**

### 🔥 Firewall
- Decides whether the traffic **should be allowed or blocked**  
- Uses rules to check:
  - Source IP  
  - Destination IP  
  - Port number  
  - Protocol (TCP/UDP)

Think of it like:
- **Port Forwarding = Door is open**
- **Firewall = Security guard checking who can enter**

---

# 🛂 What is a Firewall?

A firewall is like **border security** for your network.

It checks:
1. **Where traffic is coming from?**  
2. **Where it is going?**  
3. **Which port is it using?**  
4. **Which protocol (TCP/UDP)?**

It uses **packet inspection** to decide:
- Allow  
- Deny  

Firewalls can be:
- Hardware (big business firewalls)  
- Software (Snort, home router firewalls)  
- Built-in OS firewalls  

---

# 🟩 Types of Firewalls

We mainly focus on **two categories**:

---

# 🟦 Stateful Firewall

### How it works:
- Looks at the **entire connection**, not just single packets
- Makes dynamic decisions
- Understands TCP handshake and connection behavior

### Pros:
- Very **smart**
- Good for detecting suspicious or broken connections

### Cons:
- Uses **more system resources**
- If one connection from a device looks bad → **entire device may be blocked**

### Example:
Allows SYN and SYN/ACK but blocks if the handshake fails later.

---

# 🟥 Stateless Firewall

### How it works:
- Uses **fixed rules**  
- Only checks *individual packets*  
- Does **not** understand connection behavior

### Pros:
- Very fast  
- Uses fewer resources  
- Good for massive traffic (DDoS protection)

### Cons:
- Dumb → only follows exact rules  
- If rules are not perfect, the firewall is useless  
- Cannot detect bad connection behavior  

### Example:
If a packet is bad, it just drops that one packet —  
but still allows the device to continue sending more.

---

# 🎯 Summary

| Concept | Meaning |
|--------|---------|
| **Port Forwarding** | Opens a port on router → sends traffic to internal device |
| **Firewall** | Decides what traffic is allowed or blocked |
| **Stateful Firewall** | Tracks whole connection, smart but heavy |
| **Stateless Firewall** | Uses simple rules, fast but dumb |

---
# 🔒 Virtual Private Network (VPN) – Super Simple Notes

A **VPN** creates a **secure tunnel** between devices across the Internet.  
Even if two devices are in different cities/countries, a VPN lets them behave  
as if they are on the **same private network**.

---

# 🏢 Example: Three Networks

- **Network #1** → Office 1  
- **Network #2** → Office 2  
- **Network #3** → Devices connected through the VPN  

Devices in Network #3 are still part of their own offices…  
BUT they also share a **special private encrypted network** created by the VPN.
<img width="961" height="612" alt="Screenshot 2025-12-07 at 5 34 54 PM" src="https://github.com/user-attachments/assets/b910f9f9-e679-40c3-adc2-3964c1cd15e1" />

---

# 🎯 Benefits of Using a VPN

| Benefit | Description |
|---------|-------------|
| Connects networks across the world | Businesses with many offices can share resources like servers |
| Privacy | VPN encrypts your traffic → no one can read your data |
| Security on public WiFi | Protects you when using cafes, airports, etc. |
| Anonymity | Journalists & activists use VPNs to hide identity |
| Used by TryHackMe | Lets you attack machines safely without exposing them to the Internet |

---

# 🟦 VPN Technologies

| Technology | Description |
|------------|-------------|
| **PPP** | Used for authentication & encryption. Needs a protocol (like PPTP) to leave the network. |
| **PPTP** | Easy to set up but weak encryption. Sends PPP traffic outside. |
| **IPSec** | Strong encryption. Hard to set up but very secure. |

---

# 📡 What is a Router?

A **router connects networks** and decides the best path for data.
<img width="1079" height="328" alt="Screenshot 2025-12-07 at 5 35 22 PM" src="https://github.com/user-attachments/assets/71a46345-fb08-437c-b518-e5c9749dcf0e" />

Routers work at:
- **Layer 3 (Network Layer)**  
- They use **IP addresses**  
- They choose the path based on:  
  - Shortest route  
  - Most reliable route  
  - Fastest connection (fibre > copper)

Routers **do not** do the job of switches.

They ALSO manage:
- Port forwarding  
- Firewall rules  
- Routing tables  
- Network paths  

---

# 🔌 What is a Switch?

A switch connects **multiple devices** inside the same network.

## Two types of switches:

---

# 🟩 Layer 2 (L2) Switch

- Works at **OSI Layer 2 – Data Link Layer**  
- Uses **MAC addresses**  
- Sends **frames** to the correct device  
- CANNOT understand IP addresses  
- CANNOT perform routing  

Simple view:
> L2 switch = “Who has this MAC? Send the frame to them.”

---

# 🟥 Layer 3 (L3) Switch

- Works at **Layer 2 + Layer 3**  
- Can forward **frames** (like L2)  
- Can also **route IP packets** (like a router)  
- Behaves like a router inside big networks  
- Used for communication between VLANs  

---

# 🟦 VLAN (Virtual LAN)

A VLAN allows one physical switch to behave like **multiple separate networks**.

Example:
- Sales Department = VLAN 10  
- Accounting Department = VLAN 20  

Both are connected to the **same switch**,  
BUT they **cannot communicate** with each other unless rules allow it.

Benefits of VLANs:
- Better security  
- Better organisation  
- Control over which devices can talk to each other  
- Reduces broadcast traffic  

Even though Sales and Accounting are on the same switch:
- They can use the **same Internet connection**  
- But **cannot communicate directly** → VLAN isolation  

---

# 🎯  Final Summary

| Concept | Simple Explanation |
|--------|--------------------|
| **VPN** | Creates a private, encrypted tunnel between far-away devices |
| **Router** | Connects networks, chooses best path, uses IP addresses |
| **L2 Switch** | Sends frames using MAC addresses |
| **L3 Switch** | L2 switch + routing abilities (uses IP addresses) |
| **VLAN** | Splits one switch into many virtual networks |

---

