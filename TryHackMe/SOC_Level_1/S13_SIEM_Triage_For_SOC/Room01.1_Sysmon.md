# Sysmon — Simple Notes (SOC Analyst View)

## 1. What is Sysmon?

Sysmon is a tool from Microsoft that watches what is happening inside a Windows computer.

Think of Sysmon like a **security camera for a computer**.

It records important activities such as:

- When a program starts
- When a program connects to the internet
- When a file is created
- When the registry changes
- When a DNS request happens

These activities are saved as **logs**.

SOC analysts read these logs to detect **hackers or malware**.

---

## 2. Where are Sysmon Logs Stored?

Sysmon logs are stored in Windows Event Viewer.

Location:

Applications and Services Logs  
Microsoft  
Windows  
Sysmon  
Operational

SOC analysts usually send these logs to a **SIEM tool** like Splunk to search and analyze them.

---

## 3. What is a Sysmon Configuration File?

Sysmon needs a **config file** to decide what it should log.

Without a config file:
- Too many logs will be created
- Many logs will be normal system activity

So the config file tells Sysmon:

- what to monitor
- what to ignore

Example idea:

Normal activity → ignore  
Suspicious activity → log

This helps SOC analysts focus only on **important security events**.

---

## 4. Two Types of Rules in Sysmon

### Exclude Rules

Exclude rules remove normal activity.

Example idea:

svchost.exe runs many times in Windows.  
So SOC teams often exclude it.

Result:

Less noise  
Easier investigation

---

### Include Rules

Include rules log only suspicious activity.

Example idea:

If nmap.exe runs → log it

Why?

Attackers often use Nmap for scanning.

---

## 5. Important Sysmon Event IDs (SOC Analyst Must Know)

SOC analysts mainly monitor a few important events.

| Event ID | What It Detects | Why It Matters |
|--------|--------|--------|
| 1 | Process Creation | Detect suspicious programs |
| 3 | Network Connection | Detect suspicious connections |
| 7 | DLL Loaded | Detect DLL injection |
| 8 | Create Remote Thread | Detect process injection |
| 11 | File Creation | Detect malware dropping files |
| 13 | Registry Change | Detect persistence |
| 15 | Alternate Data Stream | Detect hidden malware |
| 22 | DNS Query | Detect malicious domains |

---

## 6. Event ID 1 — Process Creation

This event records when a program starts.

Example processes:

powershell.exe  
cmd.exe  
notepad.exe  

SOC analysts check:

- strange program names
- suspicious command lines
- encoded PowerShell commands

Example suspicious activity:

powershell.exe running a long encoded command

This may mean **malware execution**.

---

## 7. Event ID 3 — Network Connection

This event records when a program connects to another computer or the internet.

Example:

powershell.exe connects to an external IP address.

SOC analysts investigate if:

- an unknown process connects to the internet
- a suspicious port is used

Example suspicious port:

4444

This port is often used by hacking tools.

---

## 8. Event ID 7 — Image Loaded (DLL Monitoring)

This event records when a process loads a DLL file.

Attackers sometimes use **DLL injection** to hide malware inside normal processes.

Example suspicious situation:

A DLL loads from the Temp folder.

Why is this strange?

Normal programs usually do not load DLLs from Temp.

SOC analysts investigate this.

---

## 9. Event ID 8 — Create Remote Thread

This event detects when one process injects code into another process.

Example attack:

malware.exe injects code into explorer.exe

Why attackers do this:

To hide malware inside a trusted process.

SOC analysts treat this event as **very suspicious**.

---

## 10. Event ID 11 — File Creation

This event records when a file is created.

SOC analysts look for:

- suspicious executable files
- ransomware files
- scripts dropped by malware

Example:

HELP_TO_SAVE_FILES.txt

This file name is commonly used by ransomware.

---

## 11. Event ID 13 — Registry Change

This event detects changes in the Windows registry.

Attackers often modify the registry to create **persistence**.

Persistence means:

Malware runs automatically every time the computer starts.

Example location:

Software  
Microsoft  
Windows  
CurrentVersion  
Run

If malware adds itself here, it will run at every startup.

---

## 12. Event ID 15 — Alternate Data Streams

Attackers sometimes hide malware inside files using **Alternate Data Streams (ADS)**.

Example idea:

file.txt contains hidden malware.

This makes detection harder.

SOC analysts check these logs to find hidden files.

---

## 13. Event ID 22 — DNS Queries

This event records DNS requests.

Example:

A computer asks for the IP of a domain.

Example domain:

example.com

SOC analysts investigate if a system tries to contact:

- suspicious domains
- newly created domains
- malware command-and-control servers

---

## 14. Simple Attack Example (Using Sysmon Logs)

Step 1  
Malware starts → Event ID 1

Step 2  
Malware connects to attacker server → Event ID 3

Step 3  
Malware drops a file → Event ID 11

Step 4  
Malware modifies registry → Event ID 13

Now SOC analysts can see the **whole attack chain**.

---

## 15. Key Idea for SOC Analysts

Sysmon helps analysts answer questions like:

- What program started?
- What process made a network connection?
- What file was created?
- Did the registry change?
- Did the system contact a suspicious domain?

By analyzing these logs, SOC analysts can detect attacks and investigate incidents.

---

## Quick Revision

Sysmon is a monitoring tool for Windows.

It records detailed logs about system activity.

SOC analysts use Sysmon logs to detect:

- malware execution
- suspicious network connections
- file drops
- registry persistence
- hidden malware
- malicious domains
---

# Sysmon Threat Hunting — Simple SOC Notes

## 1. Malicious Activity Monitoring

When Sysmon is properly configured, most normal system activity is filtered out.

Normal activity is called **noise**.

Noise examples:
- normal Windows processes
- normal internet connections
- background system services

If noise is removed, SOC analysts can focus on **important suspicious events**.

This makes threat detection faster.

---

## 2. What SOC Analysts Try to Detect

Using Sysmon logs, SOC analysts try to detect common attacker activities.

| Threat | Meaning |
|---|---|
| Ransomware | Malware that encrypts files and asks for money |
| Persistence | Malware that stays active after reboot |
| Mimikatz | Tool used to steal passwords |
| Metasploit | Framework used by attackers |
| Command and Control (C2) | Infected system communicating with attacker |

Even though only a few threats are shown, the **detection method is usually the same**.

SOC analysts analyze logs and look for suspicious behavior.

---

## 3. Sysmon Best Practices

When SOC teams deploy Sysmon, they follow some important rules.

---

### Best Practice 1 — Exclude More Than Include

When creating Sysmon rules, it is better to **exclude normal events** instead of only including suspicious events.

Reason:

If you only include certain events, you might **miss important malicious activity**.

Example idea:

Exclude normal processes like:

explorer.exe  
svchost.exe  
chrome.exe  

This removes noise and helps analysts focus on suspicious logs.

---

### Best Practice 2 — Command Line Tools Give Better Control

Logs can be analyzed in two ways.

| Method | Description |
|---|---|
| Event Viewer | Graphical interface |
| PowerShell CLI | Command line filtering |

SOC analysts prefer command line tools because they allow **better filtering and automation**.

Common tools:

Get-WinEvent  
wevtutil.exe  

These tools help analysts search logs quickly.

---

### Best Practice 3 — Know Your Environment

Before monitoring a system, SOC analysts must understand what is **normal behavior**.

Example:

Normal activity:
- outlook.exe connecting to internet
- chrome.exe browsing websites

Suspicious activity:
- powershell.exe making unknown internet connections
- unknown programs contacting external servers

Knowing the environment helps analysts detect anomalies.

---

## 4. Filtering Events Using Event Viewer

Event Viewer allows basic filtering of logs.

Main filters available:

- Event ID
- Keywords
- Time range

Example:

If an analyst wants to see **network connections**, they filter:

Event ID = 3

This shows only network connection events.

Event Viewer can also filter using XML queries, but this process is slow and not ideal for large investigations.

---

## 5. Filtering Events Using PowerShell

SOC analysts often use PowerShell to filter logs because it provides more control.

The main command used is:

Get-WinEvent

This command reads Windows event logs.

PowerShell can filter logs using **XPath queries**.

---

## 6. Basic XPath Filters

Sysmon logs are stored in XML format.

XPath is used to search specific fields inside those logs.

Common filters:

### Filter by Event ID
*/System/EventID=3

Meaning:

Show only events where Event ID equals 3.

---

### Filter by XML Attribute Name
*/EventData/Data[@Name="DestinationPort"]

Meaning:

Search the field called DestinationPort.

---

### Filter by Event Data
*/EventData/Data=4444

Meaning:

Show logs where the value is 4444.

---

## 7. Example Threat Hunting Query

SOC analysts often combine filters to search for suspicious activity.

Example query:

Get-WinEvent -Path C:\Users\THM-Analyst\Desktop\Scenarios\Practice\Hunting_Metasploit.evtx -FilterXPath '*/System/EventID=3 and */EventData/Data[@Name="DestinationPort"] and */EventData/Data=4444'

This query searches for:

- Event ID 3 (network connections)
- Destination port 4444

Port 4444 is commonly associated with attacker tools.

If this appears in logs, it may indicate malicious activity.

---

## 8. Example Log Output

When the command runs, output may look like this:

ProviderName: Microsoft-Windows-Sysmon

TimeCreated: 1/5/2021  
Id: 3  
Message: Network connection detected

This means a process created a network connection.

SOC analysts then investigate:

- which process created the connection
- which IP address it connected to
- which port was used

---

## 9. Example Attack Detection Timeline

Sysmon logs help analysts reconstruct attacker behavior.

Example timeline:

Step 1  
Process starts  
Event ID 1

Step 2  
Process connects to attacker server  
Event ID 3

Step 3  
Malware creates a file  
Event ID 11

Step 4  
Registry modified for persistence  
Event ID 13

Using these logs, SOC analysts can understand the **entire attack chain**.

---

## Quick Revision

Sysmon records detailed system activity logs.

SOC analysts use these logs to detect threats such as:

- ransomware
- password stealing tools
- suspicious network connections
- persistence mechanisms
- command and control communication

PowerShell tools like Get-WinEvent allow analysts to filter logs quickly and investigate suspicious activity.
---












# Lab Questions
## Q.1 What is the full registry key of the USB device calling svchost.exe in Investigation 1? 
<img width="1470" height="956" alt="Screenshot 2026-03-09 at 5 26 14 PM" src="https://github.com/user-attachments/assets/864a3fd9-e58b-4fb5-9ea5-e1b21698cc44" />
## Q.2 What is the device name when being called by RawAccessRead in Investigation 1?
<img width="1470" height="956" alt="Screenshot 2026-03-09 at 5 28 02 PM" src="https://github.com/user-attachments/assets/6254c31c-2d40-462b-a770-1fecffbca89e" />
## Q.3 What is the first exe the process executes in Investigation 1?
<img width="1470" height="956" alt="Screenshot 2026-03-09 at 5 32 46 PM" src="https://github.com/user-attachments/assets/1694056e-d349-4624-9c29-4bdefea868bc" />
## Q.4 What is the full path of the payload in Investigation 2?
<img width="1470" height="956" alt="Screenshot 2026-03-09 at 5 41 16 PM" src="https://github.com/user-attachments/assets/db848bf3-e6ce-4196-b284-3e4fb55a22c7" />
## Q.5 What is the full path of the file the payload masked itself as in Investigation 2?
<img width="1470" height="956" alt="Screenshot 2026-03-09 at 5 42 12 PM" src="https://github.com/user-attachments/assets/50a6b122-64af-4bb7-b49f-1a05c1c91086" />
## Q.6 What signed binary executed the payload in Investigation 2?
<img width="1470" height="956" alt="Screenshot 2026-03-09 at 5 43 57 PM" src="https://github.com/user-attachments/assets/eb58928c-1d32-4e8e-93ec-c4e509fd2f74" />
## Q.7 What is the IP of the adversary in Investigation 2?
<img width="1470" height="956" alt="Screenshot 2026-03-09 at 5 44 35 PM" src="https://github.com/user-attachments/assets/1a4d064f-edaf-4f6e-8ba2-b8b5692b2550" />
## Q.8 What back connect port is used in Investigation 2?
<img width="1470" height="956" alt="Screenshot 2026-03-09 at 5 45 06 PM" src="https://github.com/user-attachments/assets/96a24210-96b3-406e-b41e-c51df0500b9d" />
## Q.9 What is the IP of the suspected adversary in Investigation 3.1?
<img width="1470" height="956" alt="Screenshot 2026-03-09 at 5 48 36 PM" src="https://github.com/user-attachments/assets/e1c126f6-5d34-4f1c-b827-b06a3813d198" />
## Q.10 What is the hostname of the affected endpoint in Investigation 3.1?
<img width="1470" height="956" alt="Screenshot 2026-03-09 at 5 52 00 PM" src="https://github.com/user-attachments/assets/c151d5e7-ab2b-473b-b3e2-06d3318cb4cd" />
## Q.11 What is the hostname of the C2 server connecting to the endpoint in Investigation 3.1?
<img width="1470" height="956" alt="Screenshot 2026-03-09 at 5 53 41 PM" src="https://github.com/user-attachments/assets/c466d669-8a00-4118-b1c7-42b22ac998ec" />
## Q.12 Where in the registry was the payload stored in Investigation 3.1?
<img width="1470" height="956" alt="Screenshot 2026-03-09 at 5 55 19 PM" src="https://github.com/user-attachments/assets/2c9052dc-f772-4394-98a4-965c1f92f179" />
## Q.13 What PowerShell launch code was used to launch the payload in Investigation 3.1?
<img width="1470" height="956" alt="Screenshot 2026-03-09 at 5 57 40 PM" src="https://github.com/user-attachments/assets/00a2faa5-c820-4d49-a0dd-2c25c74cf022" />
## Q.14 What is the IP of the adversary in Investigation 3.2?
<img width="1470" height="956" alt="Screenshot 2026-03-09 at 5 59 17 PM" src="https://github.com/user-attachments/assets/59761619-432c-4ae5-ba49-3a32cb2c1a62" />
## Q.15 What is the full path of the payload location in Investigation 3.2?
<img width="1470" height="956" alt="Screenshot 2026-03-09 at 6 03 34 PM" src="https://github.com/user-attachments/assets/cd8d642a-795e-4c11-8013-4599fbbdefb5" />
## Q.16 What was the full command used to create the scheduled task in Investigation 3.2?
<img width="1470" height="956" alt="Screenshot 2026-03-09 at 6 13 30 PM" src="https://github.com/user-attachments/assets/8a6c1659-171b-4356-85c1-e18f4079f078" />
## Q.17 What process was accessed by schtasks.exe that would be considered suspicious behavior in Investigation 3.2?

## Q.18 What is the IP of the adversary in Investigation 4?
## Q.19 What port is the adversary operating on in Investigation 4?
<img width="1470" height="956" alt="Screenshot 2026-03-09 at 6 19 50 PM" src="https://github.com/user-attachments/assets/4a318c2b-5354-4bfb-ba59-6b5761fafec2" />
## Q.20 What C2 is the adversary utilizing in Investigation 4?
<img width="1470" height="956" alt="Screenshot 2026-03-09 at 6 21 26 PM" src="https://github.com/user-attachments/assets/81100709-376e-496a-b7a4-1191ad837c7e" />

