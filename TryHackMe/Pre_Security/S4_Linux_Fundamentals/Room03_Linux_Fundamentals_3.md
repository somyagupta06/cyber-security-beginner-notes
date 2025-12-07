# 📝 Linux Text Editing & File Transfer 

---

# ✏️ 1. Editing Files the Easy Way — NANO

So far, we used:
- `echo`
- `>`
- `>>`

But writing big files this way is painful.  
So Linux gives us **text editors** inside the terminal.

## ⭐ Nano (simple & beginner-friendly)

To open or create a file:
nano filename

Example:
nano myfile

Nano opens like this:

Hello TryHackMe
I can write things into "myfile"

### Nano shortcuts (super easy):

| Shortcut | Meaning |
|---------|----------|
| Ctrl + O | Save (Write Out) |
| Ctrl + X | Exit |
| Ctrl + W | Search |
| Ctrl + K | Cut |
| Ctrl + U | Paste |
| Ctrl + _ | Go to line |

Nano = perfect for beginners.

---

# 🖥️ 2. VIM (Advanced Editor)

VIM is more powerful but harder to learn.

### Why developers like VIM?

✔ Custom keyboard shortcuts  
✔ Works everywhere (even rescue mode)  
✔ Syntax highlighting  
✔ Tons of plugins  

TryHackMe has a full room for VIM.  
For now, just know it exists.

---

# 🌐 3. Downloading Files From Internet — WGET

`wget` lets you download files from a URL.

Example:
wget https://example.com/myfile.txt

This is like downloading a file with a browser… but in terminal.

---

# 🔁 4. Copy Files Between Computers — SCP (Secure Copy)

SCP = “secure cp command”  
It uses **SSH** to transfer files safely (encrypted).

Format:
scp SOURCE DESTINATION

## ⭐ Example 1: Copy file FROM your machine → TO remote machine

Given:
- remote IP: 192.168.1.30  
- remote user: ubuntu  
- local file: important.txt  
- remote filename: transferred.txt  

Command:
scp important.txt ubuntu@192.168.1.30:/home/ubuntu/transferred.txt
---

# 🌟 Linux Processes – Super Simple Notes (Easy English)

Think of your computer like a school.  
Every app or command you run is like a student in a class.  
Linux calls these **processes**.

Each process gets a **PID (Process ID)** which is like a roll number.

Example:  
Process number 60 → PID = 60

---

# 🔍 1. How to See Running Processes

## See your own processes
ps

This shows the small list of programs your current terminal is running.

## See all processes on the computer
ps aux

This shows:
- your processes  
- other users’ processes  
- system background processes  

Everything is visible here.

---

# 📊 2. Live Process Viewer

top

This updates every few seconds and shows:
- which process uses CPU the most
- RAM usage
- system load  
This is like a live scoreboard.

---

# ⚔️ 3. Stopping (Killing) a Process

If a program freezes, we can stop it using its PID.

kill 1337

This will stop the process with PID 1337.

## Important signals (easy explanation)
| Signal | Meaning |
|--------|---------|
| SIGTERM | Stop nicely (finish small tasks first) |
| SIGKILL | Force stop immediately (no cleanup) |
| SIGSTOP | Pause the process without killing it |

---

# 🚀 4. How Processes Start 

When your computer starts:
1. The first special process begins (PID 0)
2. Then **systemd** starts (PID 1) → this is like the “manager”
3. Every other process is created under systemd

So systemd is like the “boss” of all processes.

---

# 🔧 5. Starting and Stopping Services (systemctl)

Some programs should always run in the background, like:
- web servers
- database servers

You can control them using systemctl:

systemctl start apache2  
systemctl stop apache2  
systemctl enable apache2  
systemctl disable apache2  

Meaning:
- start → run it now  
- stop → stop it now  
- enable → run when the computer boots  
- disable → don’t run at boot  

---

# 🔄 6. Foreground vs Background

## Foreground Process
Runs normally and shows output immediately.
Example:
echo Hi THM

## Background Process
If you add "&" at the end:

echo Hi THM &

Now the process runs silently in the background and the terminal stays free.

Good for:
- long scripts  
- big file copies  
- tasks that take time  

---

# ⏸ 7. Pause a Process (Ctrl + Z)

If a script keeps printing messages and you want to stop it temporarily:

Press Ctrl + Z

This pauses it and frees your terminal.

---

# 🔙 8. Bring a Process Back (fg)

To continue the paused process again:

fg

If there are multiple paused jobs:

fg %1  
fg %2  

---

# 📝 Quick Summary Table

| Topic | Command | Simple Meaning |
|-------|---------|----------------|
| View processes | ps | Show your processes |
| View all processes | ps aux | Show every process |
| Live monitor | top | Real-time update |
| Kill a process | kill PID | Stop a program |
| Pause | Ctrl + Z | Stop temporarily |
| Background run | command & | Run silently |
| Foreground | fg | Continue running |
| Start service | systemctl start | Run a service |
| Stop service | systemctl stop | Stop a service |
| Auto start | systemctl enable | Start at boot |

---
# 🌟 Linux Crontab, Packages, and Logs — Easy English Notes

# 🕒 1. What is Cron and Crontab?

Cron = a background worker in Linux that runs tasks on a schedule.  
Think of it like an alarm clock that runs commands for you.

Crontab = a special file where you WRITE the schedule.

Cron reads the crontab and runs each line at the time you set.

---

# 📅 2. Crontab Format (Very Easy)

A crontab line has 6 parts:

MIN  → minute  
HOUR → hour  
DOM  → day of month  
MON  → month  
DOW  → day of week  
CMD  → command to run  

Example meaning:  
If you want something to run at 2:30 PM every day:

30 14 * * * command

---

# ⭐ Example: Backup Documents Every 12 Hours

0 */12 * * * cp -R /home/cmnatic/Documents /var/backups/

Explanation:  
- 0 → minute 0  
- */12 → every 12 hours  
- * * * → we don’t care about day, month, or weekday  
- cp command → copy the Documents folder  

Asterisk (*) = “any value / I don’t care”

---

# ✏️ 3. Editing Your Crontab

To open and edit your schedule:

crontab -e

This opens nano, where you can add or remove cron jobs.

---

# 📦 4. What Are Packages and Repositories?

Package = a software (like Chrome, VLC, Python).

Repository (repo) = a storehouse on the internet where Linux gets software from.

Ubuntu uses **apt** to install software.

Examples:
apt install nmap  
apt remove nmap  
apt update  
apt upgrade  

---

# 🛠 5. Adding a New Repository (Easy Explanation)

Sometimes a software is NOT available in Ubuntu's default repo.  
Then you need to add a new repo.

Repos also use **GPG keys** — these are like signatures to prove the software is safe.

### Steps (Simple Flow):
1. Add the GPG key → computer learns to trust that software.
2. Add repo to sources list → computer knows where to download from.
3. Run apt update → refresh repo list.
4. Install the software → apt install software-name.

Example: Adding Sublime Text repo  
(You don’t need to run these on TryHackMe because internet is blocked)

Download and trust the GPG key:
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -

Create a repo file:
sudo nano /etc/apt/sources.list.d/sublime-text.list

Put this inside the file:
deb https://download.sublimetext.com/ apt/stable/

Update your package list:
sudo apt update

Install Sublime Text:
sudo apt install sublime-text

To remove a repo:
sudo add-apt-repository --remove ppa:Name/ppa

To remove software:
sudo apt remove sublime-text

---

# 📝 6. Logs in Linux (Easy Explanation)

Linux stores logs (activity records) in:

/var/log

Logs are like CCTV footage for your system.

They help you:
- see errors  
- see who accessed what  
- check attacks  
- troubleshoot problems  

Examples of logs:
- apache2 → website logs  
- ufw → firewall logs  
- fail2ban → brute force protection logs  

Two very important logs for web servers:

1. access.log → every visitor request  
2. error.log → errors on the website  

Linux automatically rotates logs so they don’t get too big.

---

# 🎯 Quick Summary Table

| Topic | Meaning |
|-------|---------|
| Cron | Runs scheduled tasks |
| Crontab | File that stores cron jobs |
| * | “any value” wildcard |
| apt install | Install software |
| apt update | Refresh package list |
| apt upgrade | Update installed software |
| Repository | Online software store |
| GPG key | Safety signature |
| Logs | System activity records |
| /var/log | Folder containing logs |

---

