# 🔐 Bandit Level: Bandit18 ➝ Bandit19

## 📂 Command(s) used:
👉 ssh bandit18@bandit.labs.overthewire.org -p 2220 "cat readme"
<img width="567" height="231" alt="Screenshot 2025-08-22 at 4 17 39 PM" src="https://github.com/user-attachments/assets/74d255c6-42c9-4659-9937-a424e009fd7b" />

## 📄 Password found:
👉 cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8

## 🧠 Notes / What I learned:
👉  
## Command Used (Explain)
- **ssh bandit18@bandit.labs.overthewire.org -p 2220 "cat readme"**
- 
**Other Commands**
- SSH with no profile and no rc:  
ssh -p 2220 bandit18@bandit.labs.overthewire.org "bash --noprofile --norc"  

- Or simply use:  
ssh -p 2220 bandit18@bandit.labs.overthewire.org /bin/sh  

- Once inside, read the password file:  
cat readme  

👉 This reveals the Bandit19 password.
---
## 📒 Notes on .bashrc, SSH & Bandit Example

---

### 🔹 .bashrc Basics
- Location → /home/username/.bashrc  
- Purpose → Every time a new interactive shell starts (local terminal or SSH login), this file runs.  
- Use → You can add custom aliases, prompt changes, ssh-agent setup, shortcuts, etc.  

---

### 🔹 SSH-related .bashrc Modifications

1. **Custom prompt for SSH sessions**
``` bash
if [ -n "$SSH_CONNECTION" ]; then  
    PS1='[\u@\h-SSH \W]\$ '  
fi
```
👉 Helps avoid confusion between local and remote sessions.  

2. **Auto-load SSH key**
``` bash
if [ -z "$SSH_AUTH_SOCK" ]; then  
    eval "$(ssh-agent -s)" > /dev/null  
    ssh-add ~/.ssh/id_rsa 2>/dev/null  
fi
``` 
👉 No need to manually add keys after every login.  

3. **Shortcut alias**
``` bash
alias sshhome='ssh user@192.168.1.10'  
alias sshwork='ssh user@work.server.com'
``` 
👉 Connect in one simple command.  

4. **Show warning/notification on SSH login**
``` bash
if [ -n "$SSH_CONNECTION" ]; then  
    echo "⚠️ You are logged in to $(hostname) on $(date)"  
fi  
```
5. **Set resource limits**
``` bash
if [ -n "$SSH_CONNECTION" ]; then  
    ulimit -u 1000  
    ulimit -n 1024  
fi
```
👉 Limit maximum processes/files for remote users.  

6. **Maintain SSH login logs**
``` bash
if [ -n "$SSH_CONNECTION" ]; then  
    echo "$(date): SSH login from $SSH_CLIENT" >> ~/.ssh/ssh_login.log  
fi  
```
---

### 🔹 Force Logout via .bashrc
If you want to immediately logout any SSH user:  
``` bash
if [ -n "$SSH_CONNECTION" ]; then  
    echo "❌ SSH access denied for $(whoami)"  
    exit  
fi  
```
---

### 🔹 Permanent Restriction (using sshd_config)
The `.bashrc` trick can be bypassed (non-interactive shells, scp, etc).  
Secure method → Modify `/etc/ssh/sshd_config`.  

1. **Deny a user**  
``` bash
DenyUsers username  
```
2. **Deny a group**  
``` bash
DenyGroups groupname  
```
3. **Allow only specific users**  
``` bash
AllowUsers user1 user2  
```
Apply changes:  
``` bash
sudo systemctl restart ssh  
```
---

### 🔹 .bashrc Bypass Methods (why it’s not secure)

- Run direct command via SSH:  
``` bash
ssh user@host "ls -l"  
```
- Run a different shell:  
``` bash
ssh user@host /bin/sh  
```
- Use scp / sftp for file transfer.  

- Change default shell (if permitted):  
``` bash
chsh -s /bin/sh  
```
👉 That’s why **real security should always be done with sshd_config**, not just `.bashrc`.  

---

 

  



### ✅ Final Takeaways
- `.bashrc` = customization file that runs at login.  
- You can use it to set prompt, aliases, limits, and warnings for SSH users.  
- `.bashrc` can force logout, but it can be bypassed.  
- For real security, always configure restrictions in `sshd_config`.  
- The Bandit challenge is a practical example of bypassing `.bashrc` restrictions.  
