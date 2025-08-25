# 🔐 Bandit Level: Bandit21 ➝ Bandit22

## 📂 Command(s) used:
👉
- cd /etc/cron.d/
- ls
- cat cronjob_bandit22
- cat /usr/bin/cronjob_bandit22.sh
- cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
<img width="523" height="213" alt="Screenshot 2025-08-25 at 2 53 40 PM" src="https://github.com/user-attachments/assets/c0b667d8-b11f-46ae-b934-97001f6cb83d" />


## 📄 Password found:
👉 tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q

## 🧠 Notes / What I learned:
👉  ## 🔹 Command Used Explaination

### 1. Go to cron.d directory
``` bash
cd /etc/cron.d/
ls -l
```
👉 Here you will find system-wide cron job configuration files.  



### 2. Check Bandit22 cron job file
``` bash
cat cronjob_bandit22
```
Output:
``` bash
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
```
👉 This means the script runs at every reboot and every 1 minute.  



### 3. Inspect the script
``` bash
cat /usr/bin/cronjob_bandit22.sh
```
Output:
``` bash
#!/bin/bash
chmod 644 /tmp/t706lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t706lds9S0RqQh9aMcz6ShpAoZKF7fgv
```
👉 This script copies the **Bandit22 password** into a file inside `/tmp` every minute.  



### 4. Read the password file
``` bash
cat /tmp/t706lds9S0RqQh9aMcz6ShpAoZKF7fgv
```
👉 This will give you the **Bandit22 password**.  

### 5. Login to Bandit22
ssh bandit22@bandit.labs.overthewire.org -p 2220
---
# ⏰ Understanding Cron in Linux

## 🔹 What is Cron?
- `cron` is a **time-based job scheduler** in Unix/Linux.  
- It is used to automatically run scripts or commands at specified times and intervals.  
- Useful for repetitive tasks like backups, cleaning temporary files, monitoring, etc.  



## 🔹 Cron Jobs Location
1. **System-wide cron jobs:**  
   - Stored inside `/etc/cron.d/`, `/etc/crontab`, `/etc/cron.daily/`, `/etc/cron.hourly/` etc.  

2. **User-specific cron jobs:**  
   - Each user can set cron jobs using the `crontab -e` command.  
   - Stored in `/var/spool/cron/crontabs/<username>`.  



## 🔹 Cron Syntax
A cron job has 6 fields:

user command-to-execute
| | | | |
| | | | +---- Day of the week (0 - 6) (Sunday=0)
| | | +------ Month (1 - 12)
| | +-------- Day of the month (1 - 31)
| +---------- Hour (0 - 23)
+------------ Minute (0 - 59)

👉 Example:  
``` bash
30 2 * * * root /usr/bin/backup.sh
```
- Runs `/usr/bin/backup.sh` at **02:30 AM every day** as `root`.



## 🔹 Special Strings in Cron
Instead of numbers, cron supports shortcuts:  
``` bash
@reboot → Run once after reboot
@yearly → Run once a year (0 0 1 1 *)
@monthly → Run once a month (0 0 1 * *)
@weekly → Run once a week (0 0 * * 0)
@daily → Run once a day (0 0 * * *)
@hourly → Run once an hour (0 * * * *)
```
👉 Example:  
``` bash
@reboot root /usr/bin/startup_script.sh
```
- Runs the script once at every reboot.  



## 🔹 Redirecting Output
- To discard all output:
``` bash
command &> /dev/null
```
- To log output into a file:
``` bash 
command >> /var/log/cronjob.log 2>&1
```


## 🔹 Examples
1. Run a script every 5 minutes:  
``` bash
*/5 * * * * user /home/user/script.sh
```
2. Run a script at midnight:  
``` bash
0 0 * * * user /home/user/backup.sh
```
3. Clean `/tmp` every hour:  
``` bash
0 * * * * root find /tmp -type f -delete
```


## 🔹 Check Cron Jobs
- List current user’s cron jobs:  
``` bash
crontab -l
```
- Edit cron jobs:  
``` bash
crontab -e
```
- Check cron service status:  
``` bash
systemctl status cron
```


## ✅ Summary
- `cron` automates tasks based on time intervals.  
- Cron jobs can be system-wide or user-specific.  
- Syntax follows **minute, hour, day, month, weekday, command**.  
- Special strings like `@reboot`, `@daily` make scheduling easier.  


