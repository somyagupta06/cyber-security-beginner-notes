# 🌐 What is DNS? 

DNS = **Domain Name System**

It is like the **phonebook of the Internet**.

Computers communicate using **IP addresses** like:
104.26.10.229

But humans find numbers hard to remember, so DNS allows us to use **names** instead, like:
tryhackme.com

DNS converts:
Domain Name → IP Address

Just like your phone converts:
Saved Contact Name → Phone Number

---

# 🏠 Why DNS is Important?

- Easy for humans  
- Computers still get the correct IP  
- Allows the internet to stay organised  

Imagine remembering thousands of IPs — impossible!  
DNS solves that.

---

# 🌲 DNS Domain Hierarchy

DNS works in a **tree structure**:

Root (.)
└── TLD (.com, .org, .net, .in, .uk)
└── Second-Level Domain (tryhackme)
└── Subdomain (admin, blog, www)
<img width="766" height="505" alt="Screenshot 2025-12-07 at 5 40 46 PM" src="https://github.com/user-attachments/assets/557ca9ca-3cc5-4911-902d-683dcc508615" />

---

# 🟣 TLD (Top-Level Domain)

This is the **rightmost** part of a domain name.

Examples:
.com
.org
.edu
.gov
.in
.co.uk

### Types of TLD:
1. **gTLD** (Generic)  
   - .com, .org, .edu, .gov, .net  
   - New ones: .online, .club, .website, .biz etc.

2. **ccTLD** (Country Code)  
   - .in (India)  
   - .ca (Canada)  
   - .co.uk (United Kingdom)  
   - .au (Australia)  

---

# 🔵 Second-Level Domain

This is the name **just before** the TLD.

Example:
tryhackme.com

- TLD → `.com`
- Second-Level Domain → `tryhackme`

### Rules:
- Max **63 characters**
- Only **a–z, 0–9, hyphens**
- Cannot start/end with a hyphen  
- No double hyphens `--`

---

# 🟡 Subdomain

A subdomain appears **before** the Second-Level Domain.

Example:
admin.tryhackme.com

Here:
- Subdomain → `admin`
- Second-Level Domain → `tryhackme`
- TLD → `.com`

You can add **multiple subdomains**:
jupiter.servers.tryhackme.com

### Subdomain Rules:
- Max **63 characters** each
- Only a–z, 0–9, hyphens  
- Whole domain name must be ≤ **253 characters**

There is **no limit** to how many subdomains you create.

Examples:
- blog.google.com  
- mail.yahoo.com  
- api.facebook.com  

---

# 🎯 Summary Table

| Term | Meaning | Example |
|------|---------|---------|
| **DNS** | Converts domain names to IPs | tryhackme.com → 104.26.10.229 |
| **TLD** | Last part of domain | .com |
| **Second-Level Domain** | Name before TLD | tryhackme |
| **Subdomain** | Part before SLD | admin.tryhackme.com |

---
# 📘 DNS Record Types (Simple Notes)

DNS is not only used to find IPs for websites.  
Different DNS records give different types of information.

Here are the most common DNS record types you will see:

---

# 🟦 1. A Record
- Resolves a domain name → **IPv4 address**
- IPv4 looks like: `104.26.10.229`

Example:
tryhackme.com → 104.26.10.229

---

# 🟩 2. AAAA Record
- Resolves a domain name → **IPv6 address**
- IPv6 looks like: `2606:4700:20::681a:be5`

Example:
google.com → 2607:f8b0:4004:815::200e

---

# 🟪 3. CNAME Record (Canonical Name)
- Resolves a domain → **another domain**
- NOT an IP directly

Example:
store.tryhackme.com → shops.shopify.com
After this, another DNS lookup happens for:
shops.shopify.com → IP address

Think of CNAME like a **redirect**.

---

# 🟥 4. MX Record (Mail Exchange)
- Used for **email servers**
- Tells which server handles emails for that domain

Example:
tryhackme.com → alt1.aspmx.l.google.com

MX records include a **priority number**:

- Lower number = higher priority  
- If the main server is down, email goes to backup servers

---

# 🟨 5. TXT Record (Text Record)
TXT records can store **any text**.

Common uses:
- Prove domain ownership for services  
- List allowed email servers (helps fight spam)  
- Security configurations (SPF, DKIM, DMARC)

Example:
"v=spf1 include:_spf.google.com ~all"

---

# 🎯  Summary Table

| Record | Purpose | Example |
|--------|---------|---------|
| **A** | Domain → IPv4 | 104.26.10.229 |
| **AAAA** | Domain → IPv6 | 2606:4700:20::681a:be5 |
| **CNAME** | Domain → another domain | store → shops.shopify.com |
| **MX** | Email server | alt1.aspmx.l.google.com |
| **TXT** | Free text (security, ownership, email rules) | SPF, DKIM |

---
# 🌐 What Happens When You Make a DNS Request?

When you type a website name like:
www.tryhackme.com
your computer needs the **IP address**.  
To find it, DNS follows a full step-by-step journey.

Let’s break it down simply.
<img width="619" height="622" alt="Screenshot 2025-12-07 at 5 41 40 PM" src="https://github.com/user-attachments/assets/1c4e6c21-8c9e-40df-ba61-bb98e1d5464b" />

---

# 🟦 Step 1: Your Computer Checks Its Local DNS Cache

Before asking the internet, your device checks:
“Have I looked up this website recently?”

- If **yes** → IP is returned instantly.  
- If **no** → It asks a **Recursive DNS Server**.

---

# 🟩 Step 2: Recursive DNS Server (Usually your ISP)

This server also checks *its own cache*.

- If it has the answer → sends the IP back to you.  
- If not → it begins the full DNS lookup journey.

Recursive servers are used by:
- ISPs (default)
- Google (8.8.8.8)
- Cloudflare (1.1.1.1)
- OpenDNS

---

# 🟥 Step 3: The Root DNS Servers

If the recursive server doesn’t know the answer,  
it asks the **Root DNS Servers** of the internet.

The root server does NOT know the IP.  
It only knows which **TLD server** to send you to.

Example:
tryhackme.com → .com TLD

So the root server replies:
“Go to the .com TLD server.”

---

# 🟨 Step 4: TLD Server (.com, .net, .org, .in, etc.)

The TLD server stores information about:
- Who is the **authoritative nameserver** for that domain.

For tryhackme.com, TLD returns:
kip.ns.cloudflare.com
uma.ns.cloudflare.com

These are the **nameservers** for TryHackMe.

---

# 🟪 Step 5: Authoritative DNS Server

This server **stores the real DNS records** (A, AAAA, CNAME, MX, TXT, etc).

Whatever type of record you requested,  
the authoritative server sends it back to the recursive server.

Example:
tryhackme.com → 104.26.10.229

---

# 🟫 Step 6: Recursive Server Caches the Answer

The recursive DNS server saves the IP for a while.  
This time is called **TTL (Time To Live)**.

TTL is measured in seconds.

Example:
TTL: 300 seconds → cache for 5 minutes

Caching speeds up future DNS lookups.

---

# 🟧 Step 7: Your Computer Gets the Final Answer

Finally, the recursive server returns the IP address to **your computer**,  
and your browser can now connect to the website.

---

# 🎯 Super Simple Flow Summary

1. Your computer checks **local cache**  
2. If needed → asks **Recursive DNS Server**  
3. If not found → asks **Root Server**  
4. Root → sends to **TLD Server**  
5. TLD → sends to **Authoritative Nameserver**  
6. Authoritative → returns final DNS record  
7. Recursive caches it → sends back to **your computer**

---

# 🧠 Why DNS Caching Matters?

Caching reduces:
- Internet load  
- Lookup time  
- Server stress  

You don’t want to do the full process every time you open Google!

---
