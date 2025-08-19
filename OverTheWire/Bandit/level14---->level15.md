# 🔐 Bandit Level: Bandit14 ➝ Bandit15



### 📂 Command(s) used: 

👉 
- nc 127.0.0.1 30000                 (IP address of your localhost)
-  MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS   (password)
     
<img width="383" height="117" alt="Screenshot 2025-08-17 at 9 50 18 PM" src="https://github.com/user-attachments/assets/9f1fb6f9-43b3-4d35-9512-1e8d34504c5d" />

  


---
   
### 📄 Password found:


👉

8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo




---

## 🧠 Notes / What I learned:
👉 
## nc (Netcat)

nc IP-address 30000

**Where:**
- nc: Netcat
- **IP-address**: ip address of your localhost
- **30000**: port number

---

# Netcat (nc) Notes

Netcat (`nc`) is called the **Swiss Army Knife of Networking**.  
It is used for connecting to ports, sending/receiving data, chatting, file transfer, and even port scanning.



## 🔹 Basic Syntax
``` bash
nc [options] <IP-address> <Port>
```


## 🔹 Modes of Netcat

### 1. Server Mode (Listening)
Netcat can act like a server and wait for connections.
``` bash
nc -l -p 30000
```
- `-l` → listen mode  
- `-p 30000` → listen on port `30000`  



### 2. Client Mode (Connecting)
Netcat can also act like a client and connect to a server.
``` bash
nc 127.0.0.1 30000
```
This connects to **localhost** on port `30000`.



## 🔹 Sending Text (Password Example)
If you want to send a password or text to a port:
``` bash
echo "mypassword" | nc <IP-address> 30000
```
Example:
``` bash
echo "letmein123" | nc 10.10.10.5 30000
```


## 🔹 Interactive Chat Example

**Terminal 1 (Server):**
``` bash
nc -l -p 30000
```
**Terminal 2 (Client):**
``` bash
nc 127.0.0.1 30000
```
Now both sides can type and chat with each other.



## 🔹 File Transfer with Netcat

**Send a file:**
``` bash
nc <IP-address> 30000 < file.txt
```
**Receive a file:**
``` bash
nc -l -p 30000 > file.txt
```


## 🔹 Port Scanning with Netcat
``` bash
nc -zv <IP-address> 20-1000
```
- `-z` = scan only (don’t send data)  
- `-v` = verbose (show details)  



## 🔹 Quick Comparison Table

| Purpose            | Command Example                        |
|--------------------|----------------------------------------|
| Listen (Server)    | `nc -l -p 30000`                       |
| Connect (Client)   | `nc 127.0.0.1 30000`                   |
| Send Text          | `echo "mypassword" | nc 127.0.0.1 30000` |
| Send File          | `nc <IP> 30000 < file.txt`             |
| Receive File       | `nc -l -p 30000 > file.txt`            |
| Port Scan          | `nc -zv <IP> 20-1000`                  |



## 🔹 Key Points to Remember
- Netcat works on both **TCP** and **UDP**.  
- You can use it for **debugging services**, **CTF challenges**, and **simple file transfer**.  
- Always use it **legally and responsibly**.
