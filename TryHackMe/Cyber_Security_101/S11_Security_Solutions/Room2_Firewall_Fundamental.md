# 🔥 Firewalls 

Think of a **security guard** standing outside a mall, bank, or restaurant.  
👉 His job = Stop unauthorized people from entering.  

Similarly, a **firewall** acts like a **digital guard** for your computer or network.  
👉 Its job = Check all **incoming** and **outgoing** data, and decide whether to allow or block it.

You tell the firewall how to work by making **rules**.  
- If data follows rules → ✅ Allowed  
- If data breaks rules → ❌ Blocked  



## 📌 Types of Firewalls

### 1. Stateless Firewall
- Works on **Layer 3 (Network)** and **Layer 4 (Transport)** of OSI model.
- **Only checks each packet individually** (doesn’t remember past packets).
- Very **fast** (good for high-speed networks).
- Problem: Forgetful ❌ → Even if some packets from a bad source are blocked, it will still check new packets from same source again.

🖼 Example:  
Imagine a guard who checks **every person separately**. He doesn’t remember who came 5 minutes ago.  



### 2. Stateful Firewall
- Also works on **Layer 3 and 4**.
- **Remembers past connections** using a **state table**.
- Smarter ✅ → If one connection is allowed, future packets from it are auto-allowed.  
- If one connection is blocked, future packets from it are auto-blocked.  

🖼 Example:  
A guard who **keeps a notebook** 📝 of who entered and who was denied.  
So if a thief was denied earlier, he won’t waste time checking him again later.  



### 3. Proxy Firewall
- Works on **Layer 7 (Application Layer)**.
- Acts like a **middleman** between you and the internet.  
- Inspects **contents of packets** (not just header).  
- Can apply **content filtering** (e.g., block adult sites, block social media, etc.).  
- Provides **anonymity** → Hides your real IP.  

🖼 Example:  
Instead of you talking directly to a stranger, a **friend talks on your behalf**, checks their words, and then decides if they can talk to you or not.  



### 4. Next-Generation Firewall (NGFW)
- Works on **Layer 3 to Layer 7** (Network → Application).  
- Most **advanced** firewall today.  
- Features:  
  - **Deep Packet Inspection** → Looks inside data fully.  
  - **Intrusion Prevention System (IPS)** → Blocks attacks in real-time.  
  - **Heuristic Analysis** → Detects new attack patterns automatically.  
  - **SSL/TLS Decryption** → Even checks encrypted traffic.  
  - Uses **Threat Intelligence Feeds** → Knows latest hacker tricks.  

🖼 Example:  
A guard who not only checks people but also uses **CCTV, fingerprint scanner, and police database** to identify criminals instantly.  



## 📊 Firewall Comparison Table

| Firewall Type        | OSI Layer        | Memory of Connections | Can Inspect Content? | Speed  | Best Use Case |
|----------------------|------------------|-----------------------|----------------------|--------|---------------|
| **Stateless**        | Layer 3, 4       | ❌ No                 | ❌ No                | ⚡ Fast | High-speed simple networks |
| **Stateful**         | Layer 3, 4       | ✅ Yes (State table)  | ❌ No                | ⚡ Fast but a bit slower | General networks needing security |
| **Proxy**            | Layer 7          | ✅ Yes                | ✅ Yes (Content)     | 🐢 Slower | Companies, content filtering |
| **Next-Gen (NGFW)**  | Layer 3–7        | ✅ Yes                | ✅ Yes + Advanced    | ⚖ Balanced | Enterprises, modern attacks |



## 🎯 Quick Recap
- **Stateless** = Fast but forgetful.  
- **Stateful** = Remembers connections.  
- **Proxy** = Middleman + content filtering.  
- **NGFW** = All-in-one smart guard 🚀.  

---

# 🚦 Firewall Rules 

A **firewall rule** = Instructions you give to your firewall to control traffic.  
👉 Rules decide **which data is allowed, blocked, or forwarded**.



## 🧩 Basic Components of a Firewall Rule

1. **Source Address** → The IP address from where traffic starts.  
   (Who is sending data?)

2. **Destination Address** → The IP address where traffic is going.  
   (Who will receive data?)

3. **Port** → The specific port number used.  
   (e.g., Port 80 = HTTP, Port 22 = SSH)

4. **Protocol** → The communication type (TCP, UDP, etc.).

5. **Action** → What to do with the traffic.  
   - ✅ Allow  
   - ❌ Deny  
   - 🔀 Forward

6. **Direction** → Rule applies to:  
   - **Inbound** (coming into network)  
   - **Outbound** (going out from network)  
   - **Forward** (redirecting traffic)



## 🎯 Types of Actions

### ✅ 1. Allow
Lets the traffic pass.

**Example:** Allow all outgoing HTTP traffic (port 80).  

| Action | Source        | Destination | Protocol | Port | Direction |
|--------|--------------|-------------|----------|------|-----------|
| Allow  | 192.168.1.0/24 | Any         | TCP      | 80   | Outbound  |



### ❌ 2. Deny
Blocks the traffic (useful for security).

**Example:** Deny all incoming SSH traffic (port 22).  

| Action | Source | Destination      | Protocol | Port | Direction |
|--------|--------|-----------------|----------|------|-----------|
| Deny   | Any    | 192.168.1.0/24  | TCP      | 22   | Inbound   |



### 🔀 3. Forward
Redirects traffic to another system inside the network.

**Example:** Forward incoming HTTP traffic to internal web server (192.168.1.8).  

| Action  | Source | Destination  | Protocol | Port | Direction |
|---------|--------|--------------|----------|------|-----------|
| Forward | Any    | 192.168.1.8  | TCP      | 80   | Inbound   |



## ↔️ Directionality of Rules

### 📥 Inbound Rules
- Apply to **incoming traffic** only.  
- Example: Allow incoming HTTP (port 80) to web server.

### 📤 Outbound Rules
- Apply to **outgoing traffic** only.  
- Example: Block outgoing SMTP (port 25) except from mail server.

### 🔁 Forward Rules
- Apply to **traffic redirection** inside the network.  
- Example: Forward all HTTP (port 80) traffic to internal web server.



## 🚀 Quick Recap
- Firewall rules = **Custom traffic control instructions**.  
- Every rule has: **Source, Destination, Port, Protocol, Action, Direction**.  
- Actions = **Allow, Deny, Forward**.  
- Directions = **Inbound, Outbound, Forward**.  
---


# 🛡 Windows Defender Firewall (WDF)

## 📌 What is Windows Defender Firewall?
- A **built-in firewall** by Microsoft in Windows OS.
- Lets you **allow or block apps/programs** and also create **custom rules**.
- Controls **incoming** and **outgoing** network traffic.

You can open it by typing **"Windows Defender Firewall"** in Windows search.



## 🏠 Firewall Dashboard (Main Page)

The dashboard shows:
1. **Network Profiles**  
2. Options to **Allow/Disallow apps**  
3. Option to **Turn On/Off firewall**  
4. Option to **Restore Defaults**  
<img width="1117" height="294" alt="Screenshot 2025-09-08 at 10 02 48 AM" src="https://github.com/user-attachments/assets/f2af1cd3-0f00-4c00-a50f-ea0747d4a596" />



## 🌐 Network Profiles

Windows decides profile using **Network Location Awareness (NLA)**.

- **Private Network** 🏡  
  - Example: Home Wi-Fi.  
  - Safer, so firewall is less strict.  

- **Guest/Public Network** ☕  
  - Example: Coffee shop Wi-Fi.  
  - More strict, can block all incoming connections.  

👉 Different firewall settings can be applied to each profile.



## ⚙️ Options on Dashboard

- **Allow an app through firewall** → Select which apps can communicate on Private/Public networks.  
- **Turn Windows Defender Firewall on/off** → Default = ON ✅ (recommended).  
- **Block all incoming connections** → Safer than turning firewall OFF.  
- **Restore Defaults** → Reset firewall settings anytime.
<img width="641" height="184" alt="Screenshot 2025-09-08 at 10 03 07 AM" src="https://github.com/user-attachments/assets/a604c936-4ca9-48b7-8c45-379de1e2a9d2" />



## 🎯 Custom Rules

Windows Defender lets you create **custom rules** (inbound/outbound).  

### Example Task: Block all outgoing HTTP/HTTPS traffic (Port 80, 443)

👉 This means you cannot browse websites.



### 📝 Steps to Create Outbound Rule

1. Open **Windows Defender Firewall** → click **Advanced Settings**.
2. In left panel, select **Outbound Rules**.
3. Click **New Rule** (on right side).
4. Select **Custom Rule** → Next.
5. Select **All Programs** → Next.
6. Choose **Protocol = TCP**.  
   - Local Port → Default.  
   - Remote Port → Select **Specific Ports** → enter:  
     ```
     80,443
     ```
     (No spaces, separate by commas).
7. Scope → Keep default → Next.
8. Action → Select **Block the connection** → Next.
9. Profile → Check all (Domain, Private, Public) → Next.
10. Name your rule (e.g., *Block Web Browsing*) → Finish.



## 🧪 Testing the Rule

1. Before rule → Try visiting:  
http://10.10.10.10/
✅ Website opens.

2. After rule → Try visiting again.  
❌ Error: Page cannot be reached.  
→ Rule is working successfully.



## 🚀 Quick Recap
- **Windows Defender Firewall** = Microsoft’s built-in firewall.  
- **Profiles** = Private (home) vs Public (untrusted).  
- **Options** = Allow apps, block incoming connections, restore defaults.  
- **Custom Rules** = Define traffic control using:  
- Direction (Inbound / Outbound)  
- Action (Allow, Deny, Forward)  
- Protocols, Ports, IPs.  
- Example rule → Block HTTP/HTTPS (80, 443) stops browsing.  
---

# 🐧 Linux Firewalls (Made Easy)

## 🔑 Core Concept
In Linux, the **firewall functionalities** are handled by a framework called **Netfilter**.  
- It provides **packet filtering, NAT (Network Address Translation), and connection tracking**.  
- Different firewall utilities in Linux are **built on top of Netfilter**.  

---

## 🔧 Common Linux Firewall Utilities

1. **iptables**
   - Most widely used traditional firewall utility.
   - Provides powerful rule management but uses complex syntax.

2. **nftables**
   - Successor of iptables (newer, cleaner).
   - More efficient packet filtering + NAT features.

3. **firewalld**
   - Uses Netfilter under the hood.
   - Comes with **predefined zones** (trusted, public, home, etc.).
   - Easier for system administrators.

4. **ufw (Uncomplicated Firewall)**
   - Beginner-friendly interface for iptables.
   - Simplifies commands for common firewall tasks.
   - Recommended for **Ubuntu/Debian users**.

---

## ⚡ Using UFW (Uncomplicated Firewall)

### 🔍 Check Status
```bash
sudo ufw status
```
Output (if inactive):
```
Status: inactive
```

## 🚀 Enable / Disable Firewall
```
sudo ufw enable
sudo ufw disable
```
- enable → Turns firewall ON (and auto-starts on boot).
- disable → Turns firewall OFF.

## 🌐 Default Policies
Allow all outgoing traffic:
```
sudo ufw default allow outgoing
```
👉 Means: All traffic leaving your system is allowed unless specified otherwise.
- Block all incoming traffic (optional safer setup):
```
sudo ufw default deny incoming
```
## ⛔ Deny Incoming SSH Traffic
Block SSH on port 22/tcp:
```
sudo ufw deny 22/tcp
```
Output:
```
Rule added
Rule added (v6)
```
## 📜 List All Rules
```
sudo ufw status numbered
```
Example Output:
```
To                         Action      From
--                         ------      ----
[ 1] 22/tcp                 DENY IN     Anywhere
[ 2] 22/tcp (v6)            DENY IN     Anywhere (v6)
```
## 🗑 Delete a Rule
Delete rule #2:
```
sudo ufw delete 2
```
Output:
```
Deleting:
 deny 22/tcp
Proceed with operation (y|n)? y
Rule deleted (v6)
```
## 📊 Comparison (Linux Firewall Utilities)
Utility|	Based On	|Complexity|	Best For
|-----|-----|-----|----|
iptables|	Netfilter|	🔴 Hard	|Advanced admins
nftables|	Netfilter|	🟡 Medium|	Modern Linux setups
firewalld	|Netfilter|	🟡 Medium	|Servers with zones
ufw|	Netfilter|	🟢 Easy	|Beginners, Ubuntu/Debian
## 🚀 Quick Recap
- Netfilter = Core Linux firewall engine.
- iptables/nftables = Advanced, rule-heavy.
- firewalld = Zone-based management.
- ufw = Simplest interface (best for most users).

👉 With UFW you can:
- Enable/disable firewall.
- Set default rules (allow/deny).
- Allow/deny specific ports (like 22/tcp for SSH).
- List or delete rules easily.


