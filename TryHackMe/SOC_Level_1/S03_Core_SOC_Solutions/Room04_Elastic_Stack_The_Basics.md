# Elastic Stack (ELK) 


## What is Elastic Stack (ELK)?
Elastic Stack (also called **ELK**) is a set of tools that help us:
- **Collect data**
- **Store data**
- **Search data**
- **See data in graphs and charts**

SOC teams use ELK to **read logs**, **find problems**, and **investigate attacks**.

Think of ELK like a **smart CCTV system for data**:
- It collects data
- Stores it safely
- Helps you search
- Shows everything clearly on screen

---

## Main Components of ELK (Only 4 – Easy to Remember)
<img width="797" height="288" alt="Screenshot 2026-01-04 at 12 15 53 PM" src="https://github.com/user-attachments/assets/3b3f8620-2c84-48e9-adc5-ea3318517c90" />





---

## 1️⃣ Elasticsearch (Storage + Search Engine)

**Simple meaning:**  
Elasticsearch is the **brain + store room** of ELK.

### What it does:
- Stores data (logs)
- Searches data very fast
- Understands JSON data
- Lets us ask questions like:
  - Which user failed login?
  - At what time attack happened?

### Easy Example:
Imagine a **big notebook** where:
- Every page is a log
- You can instantly find any word in seconds

➡️ **Elasticsearch = Data store + Search**

---

## 2️⃣ Logstash (Data Cleaner & Manager)

**Simple meaning:**  
Logstash is the **cleaner and sorter** of data.

### What it does:
- Takes data from many places
- Cleans and formats data
- Sends clean data to Elasticsearch

### Logstash has 3 parts (Very Important):

| Part | Meaning |
|----|----|
| Input | From where data comes |
| Filter | Cleaning and fixing data |
| Output | Where data goes |

### Easy Example:
Think of **school homework**:
- Input → Teacher gives homework
- Filter → You write neatly
- Output → You submit notebook

➡️ **Logstash = Data cleaner + organizer**

---

## 3️⃣ Beats (Data Collectors)

**Simple meaning:**  
Beats are **small helpers** that collect data from systems.

### What they do:
- Run on computers/servers
- Send logs to Logstash or Elasticsearch

### Common Beats:
- Winlogbeat → Windows logs
- Filebeat → Log files
- Packetbeat → Network traffic

### Easy Example:
Like **delivery boys**:
- Pick items from different houses
- Deliver them to one big warehouse

➡️ **Beats = Data collectors**

---

## 4️⃣ Kibana (Dashboard & Graphs)

**Simple meaning:**  
Kibana is the **screen** where you see everything.

### What it does:
- Shows data in charts and graphs
- Helps investigation
- Makes dashboards

### Easy Example:
Like **Google Maps**:
- Data is already there
- You just see it clearly

➡️ **Kibana = Visualization & analysis**

---

## How ELK Works Together (Flow)

Beats → Logstash → Elasticsearch → Kibana

<img width="1367" height="453" alt="Screenshot 2026-01-04 at 12 15 24 PM" src="https://github.com/user-attachments/assets/1514b32d-a35d-4041-b5ed-dccd3ae930fe" />

### Step-by-step:
1. Beats collect logs from systems
2. Logstash cleans and formats logs
3. Elasticsearch stores and searches logs
4. Kibana shows logs as graphs and tables

---

## One-Line Revision (Exam Ready)

- Elasticsearch → Store & search data
- Logstash → Clean & process data
- Beats → Collect data
- Kibana → Show data visually

---

## SOC Analyst Tip 💡
You **do NOT need** to be expert in setup.
You **MUST know**:
- How to search logs
- How to use Kibana
- How to investigate alerts

---
# Kibana – Discover Tab 


## What is Discover Tab?
The **Discover Tab** is the **most important screen** for a SOC Analyst in Kibana.

This is the place where you:
- See raw logs
- Search logs
- Apply filters
- Find suspicious activity
- Investigate incidents

👉 **SOC analysts spend MOST of their time here**

<img width="781" height="361" alt="Screenshot 2026-01-04 at 12 16 28 PM" src="https://github.com/user-attachments/assets/44a541b1-9339-4a21-81ca-571b3d5ef121" />



---

## Main Parts of Discover Tab (One by One)

---

## 1️⃣ Logs (Middle Area)

- Each **row = one log event**
- Shows what happened, when it happened, and related details
- You can click a log to expand it

📌 Think of logs like **CCTV entries** – one entry per event

---

## 2️⃣ Fields Pane (Left Side)

- Shows all **fields** extracted from logs
- Example fields:
  - source_ip
  - username
  - country
  - event_type

When you click a field:
- You see **Top 5 values**
- You see **percentage usage**

Buttons:
- **+** → show only logs with this value  
- **-** → remove logs with this value  

📌 This helps filter data **without typing**

---

## 3️⃣ Index Pattern

- Index pattern tells Kibana **which logs to read**
- Different log sources = different index patterns

Example:
- VPN logs → vpn_connections index pattern
- Windows logs → winlogbeat index pattern

📌 One index pattern can point to **many indices**

---

## 4️⃣ Search Bar (Very Important ⭐)

- Used to search logs
- Uses **KQL (Kibana Query Language)**

Two search types:
1. Free text search
2. Field-based search

(Explained below 👇)

---

## 5️⃣ Time Filter

- Filters logs based on time
- Examples:
  - Last 15 minutes
  - Last 24 hours
  - Custom range

📌 Attacks always depend on **time**, so this is critical

---

## 6️⃣ Timeline (Graph / Histogram)

- Shows **number of events over time**
- Each bar = log count for that time

Uses:
- Detect log spikes
- Find unusual activity

📌 Sudden spike = possible attack 🚨

---

## 7️⃣ Top Bar

Options to:
- Save search
- Open saved searches
- Share search
- Export data

📌 Useful when working in a SOC team

---

## 8️⃣ Add Filter

- Filter logs using dropdown
- No need to type full query

Example:
- Field: country
- Value: United States

📌 Easy and beginner-friendly

---

## Index Pattern (Easy Explanation)

- Logs from different sources look different
- Kibana **normalizes logs** into fields
- Index pattern helps Kibana understand those fields

Example:
- VPN logs → vpn_connections
- Firewall logs → firewall_logs

📌 Without index pattern, Kibana cannot show data
<img width="565" height="414" alt="Screenshot 2026-01-04 at 12 16 52 PM" src="https://github.com/user-attachments/assets/007d92dd-6aa1-450f-bd84-c63a928da03a" />

---

## Fields Pane (Why It Is Powerful)

- Shows ready-made fields
- Helps you:
  - Understand log structure
  - Apply quick filters
  - Reduce noise

📌 Best tool for beginners
<img width="568" height="398" alt="Screenshot 2026-01-04 at 12 17 03 PM" src="https://github.com/user-attachments/assets/0e8d0628-84ea-4c42-9055-fa841b559162" />

---

## Create Table (Reduce Noise ✂️)

By default:
- Logs are messy
- Too many fields

You can:
- Click a log
- Select important fields only
- Create a clean table

Benefits:
- Easy investigation
- Clear view
- Less confusion

📌 You can **save the table format**

---

## KQL – Kibana Query Language (Simple)

### 1️⃣ Free Text Search

Search any word inside logs.

Example:
United States

- Finds logs containing **exact term**
- Searching only `United` → ❌ no result

Use wildcard:
United*

- Matches United States, United Kingdom, etc.

---

### 2️⃣ Logical Operators

**AND**
"United States" AND "Virginia"

**OR**
"United States" OR "England"

**NOT**
"United States" AND NOT ("Florida")

📌 Helps narrow results smartly

---

### 3️⃣ Field-Based Search (Most Accurate)

Format:
Field : Value

Example:
Source_ip : 238.163.231.224 AND UserName : Suleman

Meaning:
- Source IP must match
- Username must be Suleman

📌 Best method for investigations

---

## One-Page Revision Summary 📝

- Discover Tab = log investigation area
- Logs = one event per row
- Fields Pane = quick filters
- Index Pattern = tells Kibana which logs to read
- Search Bar = KQL searches
- Time Filter = filter by time
- Timeline = find spikes
- Tables = clean log view

---
# Kibana – Visualization & Dashboard 
(Simple English • Beginner Friendly • Easy to Revise)

## What is the Visualization Tab?
The **Visualization Tab** is used to **convert logs into graphs and tables**.

Instead of reading raw logs, we can:
- See data as **tables**
- Understand patterns using **pie charts**
- Compare values using **bar charts**

👉 This makes data **easy to understand** for SOC analysts and managers.




---

## How to Create a Visualization

### Method 1 (Easy Way)
- Go to **Discover Tab**
- Click on any field
- Click **Visualize**

This directly opens the **Visualization Tab**.

---

## Types of Visualizations You Can Create

- Table
- Pie Chart
- Bar Chart
- Line Chart

📌 Choose based on what you want to show.

---

## Correlation (Very Important ⭐)

**Correlation means:**  
Showing relationship between **two or more fields**

### Example:
- Source_IP
- Source_Country

By dragging another field into the middle:
- Kibana shows how **IPs are related to countries**

📌 Useful for:
- Attack analysis
- Geo-location patterns

---

## Example 1: Pie Chart (Top 5 Source Countries)

Purpose:
- Show **top countries** generating traffic

Steps:
- Choose Pie Chart
- Select Source_Country
- Limit results to Top 5

📌 Helps identify suspicious countries quickly

---

## Example 2: Table (IP vs Country Count)

Purpose:
- Show Source_IP and Source_Country together
- Easy comparison

Columns:
- Source_IP
- Source_Country
- Count

📌 Tables are best for **detailed investigation**

---

## Saving a Visualization (Very Important 🚨)

After creating a visualization:

1. Click **Save** (top right)
2. Add:
   - Title
   - Description
3. Click **Save and add to library**

📌 If you don’t save → your work is lost

---

## Failed Connection Attempts Visualization

Goal:
- Show **which user**
- From **which IP**
- Failed to connect

Best Visualization:
- **Table**

Fields to include:
- UserName
- Source_IP
- Failure Count

📌 Very useful for brute-force attack detection

---

## Dashboard (Big Picture View 🧠)

**Dashboard = Collection of visualizations + saved searches**

It gives:
- One-screen visibility
- Quick understanding of system activity


::contentReference[oaicite:1]{index=1}


---

## How to Create a Custom Dashboard

### Step-by-Step:

1. Go to **Dashboard Tab**
2. Click **Create Dashboard**
3. Click **Add from Library**
4. Select:
   - Saved Visualizations
   - Saved Searches
5. Arrange boxes properly
6. Click **Save Dashboard**

📌 Always save after editing

---

## Why Dashboards Are Important for SOC

- Quick incident overview
- Easy reporting
- Less time reading logs
- Better decision making

---

## One-Page Revision Summary 📝

- Visualization Tab → Graphs & tables
- Correlation → Relationship between fields
- Pie Chart → Distribution (Top values)
- Table → Detailed analysis
- Save Visualizations → Add to library
- Dashboard → Combine everything
- Dashboard = SOC control panel

---


