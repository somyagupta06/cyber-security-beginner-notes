# 🧠 PowerShell Notes 

## 📌 What is PowerShell?

PowerShell is a tool in Windows used to:
- Run commands
- Automate tasks
- Investigate systems

👉 In SOC:
We use PowerShell to find suspicious activity and investigate attacks.

---

## ⚙️ What are Cmdlets?

Cmdlets are commands in PowerShell.

Format:
Verb-Noun

Examples:
Get-Process      -> shows running processes  
Get-Service      -> shows services  
Get-Command      -> shows all commands  

👉 Think like:
Verb = action  
Noun = object  

---

## 🔍 Important Commands (SOC Use)

### 1. Get-Command
Used to find available commands

Example:
Get-Command *process*

👉 SOC Use:
Helps understand what commands exist (useful in investigation)

---

### 2. Get-Help
Used to understand any command

Example:
Get-Help Get-Process  
Get-Help Get-Process -Examples  

👉 SOC Use:
If you see unknown command in logs, use this to understand it

---

## 🔗 Pipeline (|)

This connects commands

Example:
Get-Process | Where-Object

👉 Important:
PowerShell passes OBJECTS (not text)

👉 SOC Use:
Helps combine commands for investigation

---

## 🧬 Get-Member

Shows details of output (properties + methods)

Example:
Get-Process | Get-Member

👉 SOC Use:
Helps understand what data we can extract

---

## 🧱 Select-Object

Used to pick specific data

Example:
Get-Process | Select Name, Id

👉 SOC Use:
Used to create clean reports

---

## 🚨 Where-Object (VERY IMPORTANT)

Used to filter data

Example:
Get-Service | Where-Object {$_.Status -eq "Stopped"}

👉 SOC Use:
Used for detection and threat hunting

More examples:

Find high CPU process:
Get-Process | Where-Object {$_.CPU -gt 100}

Find specific process:
Get-Process | Where-Object {$_.Name -eq "powershell"}

---

## 📊 Sort-Object

Used to sort data

Example:
Get-Process | Sort-Object CPU -Descending

👉 SOC Use:
Find top suspicious processes quickly

---

## 🔍 File Searching (SOC Important)

Find a file:

Get-ChildItem -Path C:\Users\ -Recurse -Filter "file.txt" -ErrorAction SilentlyContinue

👉 SOC Use:
Used to find malware or suspicious files

---

## ⚠️ Common Suspicious PowerShell Activity

If you see these → be careful:

Invoke-Expression  
DownloadString  
EncodedCommand  
ExecutionPolicy Bypass  

👉 These are often used by attackers

---

## 🧠 Simple SOC Workflow

1. Check processes:
Get-Process  

2. Filter suspicious:
Get-Process | Where-Object {$_.CPU -gt 100}  

3. Sort:
Get-Process | Sort-Object CPU -Descending  

4. Select important data:
Get-Process | Select Name, CPU, Id  

---

## ✅ Quick Summary Table

| Command        | Use in SOC                |
|---------------|--------------------------|
| Get-Command   | Find commands            |
| Get-Help      | Understand commands      |
| Get-Process   | Check running processes  |
| Where-Object  | Filter suspicious data   |
| Select-Object | Extract important info   | 
| Sort-Object   | Analyze patterns         |

---

## 🎯 Final Tip

👉 PowerShell is very powerful  
👉 Attackers use it  
👉 SOC analysts must understand it to detect attacks  
---

# Lab Questions
## Q.1 What is the location of the file "interesting-file.txt"
<img width="1470" height="956" alt="Screenshot 2026-04-07 at 11 15 04 AM" src="https://github.com/user-attachments/assets/6ccc619d-4c35-4355-ab77-af597cdd0804" />
## Q.2 Specify the contents of this file
<img width="1470" height="956" alt="Screenshot 2026-04-07 at 11 17 52 AM" src="https://github.com/user-attachments/assets/b3390c89-69f8-4de4-97d1-306c9432589c" />
## Q.3 How many cmdlets are installed on the system(only cmdlets, not functions and aliases)?

<img width="570" height="159" alt="Screenshot 2026-04-07 at 11 23 22 AM" src="https://github.com/user-attachments/assets/c1f3ab99-2efb-4638-9a97-3d4667cbad66" />



## Q.4 Base64 decode the file b64.txt on Windows. 

<img width="1470" height="956" alt="Screenshot 2026-04-07 at 11 26 58 AM" src="https://github.com/user-attachments/assets/b5c61b11-29d0-4245-aa00-22b71ad2c11a" />

## Q. How many users are there on the machine?

- Get-LocalUser | Measure-Object

## Q. Which local user does this SID(S-1-5-21-1394777289-3961777894-1791813945-501) belong to?
<img width="1028" height="134" alt="Screenshot 2026-04-07 at 11 30 38 AM" src="https://github.com/user-attachments/assets/e5896208-b925-4936-8db8-9c8d4588c60b" />

## Q. How many users have their password required values set to False?
<img width="978" height="138" alt="Screenshot 2026-04-07 at 11 31 33 AM" src="https://github.com/user-attachments/assets/3e01c3d2-975e-43a1-9736-5e7c6b46f068" />

## Q. How many local groups exist?
<img width="636" height="155" alt="Screenshot 2026-04-07 at 11 32 27 AM" src="https://github.com/user-attachments/assets/198b76e1-ac08-42c1-b0ed-0897ce1a5720" />

## Q. What command did you use to get the IP address info?
- Get-NetIPAddress
## Q. How many ports are listed as listening?
- Get-NetTCPConnection -State Listen | Measure-Object
## Q. What is the remote address of the local port listening on port 445?
- Get-NetTCPConnection -LocalPort 445 | Select-Object RemoteAddress
## Q. How many patches have been applied?
- Get-HotFix | Measure-Object
## Q. When was the patch with ID KB4023834 installed?
- Get-HotFix -Id KB4023834
## Q. Find the contents of a backup file.

## Q. Search for all files containing API_KEY
- Get-ChildItem -Path C:\ -Recurse -ErrorAction SilentlyContinue | Select-String "API_KEY"
## Q. What command do you do to list all the running processes?
-  Get-Process
## Q. What is the path of the scheduled task called new-sched-task?
- Get-ScheduledTask -TaskName "new-sched-task"
## Q. Who is the owner of the C:\

- (Get-Acl C:\).Owner

## Q. What file contains the password?
<img width="1470" height="956" alt="Screenshot 2026-04-07 at 12 25 54 PM" src="https://github.com/user-attachments/assets/96418843-072d-40ed-a9d1-8f15b8aff3b9" />



## Q. What files contains an HTTPS link?

<img width="1134" height="114" alt="Screenshot 2026-04-07 at 12 26 49 PM" src="https://github.com/user-attachments/assets/4ec79442-2bb0-4a31-a432-5d64b04d17aa" />

---
# 🧠 PowerShell Notes (SOC - Quick Revision)

---

## 📂 File & Directory Commands

### Get current directory
Get-Location  
pwd  

👉 SOC Use: Know where you are working

---

### List files
Get-ChildItem  
dir  

👉 SOC Use: View files in system

---

### Read file content
Get-Content "file.txt"  
cat "file.txt"  

👉 SOC Use: Read logs, scripts, suspicious files

---

## 🔍 Searching & Investigation

### Search text inside files
Select-String "keyword" "path\*"

👉 Example:
Select-String "password" "C:\Users\Administrator\Desktop\emails\*"

👉 SOC Use: Find passwords, API keys, secrets

---

### Fix (important)
Get-ChildItem "path" -Recurse -File | Select-String "keyword"

👉 SOC Use: Avoid folder errors and search properly

---

## 👤 User Enumeration

### List users
Get-LocalUser  

### Count users
(Get-LocalUser).Count  

---

### Find user by SID
Get-LocalUser | Where-Object {$_.SID -eq "SID"}

👉 SOC Use: Identify users from logs

---

### Users with no password
Get-LocalUser | Where-Object {$_.PasswordRequired -eq $false}

---

## 👥 Group Enumeration

### List groups
Get-LocalGroup  

### Count groups
(Get-LocalGroup).Count  

---

## 🌐 Network Commands

### Get IP info
Get-NetIPAddress  

---

### Check listening ports
Get-NetTCPConnection -State Listen  

---

### Count listening ports
(Get-NetTCPConnection -State Listen).Count  

---

### Check specific port
Get-NetTCPConnection -LocalPort 445  

---

### Get remote address
Get-NetTCPConnection -LocalPort 445 | Select-Object RemoteAddress  

---

## ⚙️ System Info

### Get installed patches
Get-HotFix  

### Count patches
(Get-HotFix).Count  

---

### Find specific patch
Get-HotFix -Id KB4023834  

---

## 🔐 Hashing

### Get MD5 hash
Get-FileHash "file.txt" -Algorithm MD5  

👉 SOC Use: Verify file integrity / malware detection

---

## 🌍 Web Request

### Make web request
Invoke-WebRequest http://example.com  

👉 SOC Use: Detect downloads / attacker activity

---

## 🔎 File Searching

### Find file
Get-ChildItem -Path C:\ -Recurse -Filter "file.txt" -ErrorAction SilentlyContinue  

👉 SOC Use: Locate malware files

---

## 🧠 Scripting Basics

### Variable
$var = value  

---

### Loop
foreach($item in $list){
    command
}

---

### Condition
if(condition){
    command
}

---

## 🔌 Port Scanning

### Simple port scan
for($port=130; $port -le 140; $port++){
    Test-NetConnection 127.0.0.1 -Port $port
}

---

### Fast scan
130..140 | ForEach-Object {
    if(Test-NetConnection 127.0.0.1 -Port $_ -InformationLevel Quiet){
        $_
    }
}

---

### Count open ports
(130..140 | Where-Object {
    Test-NetConnection 127.0.0.1 -Port $_ -InformationLevel Quiet
}).Count  

---

## ⚠️ Suspicious PowerShell (SOC Alert)

Invoke-Expression  
DownloadString  
EncodedCommand  
ExecutionPolicy Bypass  

👉 SOC Use: Detect attacks

---

## 🎯 Final SOC Workflow

1. Check files → Get-ChildItem  
2. Read content → Get-Content  
3. Search keywords → Select-String  
4. Check users → Get-LocalUser  
5. Check network → Get-NetTCPConnection  
6. Verify files → Get-FileHash  

---

## 🧠 Final Tip

👉 PowerShell is used by:
- Admins (normal work)
- Attackers (abuse)

👉 SOC analyst must:
- Understand commands  
- Detect misuse  
- Investigate fast
---

# 🧠 PowerShell Scripting (SOC Notes - Easy Version)

## 📌 What is a Script?

A script = multiple commands written in one file

Example:
file.ps1

👉 Instead of typing commands again and again, we save them and run once

👉 SOC Use:
- Automation
- Faster investigation
- Repeating tasks easily

---

## 📦 Variables

Variables store data

Example:
$ports = 80,443,22

👉 SOC Use:
Store logs, ports, users, etc.

---

## 🔁 Loop (foreach)

Used to repeat work

Example:
foreach($port in $ports){
    echo $port
}

👉 SOC Use:
Check many items (ports, files, users)

---

## ❓ If Condition

Used to check something

Example:
if($port -eq 80){
    echo "HTTP Port"
}

👉 SOC Use:
Detect suspicious activity

---

## 🔍 Real Example (Port Checking Script)

Goal:
Check which ports are open

Steps:
1. Get system open ports  
2. Read ports from file  
3. Compare  

---

## 🧠 Port Scanner (Simple Script)

$ip = "127.0.0.1"

for($port=130; $port -le 140; $port++){

    $result = Test-NetConnection -ComputerName $ip -Port $port -InformationLevel Quiet

    if($result){
        echo "Port $port is OPEN"
    }

}

---

## ⚙️ Important Command

Test-NetConnection

👉 Checks:
- Port open or closed

---

## 📊 Logic (Very Simple)

For each port:
→ Try connection  
→ If success → open  
→ Else → closed  

---

## 🎯 SOC Use

- Find open ports
- Detect suspicious services
- Identify attack surface

---

## ⚡ Fast Method

130..140 | ForEach-Object {
    if(Test-NetConnection 127.0.0.1 -Port $_ -InformationLevel Quiet){
        $_
    }
}

---

## 🔢 Count Open Ports

(130..140 | Where-Object {
    Test-NetConnection 127.0.0.1 -Port $_ -InformationLevel Quiet
}).Count

---

## ⚠️ Important Concepts

| Concept | Meaning |
|--------|--------|
| Script | multiple commands |
| Variable | store data |
| Loop | repeat task |
| If | condition check |
| Test-NetConnection | check port |

---

## 🧠 Real SOC Thinking

👉 Attackers use ports to communicate  
👉 Analysts check open ports to find threats  

Example:
Unknown open port → possible malware  

---

## 🎯 Final Summary

- Scripts save time  
- Loops check multiple things  
- Conditions detect issues  
- PowerShell can replace tools like Nmap (basic level)  

👉 Learn scripting = strong SOC skill







