# Threat Intelligence (TI) 

## 1. What is Threat Intelligence?
Threat Intelligence = Data + Analysis  
We collect data about attackers and threats → then analyse it → then find patterns → then make plans to stop or reduce risks.

So basically:
- Who is attacking?
- Why are they attacking?
- How strong they are?
- What signs (artefacts/indicators) we should look for?

This info helps an organisation (company, govt, etc.) to protect itself.

---

## 2. Main Questions to Ask for Reducing Risk
- Who’s attacking you? (Identify attacker)
- What’s their motivation? (Money? Fame? Politics?)
- What can they do? (Capabilities / Skills)
- What artefacts and indicators should you look out for? (clues of attack like IP, file hash, suspicious domains)

---

## 3. Types (Classifications) of Threat Intelligence

| Type | Meaning (Easy) | Used by |
|------|----------------|---------|
| **Strategic Intel** | Big picture. High-level info about threats, patterns, trends. Helps bosses decide policies & future plans. | Senior Management, Decision Makers |
| **Technical Intel** | Small technical evidence. Attack artefacts (IP, file hash, malware signature). Helps Incident Response team. | SOC, Incident Response Teams |
| **Tactical Intel** | Tactics, Techniques, Procedures (TTPs) of attackers. How they attack. Helps improve security controls. | Security Engineers |
| **Operational Intel** | Specific info about adversary's current plans/motives. Which assets (people, tech) they are targeting now. | Security Teams doing active monitoring |

So:
- Strategic = Big picture, long-term view.
- Tactical = Attacker's methods.
- Technical = Raw technical details.
- Operational = Current active threat info.

---

## 4. Urlscan.io
A free online tool to scan and analyse websites automatically.

When you enter a URL, it records:
- Domains and IP addresses contacted
- Resources requested (files, images)
- Screenshot of the site
- Technologies used
- Other metadata

It has 2 main views:
- **Recent scans** = what other people have recently scanned
- **Live scans** = current live scanning running

### Key parts of URL scan results:
- **Summary** = IP address, domain info, page history, screenshot
- **HTTP** = Data fetched and file types received
- **Redirects** = Any HTTP or client redirects (like site moving to another page)
- **Links** = All outgoing links on homepage
- **Behaviour** = Variables, cookies, frameworks used
- **Indicators** = IPs, domains, hashes found (not always malicious)

Important: Internet changes daily. So results can change on different days.

---

## 5. Abuse.ch Project
Research project from Switzerland to track malware & botnets.
They made 5 main tools/platforms:

| Tool | What it does |
|------|--------------|
| **Malware Bazaar** | Collection of malware samples. Upload & share malware. You can hunt malware via tags, YARA rules, signatures etc. |
| **Feodo Tracker** | Tracks botnet Command & Control (C2) servers for Emotet, Dridex, TrickBot etc. Gives IP & IOC blocklists. |
| **SSL Blacklist** | Detects malicious SSL certificates and JA3 fingerprints used by botnets. Provides deny lists. |
| **URL Haus** | Database of malicious URLs used to spread malware. Search by domain, URL, hash, filetype. |
| **Threat Fox** | Share/search/export Indicators of Compromise (IOCs) like IP, domains, hashes in many formats (MISP, Suricata rules, DNS RPZ, JSON, CSV). |

### In Short:
- MalwareBazaar = All malware samples in one place.
- FeodoTracker = C2 server info & blocklists.
- SSL Blacklist = Bad SSL certificates & JA3 fingerprints.
- URLHaus = Malicious URL database.
- ThreatFox = IOC sharing/exporting tool.

---

## 6. Why These Tools Matter
- They help analysts find, verify and block malicious stuff.
- They make threat intel stronger by sharing data with everyone.
- Example: If you see a suspicious IP, you can check FeodoTracker.
- Example: If you find a weird URL, check URLHaus.
- Example: If you want malware samples, check MalwareBazaar.

---

## 7. Important Note
Because internet data changes daily, today’s scan might show different results tomorrow.

---

# TL;DR
Threat Intelligence = Knowing attackers & their tricks.
It’s divided into Strategic, Technical, Tactical & Operational.
Tools like Urlscan.io and Abuse.ch platforms help security teams collect and use this info to protect organisations.
---
# Task: Analyse Email1.eml 

## Short idea 
Open Email1.eml in Thunderbird inside the VM → copy headers, URLs, attachments and hashes → move these IOCs to your host browser → check on Cisco Talos → decide if email is Phishing / Spam / Benign.

---

## 1) Start VM and open the email
1. Click the "Start Machine" button to start the VM.  
2. Open the VM in Split View so you can see both VM and your host.  
3. Inside the VM, open Thunderbird.  
4. In Thunderbird: File → Open Saved Message... → choose Email1.eml  
   Or drag-and-drop Email1.eml into Thunderbird message list.

---

## 2) Get the raw message and headers (what to copy)
1. Open the email in Thunderbird.  
2. View → Message Source (or press Ctrl+U) to see the raw source.  
3. Copy and save these header fields somewhere (a text file is fine):
   - From:
   - To:
   - Subject:
   - Date:
   - Message-ID:
   - Return-Path:
   - Authentication-Results: (this shows SPF / DKIM / DMARC)
   - All Received: lines (copy all of them — the bottom-most is usually the origin)
   - Any X-headers (like X-Mailer, X-Originating-IP, X-Spam-Status)

Why: Headers tell us who sent the mail, where it came from, and if it passed SPF/DKIM/DMARC.

---

## 3) Save attachments and find file hashes (SHA256)
1. Click attachment in Thunderbird → Save As → save to the VM disk.  
2. To get the SHA256 of the file (do this inside VM):
   - Open Terminal in VM.
   - Run: sha256sum attachment_filename_here.ext
   - Copy the SHA256 value it prints.

Note: If sha256sum is not available, use any local tool that shows file hash. Do NOT run attachments.

---

## 4) Extract URLs and domains from the email body
1. From the Message Source, find all http:// or https:// links and copy them.  
2. Also look for domains in the text (like example.com) and copy them.  
3. Watch out for tricks:
   - hxxp instead of http (replace x with t when checking)
   - URLs with an @ sign (like http://user@evil.com) — be careful, the real host is after @
   - Shorteners (bit.ly, tinyurl) — these may hide final destination

Put all URLs and domains into a simple list.

---

## 5) Find the originating IP (from Received lines)
1. In the Received lines (bottom-most external one), look for an IP in square brackets, e.g. [203.0.113.12].  
2. That IP is often the sending server’s IP. Save it as "Sender IP".

---

## 6) Prepare an IOC table (fill these values)
| IOC Type   | Value                               | What to check on Talos                       | Notes |
|------------|-------------------------------------|----------------------------------------------|-------|
| Sender IP  | (paste the IP here)                 | Check IP reputation, ASN, country, last seen |       |
| Domain     | (paste domain here)                 | Check domain reputation, age, related hosts  |       |
| URL        | (paste full URL here)               | Check URL category, redirect chain, last seen|       |
| SHA256     | (paste SHA256 here)                 | Check file reputation and detection count    |       |
| SPF/DKIM   | (paste results from Authentication-Results) | Use to judge spoofing (fail = suspicious) |       |

---

## 7) How to check these on Cisco Talos (on your host machine — VM has no internet)
1. Copy the IOC table from the VM into a text file or into this chat (paste it).  
2. On your host browser (with internet) open Cisco Talos Reputation Center.  
3. For each IOC:
   - Enter the IP in Talos IP lookup → note Reputation, ASN, Country, Last Seen.  
   - Enter the Domain in Talos Domain lookup → note Reputation, Registration/Age, Related hosts.  
   - Enter the full URL in Talos URL lookup (if available) → note if it is flagged.  
   - Enter the SHA256 in Talos File lookup → note detection vendors and first-seen date.  
4. Also check Talos Email & Spam Data for patterns if available.

---

## 8) Simple decision checklist (easy scoring)
- Mark HIGH suspicion (Phishing) if:
  - SPF or DKIM **fail**, and
  - Sender IP or Domain has **poor** Talos reputation, and
  - URLs point to strange/new domains or redirectors, or
  - Attachment SHA256 is detected by many vendors.

- Mark MEDIUM suspicion (Suspicious Spam) if:
  - Some mismatches or new domain but no confirmed detections.

- Mark LOW suspicion (Benign) if:
  - SPF/DKIM/DMARC all **pass**, domain/IP have good reputation, attachments benign.

Write final decision like:
Decision: PHISHING  
Why: SPF=fail, Sender IP=1.2.3.4 (Talos: poor), URL redirects to bad.example, attachment SHA256 detected by many vendors.

---

## 9) What to paste here so I can help (exactly)
Copy-paste these from the VM into this chat:
1. Raw headers (or at least the fields listed in section 2).  
2. Bottom-most Received lines (all Received lines are better).  
3. Sender IP you found.  
4. List of URLs and domains found.  
5. Attachment filenames and SHA256 values.  
6. Authentication-Results (SPF/DKIM/DMARC lines).

I will then:
- Prepare the filled IOC table for you.
- Explain Talos-style checks and give the final classification (Phishing / Spam / Benign) with 3-4 line justification in plain, simple English.

---

## 10) Extra tips (desi style)
- Agar SPF fail dikhe toh dikkat samajh.  
- Agar URL me @ ya weird ghumao (redirect) ho toh woh shady hai.  
- Naya domain + urgent message = high danger.  
- Humesha attachments ko offline hash karo, aur phir host pe Talos me check karo.

---

Copy-paste the header/IOCs from the VM here and main turant Talos-style analysis aur final decision bana dunga — seedha simple aur short explanation ke saath.

