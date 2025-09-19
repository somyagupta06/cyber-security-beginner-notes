#  John the Ripper

## **Basic knowledge of cryptography terms**.  




## 🔑 What are Hashes?

A **hash** is like taking any data (short or long) and converting it into a **fixed-length code** using a **hashing algorithm**.

- ✅ Masks the original data  
- ✅ Always produces same output for the same input  
- ✅ Different input → different output  

**Example (MD5 Hashing):**
- Input: `polo` → Output: `b53759f3ce692de7aff1b5779d3964da` (32 characters long)  
- Input: `polomints` → Output: `584b64f4586136bc280f279c64f3b` (also 32 characters long)  

📌 No matter how small or big the input is, **MD5 always gives 32-character hash**.

---

## 🔐 What Makes Hashes Secure?

Hash functions are designed as **one-way functions**:
- Easy to go from **data → hash**
- Hard to go from **hash → data**

This means:
- Calculating a hash = fast (easy problem = "P")  
- Reversing a hash = super hard (intractable problem = "NP")  

### ⚖️ P vs NP (Simple Explanation)

| Term | Meaning | Example |
|------|----------|---------|
| **P (Polynomial Time)** | Problems solved quickly (time grows reasonably with size). | Sorting a list. |
| **NP (Non-deterministic Polynomial Time)** | Hard to solve, but easy to check once solved. | Cracking a hash. |

👉 Hashing (`P`) = easy.  
👉 Unhashing (`NP`) = practically impossible with normal computers.

---

## 🛠 Where John Comes In

Even though hashes are one-way, they can still be **cracked** using special techniques.

### 📂 Dictionary Attack
1. Take a **wordlist** (e.g., thousands of possible passwords).  
2. Hash each word with the same hashing algorithm.  
3. Compare the generated hashes with the target hash.  
4. If one matches → you’ve found the original word!  

**Example:**
- Target hash: `5f4dcc3b5aa765d61d8327deb882cf99`  
- Dictionary word `password` → MD5 = `5f4dcc3b5aa765d61d8327deb882cf99`  
- ✅ Match found → Original word is **password**.

---

## ⚡ John the Ripper (JtR)

- A famous tool for **fast brute-force & dictionary attacks**.  
- Supports many hash types (MD5, SHA1, NTLM, etc.).  
- Automates the process of trying millions of possible words until a match is found.  

---

# 🔑 John the Ripper — Installation & Wordlists



## ⚙️ Versions of John the Ripper

John comes in multiple editions:

| Version | Description |
|---------|-------------|
| **Core John** | Basic version, limited features. |
| **Jumbo John** | Extended version (community edition) with extra tools like `zip2john`, `rar2john`. |

👉 We will use **Jumbo John**.

---

## 🖥 Using John on Different Systems

### ✅ AttackBox & Kali Linux
- **Jumbo John is pre-installed.**
- Just type:
```bash
  john
```
- If installed, you’ll see something like:
``` bash
John the Ripper 1.9.0-jumbo-1
```
## 🐧 Other Linux Distros
- Some distros have John in their repositories:
  - Fedora:
``` bash

sudo dnf install john
```
  - Ubuntu:
``` bash
sudo apt install john
```
- ⚠️ Note: These versions may be the core John, not Jumbo.
- If you need full features → build Jumbo John from source (check official installation guide).
## 🪟 Installing on Windows
1. Download Jumbo John binaries (zip files).
   - Choose 64-bit or 32-bit depending on your system.
2. Extract the zip and run john.exe.
## 📚 Wordlists
To crack hashes with dictionary attacks, we need wordlists (collections of possible passwords).

**🔍 Where to Find Wordlists?**
 - On Kali/AttackBox:
   /usr/share/wordlists contains useful lists.
 - SecLists Repository (GitHub)
    - Contains thousands of wordlists.
    - Path: /Passwords/Leaked-Databases/
## 🪨 RockYou Wordlist
- Most famous wordlist → rockyou.txt
- ~14 million common passwords.
- Came from a 2009 data breach (rockyou.com).
- On Kali/AttackBox:
   File usually located at:
``` bash
/usr/share/wordlists/rockyou.txt
```
- Sometimes compressed as rockyou.txt.tar.gz → Extract using:
``` bash
tar xvzf rockyou.txt.tar.gz
```
# 🔑 John the Ripper – Basic Usage 

There are multiple ways to use **John the Ripper (JtR)** to crack simple hashes.  
Here we will cover **basic syntax, automatic cracking, identifying hashes, and format-specific cracking**.  



## 📌 John Basic Syntax  

``` bash
john [options] [file path]
```

- **john** → Runs the John the Ripper program.  
- **[options]** → Options like wordlist, format, etc.  
- **[file path]** → File containing the hash you want to crack.  
  - If the file is in the same directory → just use the file name.  

**Example:**  
``` bash
john hash_to_crack.txt
```

---

## ⚡ Automatic Cracking  

John can try to automatically detect the hash type and crack it using a wordlist.  
This is not always reliable, but useful when you are unsure about the hash type.  

**Syntax:**  
``` bash
john --wordlist=[path to wordlist] [path to file]
```

- **--wordlist** → Tells John to use a wordlist.  
- **[path to wordlist]** → Location of the wordlist (e.g., rockyou.txt).  
- **[path to file]** → File that contains the hash.  

**Example:**  
``` bash
john --wordlist=/usr/share/wordlists/rockyou.txt hash_to_crack.txt
```

---

## 🔍 Identifying Hashes  

Sometimes John cannot auto-detect the hash type.  
In that case, we need tools like **hash-identifier** to recognize the hash format.  

### Using `hash-identifier`  
1. Download the tool:  
``` bash

wget https://gitlab.com/kalilinux/packages/hash-identifier/-/raw/kali/master/hash-id.py
```
2. Run the tool:  
``` bash
python3 hash-id.py
```
3. Paste the hash → It will suggest possible formats.  

**Example:**  
HASH: 2e728dd31fb5949bc39cac5a9f066498
Possible Hashes:
[+] MD5
[+] Domain Cached Credentials - MD4

---

## 🎯 Format-Specific Cracking  

Once you know the hash type, you can specify it manually in John.  

**Syntax:**  
john --format=[format] --wordlist=[path to wordlist] [path to file]

- **--format** → Defines the hash format (e.g., raw-md5, raw-sha1).  
- **--wordlist** → Uses the given wordlist.  
- **[file path]** → Hash file.  

**Example:**  
``` bash
john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt hash_to_crack.txt
```
<img width="1220" height="546" alt="Screenshot 2025-08-16 at 3 19 55 PM" src="https://github.com/user-attachments/assets/0f5b6a4c-afc7-4a48-bdc8-b66bbd0155d8" />

---

## 📝 Note on Formats  

- If you’re cracking a **standard hash type** (like MD5, SHA1, etc.), you usually need to add the **raw-** prefix.  
  - Example: `raw-md5`, `raw-sha1`.  
- But this rule doesn’t apply for all hash types.  
- To check supported formats:  
``` bash
john --list=formats
```
- To search for a specific type (e.g., md5):
``` bash 
john --list=formats | grep -iF "md5"
```


## ✅ Quick Comparison Table  

| Method                | Command Example                                                                 | When to Use |
|------------------------|---------------------------------------------------------------------------------|-------------|
| **Basic Run**          | john hash_to_crack.txt                                                         | When testing simple/default behavior |
| **Automatic Cracking** | john --wordlist=/usr/share/wordlists/rockyou.txt hash_to_crack.txt             | When unsure of hash type |
| **Identify Hash**      | python3 hash-id.py → Enter hash                                                | When John fails to detect format |
| **Format-Specific**    | john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt hash.txt     | When you know the hash type |

---
# 🔐 Cracking Authentication Hashes (NTLM / NTHash)

Now that we understand the **basic syntax** of John the Ripper, let’s move on to **real-world password hashes** used in Windows systems.  
These hashes are called **Authentication Hashes** because they store user passwords in a hashed form.  

---

## 🧾 What is NTHash / NTLM?

- **NTHash (aka NTLM hash)** → Modern Windows systems use this format to store user & service account passwords.  
- **LM Hash** → Older version (used before NTLM).  
- Together often called **NT/LM hashes**.  

💡 **History Note:**  
- "NT" originally stood for **New Technology** (introduced in Windows NT, different from MS-DOS).  
- Later, "NT" was dropped from Windows product names, but **NT** still survives in technologies like NTFS (file system), NTLM (authentication).  

---

## 📂 Where Are These Hashes Stored?

- Windows stores usernames & password hashes in:  
  - **SAM (Security Account Manager) database**.  
  - **Active Directory (AD) database** → `NTDS.dit` file.  

### 🔑 How Can You Get These Hashes?
- **Mimikatz** → Tool for extracting Windows credentials.  
- **Dumping the SAM database** (if you have admin/privileged access).  
- **Dumping NTDS.dit** on a Domain Controller.  

---

## ⚔️ Cracking vs Pass-the-Hash  

- Sometimes you **don’t need to crack** the NTLM hash.  
  - You can perform a **Pass-the-Hash (PtH) attack** → directly reuse the hash for authentication.  
- But if the system has a **weak password policy**, you can try to **crack the hash** with John.  

---

## 📌 Cracking NTLM with John the Ripper  

**Syntax:**
``` bash
john --format=NT --wordlist=[path to wordlist] [hash file]
```
- **--format=NT** → Tells John the hash is NTLM/NTHash.  
- **--wordlist** → Use a password list like rockyou.txt.  
- **[hash file]** → File containing the dumped hashes.  

**Example:**  
``` bash

john --format=NT --wordlist=/usr/share/wordlists/rockyou.txt ntlm_hashes.txt
```
<img width="783" height="286" alt="Screenshot 2025-08-16 at 3 31 57 PM" src="https://github.com/user-attachments/assets/e4f25f38-1053-452c-a2a3-d115e8940291" />

---

## ✅ Quick Comparison Table  

| Term        | Meaning                                                                 | Example Location |
|-------------|-------------------------------------------------------------------------|------------------|
| **SAM**     | Local Windows database storing usernames & password hashes             | C:\Windows\System32\config\SAM |
| **NTDS.dit**| AD (Domain Controller) database containing credentials                 | C:\Windows\NTDS\ntds.dit |
| **NTLM Hash** | Password hash format used in modern Windows                           | e.g., `aad3b435b51404eeaad3b435b51404ee` |
| **Pass-the-Hash** | Technique to authenticate using the hash without cracking it      | Useful in lateral movement |

---

## 🚀 Example Attack Workflow  

1. Gain **admin/privileged access** on a Windows system.  
2. Dump NTLM hashes (with Mimikatz / SAM / NTDS.dit).  
3. Try **Pass-the-Hash attack** (faster).  
4. If needed → **crack hashes with John** using RockYou or custom wordlists.  

---
# 🔐 Cracking Hashes from /etc/shadow (Linux)

On **Linux systems**, user password hashes are stored in the `/etc/shadow` file.  
This file is **only readable by root**, because it contains sensitive info like:  
- Encrypted password hashes  
- Last password change date  
- Expiry information  

Each **line = one user account entry**.  
If you have root access and can read `/etc/shadow`, you may be able to crack some of the hashes.

---

## 📂 Why Do We Need `/etc/passwd` Too?

- John the Ripper needs both `/etc/passwd` and `/etc/shadow` together in a **specific format**.  
- `/etc/passwd` → Contains usernames + basic account info.  
- `/etc/shadow` → Contains hashed passwords.  
- John doesn’t understand raw `/etc/shadow` alone → so we use the **`unshadow` tool** to merge them.  

---

## 🛠️ Unshadowing

**Syntax:**  
``` bash
unshadow [path to passwd] [path to shadow]
```
- `unshadow` → Tool included with John  
- `[path to passwd]` → File containing `/etc/passwd` info  
- `[path to shadow]` → File containing `/etc/shadow` info  



## 📝 Example  

Suppose we extract these 2 lines from a Linux system:  

**File 1 → local_passwd**
``` bash
root:x:0:0::/root:/bin/bash
```
**File 2 → local_shadow**
``` bash
root:$6$2nwjN454g-dv4HN/$m9Z/r2xVfweYVkrr.v5Ft8Ws3/YYksfNwq96UL1FX00JjY1L61.DS3KEVsZ9rOVLB/1dTeEL/OIhJZ4GMFMGA0:18576:::
```
Now combine them:  
``` bash
unshadow local_passwd local_shadow > unshadowed.txt
```
This creates a **merged file** (`unshadowed.txt`) which John can understand.  
<img width="1140" height="123" alt="Screenshot 2025-08-16 at 3 37 27 PM" src="https://github.com/user-attachments/assets/fb3539df-7b6e-438d-9844-7a48b93a3d08" />



## 🔓 Cracking with John  

Now, use John on the unshadowed file: 
``` bash
john --wordlist=/usr/share/wordlists/rockyou.txt unshadowed.txt
```
If John doesn’t automatically detect the hash type, specify the format manually: 
``` bash
john --wordlist=/usr/share/wordlists/rockyou.txt --format=sha512crypt unshadowed.txt
```
<img width="1220" height="331" alt="Screenshot 2025-08-16 at 3 39 04 PM" src="https://github.com/user-attachments/assets/e14c7469-dd90-463b-b946-4835602803c6" />


## ✅ Quick Comparison Table  

| File          | Purpose                               | Example Line |
|---------------|---------------------------------------|--------------|
| **/etc/passwd** | User info (username, UID, home dir, shell) | `root:x:0:0::/root:/bin/bash` |
| **/etc/shadow** | Encrypted passwords + aging info      | `root:$6$2nwjN454g-dv4HN/...` |
| **unshadowed.txt** | Merged version for John to crack     | `root:$6$...:/root:/bin/bash` |



## 🚀 Workflow Summary  

1. Extract `/etc/passwd` and `/etc/shadow` (requires root).  
2. Use `unshadow` to merge them into `unshadowed.txt`.  
3. Run John with a wordlist (like `rockyou.txt`).  
4. Optionally specify `--format=sha512crypt` if needed.  
5. Recover cracked passwords 🚀.  
---
# 🔑 John the Ripper – Single Crack Mode

So far, we have used **wordlist mode** in John to crack hashes with pre-defined dictionaries.  
But John also has a powerful mode called **Single Crack Mode**, which tries to create passwords automatically based on the **username** or other related info.



## 🧩 What is Single Crack Mode?

- Instead of using a big wordlist like `rockyou.txt`, John creates **possible passwords on its own**.  
- It uses the **username** and modifies it using **word mangling rules**.  
- This works because many people create weak passwords by slightly changing their own names or related info.



## 🔄 Word Mangling Example

Let’s say the username is **Markus**.  
John might try the following mutations:

| Technique       | Example Passwords |
|-----------------|-------------------|
| Add numbers     | Markus1, Markus2, Markus3 |
| Change casing   | MArkus, MARkus, MARKus |
| Add symbols     | Markus!, Markus$, Markus* |

👉 This process of modifying a base word is called **Word Mangling**.  

John applies **mangling rules** to systematically generate variations.

---

## 📋 GECOS Field (Extra Info for Cracking)

In Linux/UNIX systems, user details are stored in `/etc/passwd`.  
Each line is separated by `:` (colon).  

The **5th field = GECOS field**, which may contain:  
- Full Name  
- Office Number  
- Phone Number  
- Home Directory  

🔎 Example entry:  
``` bash
markus:x:1001:1001:Markus Brown,Office 42,555-1234:/home/markus:/bin/bash
```
Here, **"Markus Brown"** (from GECOS) can also be used by John to generate password guesses like:  
- MarkusBrown123  
- Markus!  
- Brown42  

👉 This increases the chance of cracking weak passwords.



## ⚙️ Using Single Crack Mode

**Basic Command:**  
``` bash
john --single --format=[format] [path to file]
```
- `--single` → Activates single crack mode  
- `--format=[format]` → Hash type (e.g., `raw-sha256`, `md5crypt`, etc.)  
- `[path to file]` → File containing hashes to crack  

**Example:**  
``` bash
john --single --format=raw-sha256 hashes.txt
```


## 📂 Input File Format for Single Mode

In Single Crack Mode, John needs to know which **username** belongs to which **hash**.  
That’s why we must **prepend the hash with the username**.

🔎 Example:  

**Original hashes.txt**
``` bash
1efee03cdcb96d90ad48ccc7b8666033
```
**Modified hashes.txt (Single Mode)**
``` bash
mike:1efee03cdcb96d90ad48ccc7b8666033
```
Now John knows:  
- Username = `mike`  
- Hash = `1efee03cdcb96d90ad48ccc7b8666033`  

It can start generating possible passwords like:  
- mike123  
- Mike!  
- mike2024  
<img width="484" height="81" alt="Screenshot 2025-08-16 at 3 48 17 PM" src="https://github.com/user-attachments/assets/8e2b6ec8-622d-4835-a7b6-0e924dcea162" />

<img width="269" height="64" alt="Screenshot 2025-08-16 at 3 48 39 PM" src="https://github.com/user-attachments/assets/9d453fa4-c86a-4f8d-98f5-f99ba481f679" />

<img width="894" height="342" alt="Screenshot 2025-08-16 at 3 50 26 PM" src="https://github.com/user-attachments/assets/c7423b1b-8de7-4e75-9b0a-78cbad1165f3" />



## ✅ Quick Comparison: Wordlist Mode vs Single Mode

| Feature              | Wordlist Mode                  | Single Crack Mode |
|----------------------|--------------------------------|-------------------|
| Data source          | Uses external wordlist file    | Generates from username/GECOS |
| Best for             | Large dictionaries, leaked lists | Personalized, weak passwords |
| Speed                | Depends on wordlist size       | Usually faster (small search space) |
| Example command      | `john --wordlist=rockyou.txt hashes.txt` | `john --single --format=raw-sha256 hashes.txt` |



## 🚀 Workflow Summary

1. Prepare hash file (prepend username before hash).  
2. Run John with `--single` mode.  
3. John auto-generates a custom wordlist using mangling rules.  
4. John attempts to crack the hashes using these guesses.  
5. Useful when users pick **username-based weak passwords**.  

---
# ⚙️ John the Ripper – Custom Rules

We already saw how John can **mangle usernames** in *Single Crack Mode*.  
But what if we know the **common password patterns** used by an organisation or person?  
👉 That’s where **Custom Rules** come in.

Custom Rules let us define our **own patterns**, and John will **dynamically generate passwords** based on them.

---

## 🔎 Why Custom Rules?

Most companies enforce **password complexity** requirements:  
- ✅ Lowercase letters  
- ✅ Uppercase letters  
- ✅ Numbers  
- ✅ Symbols  

This is good for security, but users are often **predictable**.  
For example, instead of creating something random like `gT#7vLp@`, many people use patterns such as:

Polopassword1!

Here’s the pattern:  
- Capital letter at the start (`P`)  
- Base word (`olopassword`)  
- Number at the end (`1`)  
- Symbol at the very end (`!`)  

👉 This is predictable, so attackers can exploit it with **Custom Rules**.

---

## 🛠️ Where to Define Custom Rules?

Custom rules are defined inside the **john.conf** file.  
- On TryHackMe AttackBox → `/opt/john/john.conf`  
- On Linux (installed version) → `/etc/john/john.conf`

Each rule starts with:
``` bash
[List.Rules:RuleName]
```
This defines the **name** of the rule.



## 🔤 Basic Modifiers (Rule Syntax)

| Modifier | Meaning | Example |
|----------|---------|---------|
| `:`      | Append characters | word → word1 |
| `A0`     | Prepend characters | word → !word |
| `c`      | Capitalise letters positionally | word → Word |

👉 These can be combined for complex patterns.

---

## 🔡 Character Sets

We can define **which characters** to append, prepend, or modify by using square brackets `[]`.

| Pattern   | Meaning |
|-----------|---------|
| `[0-9]`   | Numbers `0–9` |
| `[0]`     | Only digit `0` |
| `[A-Z]`   | Uppercase letters |
| `[a-z]`   | Lowercase letters |
| `[A-z]`   | Both upper + lowercase |
| `[!€$%@]` | Special symbols |



## 🧩 Example: PoloPassword Rule

We want to crack passwords like:

Polopassword1!

**Step 1 – Base word**: `polopassword`  
**Step 2 – Apply transformations**:  
- Capitalise first letter → `Polopassword`  
- Append number `0–9` → `Polopassword1`  
- Append symbol from `!€$%@` → `Polopassword1!`  

**Rule Definition in john.conf**:
``` bash
[List.Rules:PoloPassword]
cAz"[0-9][!€$%@]"
```
### Explanation:
- `c` → Capitalise the first letter  
- `Az` → Append to the end of the word  
- `[0-9]` → Append any digit from 0–9  
- `[!€$%@]` → Append one special symbol  



## ▶️ Using Custom Rules

Once defined, we can call this rule using the `--rule` option.

**Command Structure**:
``` bash
john --wordlist=[path_to_wordlist] --rule=PoloPassword [path_to_hash_file]
```
**Example**:
``` bash
john --wordlist=rockyou.txt --rule=PoloPassword hashes.txt
```
Here, John will try words like:
- Polopassword1!
- Polopassword5$
- Polopassword9@



## 📊 Comparison: Wordlist vs Single Mode vs Custom Rules

| Feature            | Wordlist Mode             | Single Crack Mode          | Custom Rules |
|--------------------|---------------------------|----------------------------|--------------|
| Input source       | External wordlist (e.g., rockyou) | Username + GECOS info | Wordlist + Rules |
| Flexibility        | Low (fixed words)         | Medium (username mangling) | High (custom patterns) |
| Best for           | Common leaked passwords   | Weak username-based passwords | Predictable complexity patterns |
| Example use case   | crack `123456`            | crack `markus123`          | crack `Polopassword1!` |



## 🚀 Workflow Summary

1. Identify common password pattern (e.g., Capitalised base + number + symbol).  
2. Open `john.conf` and define a `[List.Rules:RuleName]`.  
3. Use modifiers (`c`, `A0`, `Az`, etc.) and character sets (`[0-9]`, `[A-Z]`, `[!$%]`).  
4. Run John with your custom rule:  
john --wordlist=mylist.txt --rule=RuleName hashes.txt
5. John will generate **dynamic variations** of the base words and attempt cracking.

---
# 🔐 Cracking ZIP Passwords with John the Ripper

Yes! You read that right 🚀  
We can use **John the Ripper (JTR)** to crack the password of **password-protected ZIP files**.  

The process happens in **two steps**:
1. Convert the ZIP file into a format John understands.  
2. Use John with a wordlist to crack the password.



## 🧩 Step 1: Convert ZIP → Hash

To do this, we use a tool called **zip2john** (comes with John suite).

**Basic syntax:**
``` bash
zip2john [zip_file] > [output_file]
```
- `[zip_file]` → Path of the protected ZIP file.  
- `[output_file]` → File where the extracted hash will be stored.  
- `>` → Redirects the hash output into another file.  

**Example:**
``` bash
zip2john secret.zip > zip_hash.txt
```
👉 This will generate a special hash for `secret.zip` and save it inside `zip_hash.txt`.



## 🧩 Step 2: Crack the Hash with John

Once we have the hash, we can run John as usual.

**Basic syntax:**
``` bash
john --wordlist=[path_to_wordlist] [hash_file]
```
**Example:**
``` bash
john --wordlist=/usr/share/wordlists/rockyou.txt zip_hash.txt
```
👉 John will now try each password from the **rockyou.txt** wordlist against the hash of `secret.zip`.

<img width="977" height="290" alt="Screenshot 2025-08-16 at 4 02 37 PM" src="https://github.com/user-attachments/assets/5f3328ff-d719-485c-9060-610d9a1a1242" />

<img width="712" height="178" alt="Screenshot 2025-08-16 at 4 03 33 PM" src="https://github.com/user-attachments/assets/84fb4ac9-9b35-412f-b560-8d1a1f83f6ec" />

## 📊 Workflow Overview

| Step | Tool     | Input            | Output          |
|------|----------|------------------|-----------------|
| 1    | zip2john | secret.zip       | zip_hash.txt    |
| 2    | john     | rockyou.txt + zip_hash.txt | Cracked password |



## 🧪 Example Walkthrough

1. Suppose we have a file `confidential.zip` protected with a password.  
2. Run:
zip2john confidential.zip > zip_hash.txt
→ This creates a hash file.  
3. Then run:
john --wordlist=rockyou.txt zip_hash.txt
→ John will attempt to crack the password using `rockyou.txt`.  
4. If successful, John will reveal the password. 🎉



## 🔎 Comparison with Other Modes

| Feature                | Normal Wordlist Mode | Single Crack Mode | ZIP Cracking |
|-------------------------|-----------------------|-------------------|--------------|
| Input                   | Password hash files  | Username + hash   | ZIP archive  |
| Extra tool needed       | ❌ No                 | ❌ No              | ✅ Yes (zip2john) |
| Real-world use case     | Cracking hash dumps  | Username-based guessing | Cracking password-protected ZIPs |
| Example command         | john hash.txt        | john --single hash.txt | john zip_hash.txt |



  

---
# 🔐 Cracking RAR Passwords with John the Ripper

Just like ZIP archives, we can also crack **password-protected RAR files** using John the Ripper.  
RAR files are compressed archives created with **WinRAR** or similar tools.

The process is almost the same as ZIP cracking:  
1. Convert the RAR file into a hash with **rar2john**.  
2. Use John with a wordlist to crack the password.



## 🧩 Step 1: Convert RAR → Hash

We use **rar2john** (part of John suite).

**Basic syntax:**
``` bash
rar2john [rar_file] > [output_file]
```
- `[rar_file]` → Path of the RAR archive.  
- `[output_file]` → File where the extracted hash will be saved.  
- `>` → Redirects the hash into the output file.  

**Example:**
``` bash
rar2john secret.rar > rar_hash.txt
```
👉 This generates a hash of `secret.rar` and saves it to `rar_hash.txt`.



## 🧩 Step 2: Crack the Hash with John

Now use John on the hash file with a wordlist.

**Basic syntax:**
``` bash
john --wordlist=[path_to_wordlist] [hash_file]
```
**Example:**
``` bash
john --wordlist=/usr/share/wordlists/rockyou.txt rar_hash.txt
```
👉 John will test each password from `rockyou.txt` against the RAR hash.

<img width="1103" height="627" alt="Screenshot 2025-08-16 at 4 08 37 PM" src="https://github.com/user-attachments/assets/82804cb6-4ca8-48f3-bbec-a98810c1e861" />


## 📊 Workflow Overview

| Step | Tool     | Input           | Output        |
|------|----------|-----------------|---------------|
| 1    | rar2john | secret.rar      | rar_hash.txt  |
| 2    | john     | rockyou.txt + rar_hash.txt | Cracked password |



## 🧪 Example Walkthrough

1. Suppose we have a file `backup.rar` protected with a password.  
2. Run:
``` bash
rar2john backup.rar > rar_hash.txt
```
→ Creates a hash file.  
3. Then run:
``` bash
john --wordlist=rockyou.txt rar_hash.txt
```
→ John starts cracking with the wordlist.  
4. If the password is weak/common, John will recover it. 🎉



## 🔎 Comparison: ZIP vs RAR Cracking

| Feature                | ZIP Cracking        | RAR Cracking       |
|-------------------------|---------------------|--------------------|
| Extra tool used         | zip2john           | rar2john           |
| File type supported     | `.zip` archives    | `.rar` archives    |
| Example output file     | zip_hash.txt       | rar_hash.txt       |
| John usage              | john zip_hash.txt  | john rar_hash.txt  |

 

---
# 🔑 Cracking SSH Private Key Passwords with John the Ripper

So far, we cracked **ZIP** and **RAR** archives.  
Now let’s move to something different → **SSH private key passwords**.

When logging into a remote machine, instead of a normal password, many admins use **SSH key-based authentication**.  
The private key file (commonly named `id_rsa`) may itself be **protected with a passphrase**.  

👉 If you have the private key but not its password, you can try cracking it with John.



## 🧩 Step 1: Convert SSH Key → Hash

We use **ssh2john** (or `ssh2john.py` if the binary is not available).

**Basic syntax:**
``` bash
ssh2john [id_rsa_file] > [output_file]
```
- `[id_rsa_file]` → The private SSH key file (usually named `id_rsa`).  
- `[output_file]` → File where the hash will be stored.  
- `>` → Redirects the hash output to a file.  

**Example:**
``` bash
/opt/john/ssh2john.py id_rsa > id_rsa_hash.txt
```
👉 This generates a hash from `id_rsa` and saves it into `id_rsa_hash.txt`.



## 🧩 Step 2: Crack the Hash with John

Now feed the hash file into John with a wordlist.

**Basic syntax:**
``` bash
john --wordlist=[path_to_wordlist] [hash_file]
```
**Example:**
``` bash
john --wordlist=/usr/share/wordlists/rockyou.txt id_rsa_hash.txt
```
👉 John will try each word from the wordlist as the passphrase for the SSH key.
<img width="1461" height="684" alt="Screenshot 2025-08-16 at 4 14 59 PM" src="https://github.com/user-attachments/assets/7e73cd41-4d54-49ce-8fb3-7bc52d194ad2" />



## 📊 Workflow Overview

| Step | Tool       | Input         | Output          |
|------|------------|---------------|-----------------|
| 1    | ssh2john   | id_rsa        | id_rsa_hash.txt |
| 2    | john       | rockyou.txt + id_rsa_hash.txt | Cracked passphrase |



## 🧪 Example Walkthrough

1. Suppose we have an SSH key file: `id_rsa`.  
2. Run:
``` bash
/opt/john/ssh2john.py id_rsa > id_rsa_hash.txt
```
→ Extracts a crackable hash.  
3. Then run:
``` bash
john --wordlist=rockyou.txt id_rsa_hash.txt
```
→ John starts brute-forcing the passphrase.  
4. If the passphrase is weak/common, John will recover it. 🎉  



## 🔎 Comparison with Other Cracking

| Type of File   | Conversion Tool | Output File     | John Command Example |
|----------------|-----------------|-----------------|----------------------|
| ZIP Archive    | zip2john        | zip_hash.txt    | john zip_hash.txt    |
| RAR Archive    | rar2john        | rar_hash.txt    | john rar_hash.txt    |
| SSH Private Key| ssh2john.py     | id_rsa_hash.txt | john id_rsa_hash.txt |

---
