# 🔐 Bandit Level: Bandit16 ➝ Bandit17
## 📂 Command(s) used:
👉 
- nmap -p 30000-32000 127.0.0.1
- nmap -p PORT --script ssl-enum-ciphers 127.0.0.1
- cat /etc/bandit_pass/bandit16
- ncat --ssl localhost 31790
- password
- mkdir /tmp/le17
- cd /tmp/le17
- nano private.key
- paste the RSA key
- chmod 400 private.key
- ls -l
- ssh -i private.key bandit17@localhost -p 2220
- cat /etc/bandit_pass/bandit17


## 📄 Password found:
👉 
EReVavePLFHtFlFsjn3hyzMlvSuSAcRD

## 🧠 Notes / What I learned:
👉  
## Command Used (Explaination)
1. **nmap -p 30000-32000 127.0.0.1**
- Scans all ports from 30000 to 32000 on the local machine (127.0.0.1).
- **Goal** → find which ports are open.
2. **nmap -p PORT --script ssl-enum-ciphers 127.0.0.1**
- Runs a script on the found ports to check if they are running SSL/TLS.
- **Goal** → identify which open port supports SSL/TLS.
3. **cat /etc/bandit_pass/bandit16**
- Displays the current password for bandit16.
- You will need to send this password to the SSL service.
4. **ncat --ssl localhost 31790**
- Makes a secure SSL connection to localhost on port 31790 (the SSL-enabled port you found).
- **Goal** → connect to the right service.
5. **password**
- You paste the bandit16 password here.
- If correct, the server will return an RSA Private Key.
6. **mkdir /tmp/le17**
- Create a temporary working directory in /tmp (because you don’t have write permission in home).
7. **cd /tmp/le17**
- Move into that new directory.
8. **nano private.key**
- Open a text editor to create a new file named private.key.
9. **Paste the RSA key**
- Paste the private key you received earlier, and save the file.
10. **chmod 400 private.key**
- Restrict permissions so only you can read the key.
- SSH requires private keys to have strict permissions, otherwise it refuses to use them.
11. **ls -l**
- Check the file permissions.
- It should look like: -r-------- (only owner can read).
12. **ssh -i private.key bandit17@localhost -p 2220**
- Log into the bandit17 account using the RSA private key.
- -i tells SSH which private key file to use.
13. **cat /etc/bandit_pass/bandit17**
- Once logged in as bandit17, read the password file.
- This gives you the password for the next level.
