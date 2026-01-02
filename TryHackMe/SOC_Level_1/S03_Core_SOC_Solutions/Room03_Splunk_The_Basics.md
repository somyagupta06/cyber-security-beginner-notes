# Splunk Components 

Splunk is a tool made by **:contentReference[oaicite:1]{index=1}**.
It is used to **collect, store, and search logs** (computer records).

Splunk has **3 main parts**:
1. Forwarder
2. Indexer
3. Search Head

<img width="702" height="234" alt="Screenshot 2026-01-02 at 8 35 08 AM" src="https://github.com/user-attachments/assets/ed1556c2-40bc-482d-81b0-69088116aa3f" />


You can remember it like this:
**Collect → Store → Search**

----------------------------------

## 1. Splunk Forwarder (Collector)

Think of Forwarder as a **postman**.
<img width="618" height="324" alt="Screenshot 2026-01-02 at 8 50 29 AM" src="https://github.com/user-attachments/assets/41cd5800-612d-4052-b53c-ec5d56e3a4aa" />

### What does it do?
- It **collects logs** from a computer
- It **sends logs** to the Indexer
- It is very **lightweight** (does not slow the system)

### Where is it installed?
- On the system that creates logs

### Examples of data it collects:
- Website traffic logs
- Windows Event Logs
- Linux system logs
- Database error logs

### Important points:
- It only **collects and sends**
- It does **not store** data
- It does **not analyze** data

----------------------------------

## 2. Splunk Indexer (Store + Process)

Think of Indexer as a **big store room with a smart brain**.

### What does it do?
- Receives data from Forwarder
- **Processes** the data
- Converts raw logs into **events**
- Stores data so it can be searched

### What processing means:
- Breaks data into small parts (fields)
- Example:
  IP = 192.168.1.1  
  User = Admin  
  Action = Login  

### Important points:
- It **stores data**
- It **prepares data for search**
- Without Indexer, searching is not possible

----------------------------------

## 3. Search Head (Search + View)

Think of Search Head as a **Google search page**.

### What does it do?
- Used by users
- Searches data using **SPL (Search Processing Language)**
- Shows results in easy format

### What happens when you search:
- You type a search
- Search Head asks Indexer for data
- Indexer sends matching results
- Search Head shows results

### What can you see here:
- Tables
- Pie charts
- Bar charts
- Reports

----------------------------------

## Very Easy Comparison Table

| Component     | Simple Role        | Example |
|---------------|--------------------|---------|
| Forwarder     | Collect + Send     | Postman |
| Indexer       | Store + Process    | Store room |
| Search Head   | Search + Show      | Google |

----------------------------------

## One Line Revision

- Forwarder = Collects logs
- Indexer = Stores and processes logs
- Search Head = Searches and shows data

----------------------------------

## Flow in One Line

System → Forwarder → Indexer → Search Head → User

----------------------------------

## Exam Friendly Memory Trick

**F I S**
- F = Forwarder
- I = Indexer
- S = Search Head

----------------------------------

# Splunk Home Screen

When you open Splunk,  
the first page you see is called the **Home Screen**.

This screen has **4 main parts**:
1. Splunk Bar
2. Apps Panel
3. Explore Splunk
4. Home Dashboard

----------------------------------

## 1. Splunk Bar (Top Bar)

The **Splunk Bar** is the top line of the screen.

### What is its use?
It helps you **control and manage Splunk**.

### Options in Splunk Bar:

- Messages  
  Shows system notifications and alerts.

- Settings  
  Used to change Splunk settings.

- Activity  
  Shows running searches and background tasks.

- Help  
  Gives tutorials and official documentation.

- Find  
  Helps you search inside the current app.

### Important point:
You can **switch Splunk apps directly** from the Splunk Bar  
(no need to open Apps Panel every time).

----------------------------------

## 2. Apps Panel

The **Apps Panel** shows all installed Splunk apps.

### Default App:
- Search & Reporting  
  (This app comes with every Splunk installation)

### What apps do:
- Apps give different features
- Example: Searching logs, monitoring security, reports

### Note:
You can open apps from:
- Apps Panel
- OR directly from the Splunk Bar

----------------------------------

## 3. Explore Splunk

This section is for **learning and setup**.

### What you can do here:
- Add data to Splunk
- Install new Splunk apps
- Open Splunk documentation

### Simple meaning:
This part helps beginners **start using Splunk easily**.

----------------------------------

## 4. Home Dashboard

The **Home Dashboard** shows visual data.

### By default:
- No dashboard is selected

### What you can do:
- Select a dashboard from dropdown
- Open dashboard list page
- Create your own dashboards

### Yours Tab:
- Dashboards created by **you**
- Separate from default dashboards

----------------------------------

## One Line Revision

- Splunk Bar = Control panel
- Apps Panel = List of apps
- Explore Splunk = Setup and learning
- Home Dashboard = View dashboards

----------------------------------

## Easy Memory Trick

**B A E D**
- B = Bar
- A = Apps
- E = Explore
- D = Dashboard

----------------------------------

## Very Short Flow

Open Splunk → Choose App → Add Data → Search → View Dashboard

----------------------------------


# Adding Data to Splunk 

Splunk can take **any type of data**.
When data is added, Splunk **breaks it into small events** so it can be searched easily.

----------------------------------

## What is Data in Splunk?

Data means **logs**.

Examples of logs:
- VPN logs
- Website logs
- Firewall logs
- System event logs

Each log line becomes **one event** in Splunk.

----------------------------------

## Data Source Categories

Splunk groups data into categories to understand it better.

Examples:
- Network logs (VPN, firewall)
- Web logs
- System logs
- Application logs

In this task, we work with **VPN logs**.

----------------------------------

## Add Data Screen

When you click **Add Data** on the Splunk Home Screen,
you see different options.

We use the **Upload** option.

### Upload option means:
- Upload log files from your own computer
- No live connection needed

----------------------------------

## Practical Task Overview

You upload the file:
- Name: VPN_logs
- Location in AttackBox:
  /root/Rooms/SplunkBasic/

After upload, the data becomes searchable in Splunk.

----------------------------------

## 5 Steps to Upload Data (Very Important)

### Step 1: Select Source
- Choose the log file (VPN_logs)
- Select where the file comes from

### Step 2: Select Source Type
- Tell Splunk what kind of logs these are
- Example:
  JSON logs
  Syslog
  VPN logs

This helps Splunk read data correctly.

### Step 3: Input Settings
- Choose the **Index** (storage location)
- Set the **Hostname** for the logs

Index = Where data is stored  
Hostname = Name of the system that created logs

### Step 4: Review
- Check all selected settings
- Make sure everything is correct

### Step 5: Done
- Fini
