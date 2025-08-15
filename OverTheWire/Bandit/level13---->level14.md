# 🔐 Bandit Level: Bandit13 ➝ Bandit14



### 📂 Command(s) used: 

👉 
        
   - Ls        (to get the bandit13 level directories) 
   - ssh -i sshkey.private bandit14@bandit.labs.overthewire.labs -p 2220
   - Cat  /etc/bandit_pass/bandit14 
  <img width="692" height="326" alt="Screenshot 2025-08-15 at 10 35 02 PM" src="https://github.com/user-attachments/assets/89347204-d19d-416f-ae45-f85a38edd66a" />
  <img width="388" height="53" alt="Screenshot 2025-08-15 at 10 36 00 PM" src="https://github.com/user-attachments/assets/bec5e34a-2db0-4835-b964-5d740a6bc921" />


---
   
### 📄 Password found:


👉

  MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS

---

## 🧠 Notes / What I learned:
👉 
# Bandit Level 13 → 14 Walkthrough

## Password File Location
The password is stored at:
/etc/bandit_pass/bandit14
- This file contains the password for Level 14.  
- However, only the user **bandit14** can read it.  
- You are currently logged in as **bandit13**, so you cannot read it directly.  



## The Trick
- In this level, you won’t retrieve the password directly.  
- You are given a **private SSH key** (the one you used with `ssh -i sshkey.private`).  
- You’ll use this key to log in as **bandit14**.  



## Important Note
- `"localhost"` means the **same machine** you are currently on.  
- This means you will connect to `bandit14@localhost` — logging in internally, not from another server.  

---

## Step-by-Step Solution
You are currently logged in as **bandit13@bandit**.  

1. Log in as `bandit14` using the given private key:
    ```bash
    ssh -i sshkey.private bandit14@localhost
    ```

2. Once logged in as `bandit14`, read the password file:
    ```bash
    cat /etc/bandit_pass/bandit14
    ```


3. Yahan:
- -i sshkey.private → tumhari private key ka path
- -p 2220 → correct port
- bandit14@bandit.labs.overthewire.org → username + host

4. This will output the password for **Level 14**.
5. Extra Note
- If still permission is denied , use:
  
chmod 600 sshkey.private
- Try tco connect again.
---
# SSH Public–Private Key Authentication Guide

## 1. Public and Private Key Concept
- Logging in with a **password** is less secure because someone could brute-force or guess it.  
- **Public–Private key login** is more secure.  
- It uses **two keys**:  
  - **Private Key** → Stays on your computer, must be kept secret, never share it.  
  - **Public Key** → Stored on the server (in the `.ssh/authorized_keys` file).  

**How it works:**
1. When you try to log in, the server locks a message using your public key.  
2. Only your private key can unlock it.  
3. Even if a hacker intercepts the data, they can’t read it without the private key.  



## 2. Benefits of Key-Based SSH Login
- Passwords can be guessed; breaking a private key is extremely difficult (especially if the key is long).  
- Always use **RSA keys** (DSA is no longer considered secure).  
- **RSA** = Rivest–Shamir–Adleman (algorithm name).  
- With key-based login, you don’t need a password — the login works if the keys match.  



## 3. How to Create RSA Keys (On the Client Side)
Run these commands in the terminal:
```bash
mkdir ~/.ssh
chmod 700 ~/.ssh
ssh-keygen -t rsa

```
- Location prompt → Press Enter for default.
- Passphrase prompt → Adds extra security (in case your computer is stolen).
- Generated files:
- id_rsa → Private Key (keep it secret).
- id_rsa.pub → Public Key (shareable).
## 4. About Passphrases
- A passphrase is like a password for your key.
- If your key is stolen, the passphrase gives you time to disable the old key and create a new one.
- Use a strong passphrase (store in a password manager).
- For automatic login, leave the passphrase empty.
## 5. Key Size (Encryption Strength)
- Default: 2048 bits (good enough).
- For more security, use 4096 bits:
``` bash
ssh-keygen -t rsa -b 4096
```
## 6. How to Send the Public Key to the Server
If you can log in with a password:
``` bash
ssh-copy-id username@host
```
Where:
- username = your server username
- host = server’s IP or domain
**Manual method:**
1. Copy the contents of id_rsa.pub.
2. Paste it into the server’s .ssh/authorized_keys file.
## 7. Troubleshooting (If It Doesn’t Work)
Check sshd_config on the server:
``` bash
PubkeyAuthentication yes
RSAAuthentication yes
```
**Permissions:**
```bash
chmod go-w ~/
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```
**Common error:**

Agent admitted failure to sign using the key.
**Fix:**
```basg
ssh-add
```
**Debugging:**
``` bash
ssh -v username@host
```
## 8. Extra Control (Optional)
- You can restrict a public key to run only a specific command.
- You can disable port forwarding for that key.
## 📌 Summary
- Generate RSA keys on your computer.
- Put the public key in the server’s .ssh/authorized_keys.
- Keep the private key safe (passphrase optional).
- Ensure PubkeyAuthentication is enabled on the server.
- Set correct file permissions.
- Now you can log in without a password, using only your key.
