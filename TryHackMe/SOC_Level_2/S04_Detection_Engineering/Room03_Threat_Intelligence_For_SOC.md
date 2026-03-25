# 🌐 Threat Intelligence

---

## 🧠 What is Threat Intelligence?

Threat Intelligence means:

> **Collecting data about attacks + understanding it + using it to stop attacks**

Example:

* Raw data: `185.224.128.215`
* Intelligence: "This IP is used by attackers"

👉 So, intelligence = **data + meaning**

---

## 🎯 Why SOC teams use it?

SOC team uses Threat Intelligence to:

* Detect attacks early
* Block attackers
* Investigate incidents faster

---

## 📊 Types of Threat Intelligence (Easy)

| Type        | Simple Meaning     | Example                          |
| ----------- | ------------------ | -------------------------------- |
| Strategic   | Big picture        | "Banking sector is under attack" |
| Technical ⭐ | Actual data (IOCs) | IPs, domains, hashes             |
| Tactical    | Attack methods     | Phishing, malware steps          |
| Operational | Attacker goal      | Why attack is happening          |

👉 In SOC, we mostly use **Technical Intelligence**

---

## 🔍 What are IOCs?

IOC = Indicator of Compromise

👉 These are signs of attack:

* IP address
* Domain
* File hash
* URL

Example:
185.224.128.215 → suspicious IP

---

## 🏭 Producer vs Consumer

### 🏭 Producer (Creates Intelligence)

* Finds new attacks
* Analyzes malware
* Shares reports

Example:
Security companies

---

### 🧑‍💻 Consumer (Uses Intelligence) ⭐

* Uses IOCs
* Adds them in SIEM
* Detects attacks

👉 SOC analyst = **Consumer**

---

## ⚙️ How SOC Uses Threat Intelligence

### 1. Detection

* Add IOC in SIEM
* Create alerts

---

### 2. Prevention

* Block malicious IPs

---

### 3. Investigation

* Check logs
* Confirm attack

---

## 🛠️ SOC Workflow (Very Important)

### Step 1: Get IOC list

Example:
135.181.103.89
185.224.128.215

---

### Step 2: Convert into query

Tool: Uncoder.io

Output example:
source.ip: ("135.181.103.89" OR "185.224.128.215")

---

### Step 3: Search in SIEM (Kibana)

* Index: filebeat-*
* Time range: correct dates

---

### Step 4: Analyze logs

Check:

* source.ip → internal machine
* destination.ip → attacker

---

## 🚨 How to Find Compromised Host

Follow this logic:

1. Filter logs using IOC
2. Look at **source.ip**
3. Find IP that appears many times
4. That IP = compromised system

---

## 🧠 Easy Example

| source.ip | destination.ip  |
| --------- | --------------- |
| 10.0.0.5  | 185.224.128.215 |
| 10.0.0.5  | 135.181.103.89  |
| 10.0.0.8  | 185.224.128.215 |

👉 10.0.0.5 appears more → compromised

---

## ⚠️ Important Points

* High traffic IP ≠ compromised
* IOC match is important
* Always check context
* Look for repeated behavior

---

## 🔥 Quick Revision

* Threat Intelligence = data + analysis
* IOC = sign of attack
* SOC = Consumer
* source.ip = internal machine
* destination.ip = attacker
* Repeated source.ip = compromised

---

## 🎯 Final Tip

Always think like this:

> "Which internal system is talking to bad IP again and again?"

👉 That is your answer

---
# 🌐 Threat Intelligence – Prevention & Hunting 

---

## 🧠 Goal

Your organisation is a **consumer**.

So your job is:

> **Take IOCs → Apply controls → Stop attacks → Hunt activity**

---

## 📌 Types of IOCs (Easy)

| IOC Type | Meaning                | Example         |
| -------- | ---------------------- | --------------- |
| Domain   | Website used in attack | agrosaoxe.info  |
| IP       | Attacker machine       | 185.224.128.215 |

---

# 🔥 PART 1: Prevention (Blocking Attacks)

---

## 🚧 1. IP Blocking (Firewall)

👉 What it does:

* Blocks traffic from/to bad IP

### Example:

Block:
185.224.128.215

👉 Result:

* Attacker cannot connect
* Malware cannot call attacker

---

## 📧 2. Domain Blocking (Email Gateway)

👉 What it does:

* Blocks emails from bad domains

### Example:

Block:
badmail.com

👉 Result:

* Phishing email never reaches user

---

## 🌐 3. DNS Sinkhole (IMPORTANT ⭐)

👉 What it does:

* Stops connection to bad domain

### Simple idea:

Normal:
user → bad domain → attacker IP ❌

Sinkhole:
user → bad domain → fake IP (sinkhole) ✅

👉 So attacker connection fails

---

# 🔍 PART 2: Hunting Sinkholed Domains

---

## 🎯 Your Task

Find logs where this domain was used:

agrosaoxe.info

---

## ⚙️ Step-by-Step (Kibana)

---

### ✅ Step 1: Open Discover

* Index: filebeat-*
* Time: Feb 14 → Feb 17

---

### ✅ Step 2: Search Domain

Use this query:

dns.question.name: "agrosaoxe.info"

👉 This shows:

* Who tried to access this domain

---

### ✅ Step 3: Find Source IP

👉 Add field:
source.ip

Now check:

* Which internal system is making request

---

## 🔥 Step 4: Check Sinkhole IP

Now use second query:

dns.answers.data: "SINKHOLE_IP"

👉 Replace with actual sinkhole IP from logs

---

## 🧠 How to Identify Sinkhole IP?

👉 Look at results from Step 2

You will see:

* Same IP repeated in answers

Example:
dns.answers.data: 10.10.10.10

👉 That = sinkhole IP

---

## 🔁 Step 5: Confirm Activity

Now:

* Find all logs hitting sinkhole IP
* Check source.ip again

---

# 🚨 Final Logic (Very Important)

👉 If system tries to connect to malicious domain:

That system = suspicious / compromised

---

## 🧠 Easy Example

| source.ip | domain         | answer IP   |
| --------- | -------------- | ----------- |
| 10.0.0.5  | agrosaoxe.info | 10.10.10.10 |
| 10.0.0.5  | agrosaoxe.info | 10.10.10.10 |

👉 Same system repeating → compromised

---

# ⚠️ Important Points

* Remove [.] → use normal domain
* Always check source.ip
* Repeated behavior = strong indicator
* Sinkhole IP is fake (defense system)

---

# 🔥 Quick Revision

* IOC = bad indicator
* Firewall → blocks IP
* Email Gateway → blocks domain emails
* DNS Sinkhole → redirects bad domain
* source.ip = internal machine
* Repeated domain requests = compromised

---

# 🎯 Final Tip

Always ask:

> "Which system is trying to reach a bad domain again and again?"

👉 That is your answer

---
# 🌐 Detection using Threat Intelligence 

---

## 🧠 Goal

> Use IOCs not just to block attacks, but also to **detect suspicious activity**

---

## 🔥 Prevention vs Detection

| Type       | Meaning                          |
| ---------- | -------------------------------- |
| Prevention | Stop attack before it happens    |
| Detection  | Find attack when it is happening |

---

## 📊 IOC-based Detection (Simple)

### 1. IP Address

* Connection to bad IP = suspicious

| Direction                    | Meaning               |
| ---------------------------- | --------------------- |
| Outbound (internal → bad IP) | Malware inside system |
| Inbound (bad IP → internal)  | Attack attempt        |

---

### 2. URL (Proxy Logs)

| Method | Meaning                         |
| ------ | ------------------------------- |
| GET    | Download or open malicious file |
| POST   | Data theft or credentials sent  |

---

### 3. Domain (DNS Logs)

* If system connects to bad domain:

  * Malware download
  * Phishing
  * C2 communication

---

# 🛡️ Detection using Prevention Tools

---

## 🌐 DNS Sinkhole ⭐

* Bad domain → resolves to `0.0.0.0` or `127.0.0.1`

👉 Meaning:

* Domain is blocked
* System still trying to access it

💥 Suspicious activity

---

## 🔥 Firewall Blocking

* Logs show blocked IP connection

👉 Meaning:

* System tried to connect to attacker

---

## 🌐 Proxy Blocking

* Blocked website access

👉 Meaning:

* Attempt to open malicious site

---

## 📧 Mail Gateway Blocking

* Blocked email

👉 Meaning:

* Phishing attempt

---

# 🧠 Key Detection Logic

> Blocked activity = still important signal

👉 Even if attack is blocked:

* System behavior is suspicious
* Needs investigation

---

# ⚙️ Sigma Rule (Easy)

```yaml
dns.resolved_ip: '0.0.0.0'
```

👉 Meaning:

> Find all DNS queries where result is 0.0.0.0

---

## 🔍 Why important?

* Shows sinkhole activity
* Indicates connection to malicious domain

---

# 🛠️ ElastAlert (Simple)

👉 Tool that:

* Reads logs
* Applies rule
* Generates alert

---

## 📦 Rule Structure (Easy)

```yaml
alert:
- debug
```

→ show alert

---

```yaml
filter:
  query: dns.resolved_ip:"0.0.0.0"
```

→ detection logic

---

```yaml
index: filebeat-*
```

→ where logs are stored

---

```yaml
type: any
```

→ alert on every match

---

# 🔍 Workflow (SOC Process)

1. Create detection rule (Sigma)
2. Convert to SIEM format
3. Run in ElastAlert
4. Generate alerts
5. Analyze output

---

# 📂 Output Analysis

File:
output.txt

Check:

* source.ip → internal system
* domain → malicious domain
* count → repeated behavior

---

# 🚨 Detection Logic (Important)

If:

* DNS resolves to 0.0.0.0
* Same system repeats requests

👉 System is likely compromised

---

# 🧠 Example

| source.ip | domain  | resolved IP |
| --------- | ------- | ----------- |
| 10.0.0.5  | bad.com | 0.0.0.0     |
| 10.0.0.5  | bad.com | 0.0.0.0     |

👉 Repeated → compromised

---

# ⚠️ Important Points

* Detection ≠ prevention
* Blocked logs still useful
* Always check repetition
* Context is important

---

# 🔥 Quick Revision

* IOC = indicator of attack
* Detection = finding attack
* Sinkhole = fake IP (0.0.0.0)
* Sigma = detection rule
* ElastAlert = alert tool
* output.txt = results

---

# 🎯 Final Tip

> "If a system tries to access a blocked domain, it is suspicious"

---
