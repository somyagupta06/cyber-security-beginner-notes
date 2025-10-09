# 🧠 Cyber Threat Intelligence (CTI) and OpenCTI 

## 🔍 What is Cyber Threat Intelligence (CTI)?
Cyber Threat Intelligence means collecting and studying data about **cyber attacks**, **hackers**, and **malware**  
to understand how to protect your system or company from them.

But handling CTI is not easy!  
Companies face problems like:
- How to **collect** threat data
- How to **analyse** that data
- How to **present** it in a useful way

So, different **platforms** are made to solve these problems — one of them is **OpenCTI**.

---

## 🚀 What is OpenCTI?

**OpenCTI** (Open Cyber Threat Intelligence) is an **open-source tool** —  
means it’s free to use and anyone can modify it.

It helps organisations to:
- **Store** cyber threat data  
- **Analyse** that data  
- **Visualise** (see relationships using graphs)
- **Present** threat info in a clear way

Basically, OpenCTI is like a “big brain” for handling all cyber threat information.

---

## 🎯 Objective of OpenCTI

OpenCTI was developed by **ANSSI** (French National Cybersecurity Agency).  
Its main goal is to make one powerful system that can handle **both technical and non-technical** info about threats.

### 👉 What it does:
- Connects small pieces of threat data and shows **relationships** between them  
  (like how a hacker group, a malware, and an attack are linked)
- Uses **MITRE ATT&CK framework** (a famous model that explains hacker techniques)
- Can **connect with other tools** like:
  - **MISP** (for threat sharing)
  - **TheHive** (for incident response)
  - **MITRE ATT&CK** (for tactics & techniques)

So, OpenCTI can become a central hub that connects all your threat tools together.

---

## 🧩 OpenCTI Data Model

OpenCTI uses something called **STIX2 (Structured Threat Information Expression)**.  
It is a **standard format** used everywhere in cyber security for sharing threat data.
<img width="1036" height="633" alt="Screenshot 2025-10-09 at 8 15 47 PM" src="https://github.com/user-attachments/assets/5f647a53-17ec-4a18-8c00-174967d6274e" />

### Think like this:
STIX2 tells us **how to write threat data** in a common language,  
so every system can **understand** and **share** the same info easily.

In OpenCTI:
- Each thing (like malware, campaign, attack, etc.) is an **entity**
- Links between them (like “malware used in campaign”) are **relationships**

This helps trace the **origin** and **connections** of all threat data.

---

## 🏗️ OpenCTI Architecture (Simple View)

OpenCTI has several main parts that work together.  
Here’s how it functions in simple terms 👇

### 🧱 Main Components:

| Component | Role / Function | Description |
|------------|----------------|--------------|
| **GraphQL API** | Main gateway | Connects users (clients) to database and message system |
| **Write Workers** | Writers | Python programs that write queries asynchronously (they handle tasks coming from message system called RabbitMQ) |
| **Connectors** | Data bridges | Python programs that bring in, enrich or export data |

---

## 🔌 OpenCTI Connectors

Connectors are like “pipes” that connect OpenCTI with the outside world.  
They help it **get data**, **enrich it**, or **send it somewhere else**.

There are 5 main types of connectors 👇

| Connector Type | What it Does | Examples |
|----------------|--------------|-----------|
| **External Input Connector** | Brings data from other tools | CVE, MISP, TheHive, MITRE |
| **Stream Connector** | Reads data stream inside OpenCTI | History, Tanium |
| **Internal Enrichment Connector** | Adds extra details to existing data | Observables enrichment |
| **Internal Import File Connector** | Takes info from uploaded files | PDFs, STIX2 Import |
| **Internal Export File Connector** | Sends info to other formats or tools | CSV, STIX2 export, PDF |

So basically:
- **External Input** → brings new info  
- **Stream** → keeps track of changes  
- **Internal Enrichment** → makes data smarter  
- **Import** → reads files  
- **Export** → creates reports or exports data

---

## 🧠 Summary 

| Topic | Summary |
|--------|----------|
| **CTI (Cyber Threat Intelligence)** | Studying hacker and threat data to protect systems |
| **OpenCTI** | Free platform to manage, analyse, and visualise CTI |
| **Developed by** | ANSSI (French cybersecurity agency) |
| **Main goal** | Connect technical + non-technical threat info |
| **Data Model** | Uses STIX2 format to structure info |
| **Framework used** | MITRE ATT&CK |
| **Integration** | Works with tools like MISP, TheHive |
| **Main Parts** | GraphQL API, Write Workers, Connectors |
| **Connector Types** | 5 types: External, Stream, Enrichment, Import, Export |

---

## 💡 In Short:
OpenCTI = A single place where all cyber threat info is collected, connected, visualised and shared  
→ So that security teams can **understand attacks better** and **respond faster** 💪

---

# ✅ Final Tip:
If someone asks “What is OpenCTI?”, just say:
> “It’s an open-source platform that helps analyse and connect cyber threat data using STIX2 and MITRE ATT&CK, and can integrate with tools like MISP and TheHive.”

---
# 💻 OpenCTI Dashboard & Interface 

## 🏠 OpenCTI Dashboard (Home Screen)
When you log in to OpenCTI, the **Dashboard** is the first screen you see.  
It shows many colourful **widgets** (small boxes) that give you a **summary** of your threat data.

### 👀 What the Dashboard shows:
- Total number of **entities** (like malware, campaigns, etc.)
- Number of **relationships** (connections between entities)
- **Reports** and **observables** (technical data) stored
- **Changes in last 24 hours** (what new data came or changed recently)

So basically, the dashboard gives a **quick health check** of all threat intelligence stored in the system.

---

## 📂 Activities & Knowledge (Left Panel Sections)

On the **left side panel**, you will find two main groups:

### 1. 🧩 Activities
- This section has all **security incidents** or **reports**.
- These reports help analysts (cyber experts) easily study attacks.
- Analysts can check “What happened?”, “Where it happened?”, and “Who was attacked?”

In short → “Activities” = Real attack stories.

### 2. 🧠 Knowledge
- This part stores **linked data** — like tools used by hackers, victims, campaigns, and threat actors.
- It connects all small pieces of data together so analysts can see the **big picture**.

In short → “Knowledge” = All background information about attacks.

---

## 📊 Analysis Tab

The **Analysis tab** is like a research lab.  
It shows all **reports**, **references**, and **notes** made by analysts.

### 🔍 What you can do here:
- View reports and their **sources**
- Add your **own investigation notes**
- Attach **external resources** (like MITRE ATT&CK reports)

Example: You can open the “Triton Software” report (from MITRE) and check every detail, or add your findings about it.

---

## ⚡ Events Tab

Events are about what’s **actually happening** in the organisation’s network.

### 👩‍💻 What analysts do here:
- Record suspicious or malicious **activities**
- Link those events with existing **threat intelligence**
- Create associations (like “this malware caused this event”)

So basically, “Events” = Real-time happenings and detections.

---

## 🧾 Observations Tab

When a cyber attack happens, many small technical clues are found —  
like **IP addresses**, **malware files**, **hashes**, or **commands used**.

These clues are called **observables**.

### 🧠 What happens in this tab:
- Shows all **technical data** found during attacks
- Helps analysts **compare** what they see in their network with known threat data
- Used for **threat hunting** and **correlation**

So, “Observations” = All the small signs that show an attack is happening or has happened.

---

## ⚔️ Threats Tab

This tab stores everything related to **attackers and attacks**.  
It’s like the “Most Wanted” section of OpenCTI 😎

### Types of Threat Info:

| Type | Description |
|------|--------------|
| **Threat Actors** | Hackers or groups who perform attacks |
| **Intrusion Sets** | Collection of TTPs (Tactics, Techniques, Procedures), malware, tools, etc., used by specific hackers |
| **Campaigns** | Series of attacks over time against selected victims, usually done by APTs or criminal groups |

Example:  
- Threat Actor → Lazarus Group  
- Intrusion Set → Tools & techniques Lazarus uses  
- Campaign → Actual operations Lazarus launched

---

## 💣 Arsenal Tab

The **Arsenal tab** is like a hacker’s toolbox 🧰  
It lists everything related to attacks — both **malicious** and **legit tools** that hackers may misuse.

### 🔍 What’s inside:

| Category | Description | Example |
|-----------|--------------|----------|
| **Malware** | List of known malware with info | 4H RAT, Trojan, etc. |
| **Attack Patterns** | The methods (TTPs) hackers use | Command-Line Interface technique |
| **Courses of Action (CoA)** | Ways to prevent attacks (from MITRE) | Security patches, firewalls |
| **Tools** | Normal network tools (sometimes used by hackers) | CMD, PowerShell |
| **Vulnerabilities** | Known system bugs and weaknesses | CVE-2023-XXXX |

So, the Arsenal tab helps analysts know:
- What tools attackers use  
- What weaknesses they exploit  
- How to stop them (using CoA)

---

## 🌍 Entities Tab

This tab lists all the **people, organisations, countries, and sectors** related to the data.

### 📌 Purpose:
- Helps link attacks to **specific targets**
- Adds extra **knowledge** (like which sector or company was attacked)
- Used for detailed **context analysis**

---

## 🧭 General Tabs Navigation (When you open an Entity)

When you open any entity (like **Cobalt Strike malware**),  
you’ll see several smaller tabs — each shows different types of info.

### 1. 🗂️ Overview Tab
- Basic info about the entity (ID, confidence level, description)
- Shows **relations** with threats, campaigns, etc.
- Shows **reports** that mention it
- Lists **external references**

Example: For Cobalt Strike, you can see how many attacks or reports mention it.

---

### 2. 🧠 Knowledge Tab
- Shows **linked information** related to that entity.
- Includes related **reports, indicators, relations, attack pattern timeline**
- On the right side, you can view details like:
  - Threats involved  
  - Attack vectors  
  - Events and observables linked to it

Basically, this tab helps you **connect all dots** about that entity.

---

### 3. 🧪 Analysis Tab
- Shows **reports** where the entity appeared.
- Helps analysts use that info for **future investigations**.

Example: You can check which reports include Cobalt Strike, and study its past attacks.

---

### 4. 🚨 Indicators Tab
- Lists all **IOCs (Indicators of Compromise)** related to that entity.
- Helps detect or block those indicators in your own network.

---

### 5. 📁 Data Tab
- Contains files uploaded or exported related to that entity.
- Can be technical (like STIX2 files) or non-technical (like PDFs, summaries).

---

### 6. 🕒 History Tab
- Tracks **all changes** made to that entity.
- Shows updates to attributes, relations, and other details.
- Very useful for **audit trails** or seeing what’s changed over time.

---

## 🧩 Summary Table

| Tab / Section | What it Shows | Why It’s Useful |
|----------------|---------------|-----------------|
| **Dashboard** | Overview of all threat data | Quick summary |
| **Activities** | Incident reports | For case studies |
| **Knowledge** | Tools, victims, actors | For context understanding |
| **Analysis** | Reports, references, notes | For research |
| **Events** | Real-time attack data | For monitoring |
| **Observations** | Technical indicators | For detection |
| **Threats** | Actors, intrusion sets, campaigns | For profiling attackers |
| **Arsenal** | Tools, malware, vulnerabilities | For understanding attack tools |
| **Entities** | Sectors, orgs, countries | For knowing who’s involved |
| **Overview** | General info about selected entity | For summary view |
| **Knowledge (tab)** | Links and timelines | For deeper connections |
| **Analysis (tab)** | Related reports | For investigation |
| **Indicators** | IOCs found | For network defense |
| **Data** | Uploaded/exported files | For reporting |
| **History** | Changes made | For tracking updates |

---

## 💬 In Short:

OpenCTI Dashboard = Your cyber war control room ⚔️  
It helps you:
- See what’s happening (Dashboard)
- Learn about attackers (Threats)
- Understand attack tools (Arsenal)
- Study detailed reports (Analysis)
- Track all data and changes (Entities & History)

Together, it turns **raw threat data** into **clear, connected intelligence** 🧠💥


