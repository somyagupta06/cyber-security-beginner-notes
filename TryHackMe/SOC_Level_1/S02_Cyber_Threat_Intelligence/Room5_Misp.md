# MISP - Malware Information Sharing Platform

## What is MISP?
- MISP is an **open-source platform** where trusted members share and receive **cyber threat information**.  
- It helps to **collect, store, and distribute** details about malware, cyber-attacks, fraud, and other security risks.  
- Information can be shared in:
  - **Closed groups** (only private members)  
  - **Semi-private groups** (limited members)  
  - **Open/Public groups** (available for everyone)  

The shared threat information can be used directly in:
- **NIDS** (Network Intrusion Detection Systems)  
- **Log Analysis tools**  
- **SIEM** (Security Information & Event Management Systems)  

---

## Why is MISP Useful? (Use Cases)
1. **Malware Reverse Engineering** → Share indicators to understand how different malware works.  
2. **Security Investigations** → Use shared data to investigate cyber-attacks.  
3. **Intelligence Analysis** → Collect details about hacker groups and their methods.  
4. **Law Enforcement** → Help police and forensic teams in cybercrime cases.  
5. **Risk Analysis** → Study new threats and check how dangerous they are.  
6. **Fraud Analysis** → Share financial indicators to detect and prevent fraud.  

---

## What does MISP Support? (Core Features)
- **IOC Database** → A large library that stores technical and non-technical details about malware, incidents, and attackers.  
- **Automatic Correlation** → MISP automatically finds links between malware indicators and events.  
- **Data Sharing** → Share data easily across different communities and MISP platforms.  
- **Import & Export** → Move data in and out of other tools (like NIDS, HIDS, OpenIOC).  
- **Event Graph** → Visual graph that shows connections between attributes and objects.  
- **API Support** → Connect MISP with your own systems for automatic data sharing.  

---

## Important Terms in MISP
| Term | Simple Meaning |
|------|----------------|
| **Events** | A collection of information about one incident (like a story of a cyber-attack). |
| **Attributes** | Small data points inside an event (like IP, domain, email). |
| **Objects** | Group of attributes combined (example: "Laptop" object with IP + MAC + Username). |
| **Object References** | Show the relationship between different objects. |
| **Sightings** | Record of when and where a specific data point was seen. |
| **Tags** | Labels (like #Malware, #Phishing) to organize data. |
| **Taxonomies** | Pre-defined classification systems to tag and organize events. |
| **Galaxies** | Knowledge base about hacker groups, malware families, etc. |
| **Indicators** | Information used to detect suspicious or malicious activity. |

---

## Example for Easy Understanding
- A hacker tries to hack your school’s server.  
- Hacker’s **IP address**: `123.45.67.89`  
- Hacker sends a **phishing email**: `fakebank@gmail.com`  
- These details are stored as **Attributes** in MISP.  
- You create an **Event** called *"School Hack Attempt"* and add these attributes inside.  
- MISP automatically checks if this IP or email was also used in other attacks.  
- If yes → MISP shows a connection.  
- Now, other schools or organizations can also get alerted about the same hacker.  

---

## In Short:
**MISP is like a "WhatsApp group for cyber experts"** 📲  
- But instead of jokes, they share **IP addresses, malware samples, and fraud details**.  
- This sharing helps everyone to stay **more secure together**.  
---
# MISP Dashboard and Event Management (Simple Notes)

## Dashboard Overview
The MISP dashboard is what an analyst sees.  
It allows you to **track, share, and connect (correlate)** events and IOCs.  

### Dashboard Menu Options
- **Home button** → Brings you back to the start page or your custom homepage.  
- **Event Actions** → Create, edit, delete, publish, search, and list events and attributes.  
- **Dashboard** → Create your own dashboard with widgets.  
- **Galaxies** → Quick access to the list of MISP Galaxies (knowledge items).  
- **Input Filters** → Rules that control what data can be entered. (Admins can block values, use regex, etc.)  
- **Global Actions** → Access info about MISP, your profile, news, terms of use, list of organisations, and their contributions.  
- **MISP** → A link to the base MISP URL.  
- **Name** → Shows the logged-in user’s name (auto-generated from email).  
- **Envelope** → User Dashboard with notifications and proposals for your organisation.  
- **Logout** → Ends your session.  

---

## Event Management
This is where analysts work with events and attributes.

### Event Actions Process
1. **Event Creation** → Start with general information about an incident (description, time, risk level).  
2. **Add Attributes/Attachments** → Add details (IPs, domains, emails, files).  
3. **Publish** → Share with your organisation/community.  

---

## Event Creation
- Events = containers for incident details.  
- When creating an event, you must also decide its **distribution level**:  
  - **Your organisation only** → Only your org can see it.  
  - **This community only** → Your org + other orgs on this MISP server + connected servers.  
  - **Connected communities** → Shared with your server + other servers synced with it (up to 2 hops away).  
  - **All communities** → Shared with all MISP communities worldwide.  
- Optionally, you can also use a **sharing group** (custom list of organisations).  

---

## Attributes & Attachments
- **Attributes** → Added manually or imported (formats like OpenIOC, ThreatConnect).  
- **Important options:**  
  - **For Intrusion Detection System (IDS)** → Mark attributes as usable in IDS signatures.  
  - **Batch Import** → Add many attributes at once (e.g., list of IPs separated by new lines).  
- **Attachments** → You can add malware samples, reports, or dropped files.  
  - If it’s malware, check the **Malware box** → File will be zipped + password protected.  

---

## Publishing Events
- After analysts create events, the **organisation admin reviews and publishes** them.  
- Published events are shared with the selected distribution channels.  

---

## Feeds
Feeds = pre-configured resources with threat indicators.  
- They provide continuously updated info about threats and attackers.  
- Feeds help analysts stay **proactive against attacks**.  

### Feeds allow you to:
- Exchange threat information.  
- Preview events + attributes.  
- Select and import events into your MISP.  
- Correlate attributes between feeds and events.  
- Feeds are enabled/managed by the Site Admin.  

---

## Taxonomies
- **Taxonomy** = classification system.  
- In MISP, taxonomies classify events, indicators, and threat actors.  
- They are expressed in **machine tags** with 3 parts:  
  1. **Namespace** → What property it belongs to.  
  2. **Predicate** → What kind of property.  
  3. **Value** → Actual detail (text/number).  

### Analysts can use Taxonomies to:
- Tag events for tools like VirusTotal.  
- Make sure events are classified properly before publishing.  
- Add meaningful tags to IDS exports.  

---

## Tagging
- Tags come from **feeds + taxonomies**.  
- Tags can be added to **events** or **attributes**.  
- Tags help analysts and organisations **quickly find and share** threat data.  
- You can also create your **own custom tags**.  

### Best Practices for Tagging
1. **Event-level vs Attribute-level**  
   - Put tags at the event level (general).  
   - Only tag attributes separately if they are exceptions.  
2. **Minimal required tags** (recommended):  
   - **Traffic Light Protocol (TLP)** → Defines how data can be shared (color-coded).  
   - **Confidence** → Tells if the data is trustworthy.  
   - **Origin** → Source of the data (manual or automated).  
   - **Permissible Actions Protocol** → Rules on how the data can be used.  

---

# In Short:
- **Dashboard** = Control panel for analysts.  
- **Events** = Incidents (container of details).  
- **Attributes** = Data points (IPs, domains, files).  
- **Attachments** = Malware samples, reports, etc.  
- **Feeds** = Updated threat intelligence sources.  
- **Taxonomies** = Classification system with tags.  
- **Tagging** = Labels to make events organised and shareable.  
