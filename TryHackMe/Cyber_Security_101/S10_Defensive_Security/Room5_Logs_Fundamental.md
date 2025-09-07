# 📌 Logs in Cybersecurity 

## 🔹 What are Logs?
Logs = Digital footprints  
- Just like **footprints in snow** help police in real life,  
  **logs in a computer** help investigators trace activities.  
- Every small action (normal OR malicious) leaves some log.

👉 Example:  
- Real life: Footprints → "Someone entered the cabin"  
- Digital life: Log entry → "Someone logged in at 3:45 PM from IP 192.168.1.10"



## 🔹 Why Logs are Important?
Logs act like **CCTV + diary + medical report** of a computer system.  
They help in:  
1. Detecting suspicious activity  
2. Investigating incidents  
3. Fixing problems  
4. Checking performance  
5. Proving compliance



## 🔹 Use Cases of Logs

| Use Case                       | Description (Easy Words)                                                                 | Example |
|--------------------------------|-------------------------------------------------------------------------------------------|---------|
| **Security Events Monitoring** | Real-time checking of logs to spot unusual behavior.                                      | If user tries 100 wrong passwords in 5 min → suspicious. |
| **Incident Investigation**     | Finding exact details of what happened during an attack.                                  | Check logs to see hacker entered via weak password. |
| **Troubleshooting**            | Finding and fixing errors in system or app.                                               | App crashed → error logs tell "Database not connected." |
| **Performance Monitoring**     | Logs show speed & health of applications.                                                 | Server slow → logs show "High CPU usage at 9 PM." |
| **Auditing & Compliance**      | Proving rules were followed by showing digital trail.                                     | Company must prove "who accessed payroll data" → logs help. |



## 🔹 Real Life Analogy
- Logs = **Detective clues**  
- Without logs → like solving crime with no evidence.  
- With logs → like solving crime with CCTV + footprints + broken door signs.



## 🔹 Quick Comparison: Physical vs Digital Traces

| Physical World (Police Case) | Digital World (Computer Case) |
|------------------------------|--------------------------------|
| Footprints in snow           | Login attempts in logs         |
| CCTV footage                 | Security camera logs / system logs |
| Broken door                  | Firewall breach logs           |
| Witness statement            | Application logs / error logs  |



## 🔹 Summary
- Logs = backbone of security investigation.  
- They record everything: **who, what, when, where, how**.  
- By combining multiple logs → we can trace the attacker.  

---

# 📌 Types of Logs in Cybersecurity 

## 🔹 Why Different Types of Logs?
- A system generates thousands of log entries every day.  
- If we open the full log file, it looks **too messy** (like a big jungle 🌲).  
- To make investigation easier → logs are divided into **categories**.  
- Each category = specific type of information.  

👉 Example:  
- You want to check login attempts in Windows → **Security Logs**.  
- You want to check if the system crashed → **System Logs**.  
- You want to check if someone accessed a website → **Access Logs**.  



## 🔹 Main Types of Logs

| Log Type          | Usage (Easy Words)                                                                 | Example Events |
|-------------------|--------------------------------------------------------------------------------------|----------------|
| **System Logs**   | Show health & activities of the Operating System. Helpful for troubleshooting.      | - Startup / Shutdown <br> - Driver loading <br> - Hardware issues <br> - System errors |
| **Security Logs** | Record all **security-related** activities. Helpful for detecting suspicious acts.  | - Login success/fail <br> - User account changes <br> - Security policy changes <br> - Abnormal activity |
| **Application Logs** | Show what happens inside applications.                                           | - User actions <br> - App crashes <br> - Updates <br> - App errors |
| **Audit Logs**    | Keep track of **who did what** for compliance & monitoring.                         | - Data access <br> - Policy enforcement <br> - User activity <br> - System changes |
| **Network Logs**  | Show details of incoming/outgoing network traffic. Useful for attack investigation. | - Firewall logs <br> - Connection logs <br> - Incoming traffic <br> - Outgoing traffic |
| **Access Logs**   | Show who accessed which resource (websites, databases, apps, APIs).                 | - Web server access <br> - Database queries <br> - API usage <br> - App resource access |



## 🔹 Real Life Analogy
Think of a **hospital** 🏥:

| In Hospital (Physical)         | In Computer (Digital)   |
|--------------------------------|--------------------------|
| Patient record file            | System Logs             |
| Security guard entry register  | Security Logs           |
| Doctor’s treatment notes       | Application Logs        |
| Government health inspection   | Audit Logs              |
| CCTV monitoring at gates       | Network Logs            |
| Visitor entry register         | Access Logs             |



## 🔹 Why Categorization Helps?
- Instead of searching a big log jungle, you focus only on the **right category**.  
- Saves time + makes investigation faster.  

👉 Example:  
- Want login info? → Check **Security Logs**.  
- Want to know if a website was visited? → Check **Access Logs**.  
- Want to find why system crashed? → Check **System Logs**.  



## 🔹 Log Analysis 
Logs alone = raw data 📑  
To find value → we need **Log Analysis**.

### What is Log Analysis?
- Process of **extracting useful information** from logs.  
- Goal = Find abnormal or suspicious activities.  
- Manual search = very hard (too much data).  
- So, we use **manual methods + automated tools**.



## 🔹 Examples of Log Analysis
1. **Windows Security Logs** → Check failed login attempts at night.  
2. **Web Server Access Logs** → Check which IP visited website most times.  



## 🔹 Summary
- Logs are divided into categories → easier to investigate.  
- Different log types serve different purposes.  
- Log Analysis = technique to find useful info from logs.  
- Without analysis, logs = huge diary 📔 that no one can read.  
---
# 📌 Windows Logs & Event Viewer

## 🔹 Logs in Windows OS
Like other operating systems, Windows also keeps track of activities.  
These records are stored in different **log files**, each serving a purpose.

### Main Types of Windows Logs
1. **Application Logs**
   - Related to apps running on Windows.
   - Store errors, warnings, and compatibility issues.
   - 👉 Example: If MS Word crashes, details are stored here.

2. **System Logs**
   - Related to the operating system itself.
   - Store info about drivers, hardware, services, startup/shutdown.
   - 👉 Example: "Graphics driver failed to load" → stored in System Logs.

3. **Security Logs**
   - Most important for security monitoring.
   - Store info about user logins, account changes, security policy changes.
   - 👉 Example: "User failed to login 5 times" → stored in Security Logs.



## 🔹 Event Viewer Utility
Unlike other systems where you manually open log files,  
Windows has a built-in tool → **Event Viewer**.

### How to Open?
- Press **Start** → type `Event Viewer` → press Enter.

### What You’ll See in Event Viewer:
1. **Windows Logs section** (Application, System, Security).
2. **Detailed logs** → list of events.
3. **Options panel** → search, filter, and analyze logs.

👉 Example: If you want to check login attempts, open **Security Logs**.



## 🔹 Structure of an Event Log Entry
When you double-click a log, you’ll see fields like:

- **Description** → Detailed info of the activity.  
- **Log Name** → Which log file (System, Security, Application).  
- **Logged** → Date & time of the event.  
- **Event ID** → Unique number for activity type.  



## 🔹 Important Event IDs in Windows

| Event ID | Description                                |
|----------|--------------------------------------------|
| **4624** | User account successfully logged in        |
| **4625** | User account failed to login               |
| **4634** | User account logged off                    |
| **4720** | A new user account was created             |
| **4724** | Attempt to reset a password                |
| **4722** | User account enabled                       |
| **4725** | User account disabled                      |
| **4726** | User account deleted                       |

👉 Example: If you want to see all successful logins → search for **Event ID 4624**.



## 🔹 Filtering Logs in Event Viewer
Windows Event Viewer has a feature → **Filter Current Log**.

Steps:
1. Open Event Viewer → Select **Security Logs**.  
2. Click **Filter Current Log**.  
3. Enter Event ID (e.g., `4624`).  
4. Click OK → Only filtered logs will appear.  

👉 Example: Filtering Event ID 4624 → You will only see successful login events.



## 🔹 Why This is Useful?
- Instead of going through thousands of logs,  
  you just filter by **Event ID** and quickly find what you need.  
- Saves time during investigation.  



## 🔹 Summary
- Windows OS has **Application, System, Security Logs** as main categories.  
- **Event Viewer** = GUI tool to view & analyze logs.  
- Each log entry has fields like Description, Log Name, Logged time, and Event ID.  
- **Event IDs** are shortcuts → you can search/filter by them.  
- Example: `4624 = Successful Login`, `4625 = Failed Login`.  

---
# 📌 Web Server Access Logs & Manual Log Analysis

## 🔹 What are Web Server Access Logs?
- Whenever we visit or interact with a website (view, login, upload file),  
  the web server **records these requests** in a log file.  
- This log file keeps a **history of all website requests**.  

👉 Example: Apache Web Server stores logs in:  
`/var/log/apache2/access.log`



## 🔹 Fields in an Access Log Entry
Each line in the log tells us details of one request.  

**Sample Log Entry:**
```
172.16.0.1 - - [06/Jun/2024:13:58:44] "GET /products HTTP/1.1" 404 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64)..."
```

| Field        | Meaning (Easy Words)                                                                 | Example Value |
|--------------|----------------------------------------------------------------------------------------|---------------|
| **IP Address** | Who made the request (user’s machine identity).                                      | `172.16.0.1` |
| **Timestamp** | When the request happened.                                                            | `[06/Jun/2024:13:58:44]` |
| **HTTP Method** | What action was requested.                                                          | `GET`, `POST` |
| **URL**      | Which page/resource was requested.                                                     | `/products` |
| **Status Code** | Server’s reply → whether request succeeded/failed.                                  | `200 OK` (success), `404 Not Found`, `500 Internal Error` |
| **User-Agent** | Info about the browser, OS, and device making the request.                           | `Mozilla/5.0 ... Chrome/58` |



## 🔹 Common HTTP Status Codes
| Code | Meaning             | Example Situation                          |
|------|---------------------|---------------------------------------------|
| 200  | OK (success)        | Page loaded successfully                   |
| 404  | Not Found           | User asked for a missing page `/xyz`       |
| 500  | Server Error        | Website error due to server-side issue     |
| 403  | Forbidden           | User tried to access restricted resource   |

---

## 🔹 Manual Log Analysis (Linux Commands)

### 1. **cat** → Show contents of a log file
```
cat access.log
```
👉 Displays all log entries in the file.  
👉 Can also **combine multiple logs**:
cat access1.log access2.log > combined_access.log



### 2. **grep** → Search specific text or pattern
```
grep "192.168.1.1" access.log
```
👉 Finds all lines where this IP appears.  
👉 Useful for checking:
- If a specific user visited site.
- If a resource was accessed multiple times.



### 3. **less** → View logs page by page
```
less access.log
```
👉 Good for big log files.  
- Use **Spacebar** → next page  
- Use **b** → previous page  
- Use **/pattern** → search  
- Use **n** → next occurrence  
- Use **N** → previous occurrence  



## 🔹 Why Logs Rotate?
- Log files grow **too big** quickly.  
- To manage size, systems **rotate logs** → create new log files for specific time ranges.  
👉 Example:  
- `access.log.1`, `access.log.2.gz` etc.  



## 🔹 Real-Life Example of Analysis
Suppose we want to detect suspicious activity:
1. A hacker is trying to find hidden pages → lots of **404 errors** in a row.  
2. A brute force login attempt → many **failed login requests** from the same IP.  
3. DDoS attack → too many requests in a short time from one IP.  

By filtering/searching in logs with `grep`, we can spot these patterns.  



## 🔹 Summary
- Web servers store all requests in **access logs**.  
- Logs contain IP, time, method, URL, status, and User-Agent.  
- Important Linux tools for manual log analysis:  
  - **cat** → display/merge logs  
  - **grep** → search patterns (IP, URL, errors)  
  - **less** → scroll & search logs easily  
- Logs are rotated to avoid huge file sizes.  
- Manual log analysis helps detect **errors, suspicious activity, attacks**.  

