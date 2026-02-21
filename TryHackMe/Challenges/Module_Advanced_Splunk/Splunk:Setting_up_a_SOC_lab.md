# Splunk + Universal Forwarder (Linux Lab) 


---

# 1️⃣ What is Splunk?

Splunk = SIEM tool  

It helps to:
- Collect logs
- Analyze logs
- Search logs
- Detect attacks in real time

---

# 2️⃣ Installing Splunk Enterprise (Linux)
<img width="943" height="240" alt="Screenshot 2026-02-20 at 10 57 20 PM" src="https://github.com/user-attachments/assets/caf4d0f4-8340-44d8-8dd8-15d54b2956c1" />

### Step 1 – Switch to root user

sudo su

---

### Step 2 – Extract installer

tar xvzf splunk_installer.tgz

This creates a folder called:

splunk

---

### Step 3 – Move Splunk to /opt

mv splunk /opt/

---

### Step 4 – Start Splunk

Go to:

cd /opt/splunk/bin

Then run:

./splunk start --accept-license

First time:
- It will ask to create admin username
- Set password (minimum 8 characters)

---

### Step 5 – Access Web Interface

Open browser inside VM:

http://coffely:8000

Or:

http://MACHINE_IP:8000

Login using admin credentials.

---

# 3️⃣ Important Splunk CLI Commands

All commands are run from:

/opt/splunk/

---

Start Splunk:

./bin/splunk start

---

Stop Splunk:

./bin/splunk stop

---

Restart Splunk:

./bin/splunk restart

---

Check Status:

./bin/splunk status

---

Search from CLI:

./bin/splunk search coffely

---

Help Command:

./bin/splunk help

Most important for learning.

---

# 4️⃣ What is Splunk Forwarder?

Forwarder = Agent that sends logs to Splunk.

There are 2 types:

Heavy Forwarder  
- Can filter logs  
- Can modify logs  

Universal Forwarder  
- Lightweight  
- Just sends logs  
- No filtering  

In this lab → we use Universal Forwarder.

---

# 5️⃣ Install Universal Forwarder (Linux)

### Step 1 – Switch to root

sudo su

---

### Step 2 – Extract forwarder

tar xvzf splunkforwarder.tgz

---

### Step 3 – Move to /opt

mv splunkforwarder /opt/

---

### Step 4 – Start forwarder

cd /opt/splunkforwarder

./bin/splunk start --accept-license

Create admin credentials.

---

### ⚠️ Port Issue

Default forwarder management port = 8089

If already used → change it

Example:

Enter new mgmt port: 8090

---

# 6️⃣ Configure Splunk to Receive Logs

Go to Splunk Web:

Settings → Forwarding and Receiving  
Click Configure Receiving  
Add new receiving port  

Default receiving port:

9997

Now Splunk listens on port 9997.
<img width="910" height="675" alt="Screenshot 2026-02-20 at 10 58 39 PM" src="https://github.com/user-attachments/assets/3060cde3-a56e-40b3-9303-6b2bb9e44538" />
<img width="1132" height="403" alt="Screenshot 2026-02-20 at 10 59 03 PM" src="https://github.com/user-attachments/assets/3e6ffd03-aa2b-4e80-a8f3-ce9eb25bca28" />
<img width="1205" height="334" alt="Screenshot 2026-02-20 at 10 59 16 PM" src="https://github.com/user-attachments/assets/f1c648eb-e540-45ce-b721-b107a2df0440" />
<img width="1194" height="350" alt="Screenshot 2026-02-20 at 10 59 29 PM" src="https://github.com/user-attachments/assets/3315f490-a4cf-4bcb-8bef-0ba1fc2e0903" />

---

# 7️⃣ Create New Index

Go to:

Settings → Indexes → New Index

Create index:

Linux_host

If not created → logs go to default "main" index.
<img width="1036" height="814" alt="Screenshot 2026-02-20 at 10 59 46 PM" src="https://github.com/user-attachments/assets/836f9b3e-8691-404c-abc2-0263fff69efc" />
<img width="1214" height="449" alt="Screenshot 2026-02-20 at 10 59 57 PM" src="https://github.com/user-attachments/assets/2e6ad2b1-6566-4688-a060-027b3655fec8" />
<img width="795" height="622" alt="Screenshot 2026-02-20 at 11 00 07 PM" src="https://github.com/user-attachments/assets/96785920-b86e-4e11-8158-9a09f7a5aba4" />

---

# 8️⃣ Configure Forwarder to Send Logs

Go to:

cd /opt/splunkforwarder/bin

Add forward server:

./splunk add forward-server MACHINE_IP:9997

Login with Splunk admin credentials.

Now forwarder knows where to send logs.

---

# 9️⃣ Add Log File to Monitor

Linux logs are stored in:

/var/log/

Example file:

/var/log/syslog

Add monitoring:

./splunk add monitor /var/log/syslog -index Linux_host

Now forwarder monitors syslog.

---

# 🔟 Check inputs.conf

Location:

/opt/splunkforwarder/etc/apps/search/local/

View file:

cat inputs.conf

Example content:

[monitor:///var/log/syslog]
disabled = false
index = Linux_host

This confirms monitoring is active.

---

# 1️⃣1️⃣ Generate Test Log

Use logger command:

logger "coffely-has-the-best-coffee-in-town"

This adds log to syslog.

Since syslog is monitored,
it will appear in Splunk.

Search in Splunk:

index=Linux_host

You should see your test log.

---

# Complete Flow 

Linux System  
↓  
Universal Forwarder  
↓  
Port 9997  
↓  
Splunk Enterprise  
↓  
Stored in Index (Linux_host)  
↓  
Searchable in Dashboard  

---

# Final Quick Summary

Splunk Enterprise:
Collects and analyzes logs.

Universal Forwarder:
Sends logs from endpoint to Splunk.

Important ports:
8000 → Web Interface  
9997 → Receiving port  
8089 → Default management port  

Key Steps:
Install Splunk  
Start Splunk  
Install Forwarder  
Configure Receiving  
Create Index  
Add Forward Server  
Monitor Log File  

Now logs from Linux are visible in Splunk.

---
# Splunk on Windows + Forwarder + Web Logs 

---

# 1️⃣ Install Splunk Enterprise (Windows)
<img width="900" height="270" alt="Screenshot 2026-02-20 at 11 00 59 PM" src="https://github.com/user-attachments/assets/7a95984e-37c1-426e-ba06-cd6ed9c3f2fe" />

### Step 1 – Run Installer

File is already in:

Downloads folder  
<img width="838" height="504" alt="Screenshot 2026-02-20 at 11 01 14 PM" src="https://github.com/user-attachments/assets/bf94e6a0-baa6-446e-83c3-96dfd5aa9c42" />

Double click:

Splunk-Instance installer  

---

### Step 2 – Accept License

✔ Check License Agreement  
Click Next  

---

### Step 3 – Create Admin Account

Create:

- Username  
- Strong password  

This account has full control of Splunk.

---

### Step 4 – Installation Location

Default path:

C:\Program Files\Splunk

Installation takes 5–8 minutes.

---

### Step 5 – Access Splunk

Open browser inside VM:

http://127.0.0.1:8000  

Or:

http://MACHINE_IP:8000  

Login using admin credentials.

---

# 2️⃣ Configure Splunk to Receive Data

Go to:

Settings → Forwarding and Receiving  

Click:

Configure Receiving  

Add new port:

9997 (default receiving port)

Now Splunk is listening on port 9997.

---

# 3️⃣ Install Splunk Universal Forwarder (Windows)

### Step 1 – Run Forwarder Installer

File already in Downloads folder.

Double click installer.

✔ Accept License  
Choose On-Premises option  

---

### Step 2 – Create Forwarder Account

Create:

- Username  
- Password  

Used to connect forwarder to Splunk Indexer.

---

### Step 3 – Deployment Server (Optional)

Can skip in this lab.

Used in large environments.

---

### Step 4 – Setup Listener

Enter:

Splunk Server IP  
Port: 9997  

This tells forwarder where to send logs.

Installation takes 3–5 minutes.

---

# 4️⃣ Verify Forwarder Connection

Go to:
<img width="701" height="581" alt="Screenshot 2026-02-20 at 11 02 01 PM" src="https://github.com/user-attachments/assets/58df5467-7adf-48d5-8c70-f89123b00e09" />

Settings → Forwarder Management  

You should see:

Host name (example: coffelylab)

This confirms forwarder is connected.
<img width="1193" height="316" alt="Screenshot 2026-02-20 at 11 02 12 PM" src="https://github.com/user-attachments/assets/dd48c849-a845-4a44-9b78-3a18af474dc1" />

---

# 5️⃣ Ingest Windows Event Logs

### Step 1 – Add Data

Go to:

Settings → Add Data  

Select:

Forward  

<img width="749" height="617" alt="Screenshot 2026-02-20 at 11 02 41 PM" src="https://github.com/user-attachments/assets/542b87f2-e4dc-4fdc-9c49-3290f85fcd63" />
<img width="976" height="249" alt="Screenshot 2026-02-20 at 11 02 57 PM" src="https://github.com/user-attachments/assets/746561f1-bd1d-4533-8802-672d5fc5e2e8" />

---
### Step 2 – Select Host

Move:

coffelylab  

From Available Hosts → Selected Hosts  

Click Next  
<img width="995" height="457" alt="Screenshot 2026-02-20 at 11 03 30 PM" src="https://github.com/user-attachments/assets/ecd9d7ec-6a87-4679-8e5d-ce63e7581220" />

---

### Step 3 – Select Log Source

Choose:

Local Event Logs  

Select important logs like:

- Application  
- Security  
- System  
<img width="1068" height="508" alt="Screenshot 2026-02-20 at 11 03 42 PM" src="https://github.com/user-attachments/assets/fb6d5210-3232-4018-a3cd-d8645f7c9a23" />

---

### Step 4 – Create Index

Create new index:

Windows_host  

Select it.
<img width="688" height="303" alt="Screenshot 2026-02-20 at 11 03 52 PM" src="https://github.com/user-attachments/assets/603127f4-98cf-424c-90ac-bbca507f2172" />

---

### Step 5 – Start Searching

Click:

Start Searching  

You should now see Windows Event Logs.
<img width="627" height="347" alt="Screenshot 2026-02-20 at 11 04 12 PM" src="https://github.com/user-attachments/assets/635ccdf1-895a-4f08-b896-cbbd9378cc7f" />
<img width="900" height="391" alt="Screenshot 2026-02-20 at 11 04 22 PM" src="https://github.com/user-attachments/assets/cd7a6f97-d07c-4c4c-9218-989942e9dfb1" />

---

# 6️⃣ Ingest IIS Web Logs (Coffely Website)

Website URL inside VM:

http://coffely.thm  

Logs are stored in:

C:\inetpub\logs\LogFiles\W3SVC*

These files contain:

- User requests  
- Responses  
- Orders placed  
- HTTP status codes  

---

## Steps to Ingest Web Logs

### Step 1 – Add Data

Settings → Add Data  
Select Forward  

---

### Step 2 – Select Forwarder Host

Choose web host machine.

---

### Step 3 – Configure Log Directory

Enter path:

C:\inetpub\logs\LogFiles\W3SVC*

This monitors all IIS log files.

---

### Step 4 – Select Source Type

Choose:

IIS  

(Create new index like:)

Web_host  

---

### Step 5 – Review & Submit

Confirm settings.

Visit:

http://coffely.thm  

Generate activity.

Logs appear in Splunk in 4–5 minutes.

---

# 7️⃣ What We Achieved

Windows Host  
↓  
Universal Forwarder  
↓  
Port 9997  
↓  
Splunk Enterprise  
↓  
Stored in:

Windows_host index  
Web_host index  

Now logs are searchable.

---

# 8️⃣ Important Ports

8000 → Splunk Web Interface  
9997 → Receiving Port  
8089 → Management Port  

---

# Final Simple Summary

Splunk Enterprise:
Collects and searches logs.

Universal Forwarder:
Sends logs from host to Splunk.

Windows Lab:
- Installed Splunk
- Installed Forwarder
- Ingested Event Logs
- Ingested IIS Web Logs

Now you can:
- Monitor user activity
- Track website orders
- Detect suspicious behavior

---


