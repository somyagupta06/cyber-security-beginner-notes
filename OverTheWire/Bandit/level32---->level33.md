# 🔐 Bandit Level: Bandit32 ➝ Bandit33

## 📂 Command(s) used:
👉 
- $SHELL
- EXIT
- $0
- ls
- cat /etc/bandit_pass/bandit33          


<img width="587" height="217" alt="Screenshot 2025-09-07 at 12 13 51 PM" src="https://github.com/user-attachments/assets/ba918476-9ffa-48c6-9443-592931960a62" />




## 📄 Password found:
👉 
tQdtbs5D5i2vJwkO8mEyYEyTL8izoeJ0

## NOTES
# Bandit Uppercase Shell - Walkthrough (Bandit32 → Bandit33)

## Overview
In this level, you are given a **restricted shell** called the **Uppercase Shell**.  
This shell only allows uppercase input and blocks normal commands like `ls`, `cat`, or `exit`.  
The challenge is to bypass this shell to read the next password file.

---

## Step 1: Login / Access Uppercase Shell
After connecting to Bandit:

```bash
ssh bandit32@bandit.labs.overthewire.org -p 2220
```
The shell displays:
```
WELCOME TO THE UPPERCASE SHELL
```
- You are now in a restricted shell.
- Only uppercase letters are allowed.
- Normal commands in lowercase (like ls, cat) will fail.
## Step 2: Understanding the Restrictions
Attempting normal commands:
```
ls
```
- Output: shell may ignore or show nothing.
```
cat /etc/bandit_pass/bandit32
```
- Output: Permission denied
```
exit
```
- Output: Permission denied
**Observation:** The shell blocks normal commands and file access.
We need a trick to bypass the restriction.
## Step 3: Using $0 to Bypass the Uppercase Shell
- $0 holds the current shell or script name.
```
$0
```
- This executes the current shell.
- In the Uppercase Shell, executing $0 may spawn a normal shell, bypassing restrictions.
- Once in the normal shell, you can run lowercase commands freely.
## Step 4: Listing Files (Optional)
ls
- Shows available files/folders. Usually uppershell is present.
## Step 5: Reading the Next Password
```
cat /etc/bandit_pass/bandit33
```

# Explaination


## Overview
In Bandit32, you are dropped into a **restricted shell** called the **Uppercase Shell**.  
- Only uppercase input is allowed.  
- Normal lowercase commands like `ls`, `cat`, or `exit` **do not work**.  
- The goal is to **read the password for Bandit33**.



## Understanding `$0`

- `$0` is a **bash/shell variable** that stores the **current script or shell name**.
  - Example:
    ```bash
    echo $0
    ```
    - Might return `/bin/bash` if running a normal shell.
    - Might return `/path/to/uppershell` if running the Uppercase Shell binary.
- In the Uppercase Shell, `$0` is still available, so you **know the path of the currently running shell**.



## The Trick / Exploit

1. **Problem:** You cannot run lowercase commands:
    ```bash
    ls
    cat /etc/bandit_pass/bandit32
    exit
    ```
    - All fail due to uppercase restriction.

2. **Solution:** Use `$0` to **re-run the current shell**.
    ```bash
    $0
    ```
    - This executes the shell binary again.
    - If the shell allows it, this can **spawn a normal shell**.
    - Once in the normal shell, lowercase commands work.


## Summary 

- `$0` = current shell or script path.
- Restricted Uppercase Shell **blocks normal commands**.
- Executing `$0` **relaunches the current shell**, potentially bypassing restrictions.
- Once in the normal shell, you can **access files normally** and proceed to the next level.



## Key Commands (Raw)

```bash
$0
cat /etc/bandit_pass/bandit33
```
## Conceptual Insight
- The main challenge is escaping a restricted environment, not the file itself.
- $0 exploit works because it points to the currently running shell, which can be executed again to get a normal shell environment.
