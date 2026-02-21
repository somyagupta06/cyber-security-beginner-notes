# Splunk Notes — Reports, Dashboards & Alerts 

---

## 1. Why Do We Use Splunk?

Organizations have thousands of computers, servers and users.

All these systems generate **logs (data)**.

Splunk collects all security data in **one place**.

Problem:
There is TOO MUCH data.
Example:
35000+ events → Human cannot read manually.

So Splunk helps us:
- Search data
- Analyze data
- Visualize data
- Detect threats

---

## 2. Search & Reporting Tab

When we login into Splunk:

Go to → Search & Reporting

Here we can search logs.

Example search:

index=*

Meaning:
Show all available data.

⚠️ Important:
Do NOT use "All Time" search in real environments.
Because it increases system load and slows Splunk.

---

## 3. Problem With Raw Data

Raw logs look confusing.

Example:
Thousands of login events.

If analyst reads everything manually:
- Time waste
- Hard to understand attacks
- No quick overview

Solution → Reports & Dashboards

---

## 4. What is a Report?
<img width="1354" height="459" alt="Screenshot 2026-02-21 at 10 37 16 PM" src="https://github.com/user-attachments/assets/1a87cf31-2583-48fe-b99a-1a95cc3304cf" />

A Report = Saved Search

It automatically runs a search and saves results.

Example:
SOC wants login activity every 8 hours.

Instead of manual search:
Create a report.

Report will:
- Run automatically
- Save results
- Show ready data to next analyst

---

## 5. Why Reports Are Useful?

✅ Automatic execution  
✅ Saves analyst time  
✅ Reduces server load  
✅ Searches do not run together

Example:
Search runs every 10 minutes instead of same time.

---

## 6. Creating a Report

Step 1:
Run a search.
<img width="1362" height="266" alt="Screenshot 2026-02-21 at 10 37 36 PM" src="https://github.com/user-attachments/assets/60d1e52d-0890-4c04-9a07-baa169530f9c" />

Example:
host=vpn_server | stats count by Username

Meaning:
Count how many times each VPN user logged in.

Step 2:
Click → Save As → Report
<img width="756" height="491" alt="Screenshot 2026-02-21 at 10 37 48 PM" src="https://github.com/user-attachments/assets/d835d4bd-5f68-456d-8e79-1b6c55220610" />

Step 3:
Fill details:
- Name
- Description
- Permissions

Step 4:
Click Save

Report created ✅

---

## 7. Reports Tab Explained

Reports tab shows:

- Report Name
- Owner
- Permissions
- Next Scheduled Time

Options:

Open in Search → Run same query again  
Edit → Modify report  
Sharing → Who can access report

Private = Only you  
Shared = Other users can see

---

## 8. What is a Dashboard?

Dashboard = Visual View of Reports

Instead of tables,
data is shown using charts.

Purpose:
Quick understanding.

Used by:
- SOC Analysts
- Managers
- Security Teams

---

## 9. Why Dashboards Are Important?

Dashboard helps to see:

- Login spikes
- Attack increase
- Failed logins
- System activity

Example:
Column chart shows:
Sarah logged in most times.

One look → instant understanding.

---

## 10. Creating a Dashboard

Step 1:
Go to Dashboards tab
<img width="1357" height="656" alt="Screenshot 2026-02-21 at 10 39 02 PM" src="https://github.com/user-attachments/assets/4cfb6216-5ac8-458a-8abe-c0d24fe119bc" />

Step 2:
Click Create Dashboard
<img width="757" height="712" alt="Screenshot 2026-02-21 at 10 39 15 PM" src="https://github.com/user-attachments/assets/038a1c04-a6d3-44ea-b7e7-4c7204fb49ed" />

Step 3:
Enter:
- Dashboard Name
- Permissions

Choose:
Classic Dashboard

Click Create.

---

## 11. Adding Report to Dashboard

Click:
Add Panel
<img width="1363" height="654" alt="Screenshot 2026-02-21 at 10 39 47 PM" src="https://github.com/user-attachments/assets/dfe20cfb-1d97-4fbd-b6d8-f4656a1f9e36" />

Select:
New from Report

Choose your report.
<img width="1008" height="817" alt="Screenshot 2026-02-21 at 10 40 00 PM" src="https://github.com/user-attachments/assets/0f33785d-1aed-4649-8689-338aea413cb9" />

Now select visualization:
- Column Chart
- Pie Chart
- Line Chart
<img width="1365" height="660" alt="Screenshot 2026-02-21 at 10 40 41 PM" src="https://github.com/user-attachments/assets/7c478162-8c48-422c-865d-ffd4641869cd" />

Save dashboard ✅

Now data becomes visual.

---

## 12. Reports vs Dashboards

Report:
Saved search results.

Dashboard:
Visual collection of reports.

| Feature | Report | Dashboard |
|---------|--------|-----------|
| Saved Search | Yes | Uses Reports |
| Visualization | Limited | Yes |
| Quick Overview | No | Yes |
| Management View | No | Yes |

---

## 13. What is an Alert?

Alert = Automatic Warning

Splunk notifies when something suspicious happens.

Example:
If login attempts > 5
→ Possible brute force attack.

Splunk sends alert immediately.

---

## 14. Creating an Alert

Step 1:
Run search.

Example:
User login activity for Sarah.

Step 2:
Click Save As → Alert
<img width="1368" height="503" alt="Screenshot 2026-02-21 at 10 41 10 PM" src="https://github.com/user-attachments/assets/c8eb6ecf-936a-4877-aa9a-d4b040ff5731" />

---

## 15. Alert Settings

Important options:

Alert Type:
Scheduled → Runs after fixed time

Trigger Condition:
When alert should fire.

Example:
Number of results > 5

Meaning:
Alert if login count exceeds 5.
<img width="1104" height="803" alt="Screenshot 2026-02-21 at 10 41 38 PM" src="https://github.com/user-attachments/assets/2fcce785-8dfd-4066-8dcd-4a0024fe0a93" />

---

## 16. Throttle Option

Prevents too many alerts.

Example:
Send only ONE alert every 60 minutes.

This avoids:
Alert Fatigue (too many alerts).
<img width="1100" height="531" alt="Screenshot 2026-02-21 at 10 42 05 PM" src="https://github.com/user-attachments/assets/e91038f8-f847-4575-aba1-ae323f473b81" />

---

## 17. Trigger Actions

What happens after alert?

Possible actions:
- Send Email
- Run Script
- Notify SOC Team

Example:
Email sent to SOC team automatically.
<img width="467" height="457" alt="Screenshot 2026-02-21 at 10 42 18 PM" src="https://github.com/user-attachments/assets/75b1e2b1-8813-4b1b-aeaf-16505055132b" />
<img width="1089" height="741" alt="Screenshot 2026-02-21 at 10 42 40 PM" src="https://github.com/user-attachments/assets/d77c97e7-e662-471c-9c0f-ebc26623688d" />

---

## ⭐ Final Easy Understanding

Splunk Workflow:

Data → Search → Report → Dashboard → Alert

Search → Find data  
Report → Automate search  
Dashboard → Visualize data  
Alert → Immediate warning

---

✅ REPORT = Automation  
✅ DASHBOARD = Visualization  
✅ ALERT = Notification
