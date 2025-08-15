# 🔓 Hashing Basics 

## 1. What is a Hash Function?
- A **hash function** is a process that takes any size input (like a file, text, or data) and gives a fixed-size output called a hash value (or digest).
- **Properties:**

  1. Fixed size output: No matter how big the input is, the output size is fixed.
  2. Irreversible: You cannot easily find the original input from the hash value.
  3. Sensitive to change: Even changing 1 bit in the input gives a completely different hash.
  4. Fast to compute: Hashing should be quick.


**Example:**
File1: T -> binary 01010100 -> hex 54
File2: U -> binary 01010101 -> hex 55

- Files differ by **1 bit**, but MD5, SHA1, SHA256 hashes are completely different.



## 2. Why Use Hash Functions?
- Verify **data integrity**: ensure downloaded/copied files match the original.
- Password storage:
  - Websites store **hash of password**, not the password itself.
  - At login, hashes are compared.

**Example:**
- Check if a 6GB file is identical by comparing hash values:
  - Hashes match → files identical
  - Hashes differ → files differ



## 3. Common Hash Algorithms
- **MD5** → 128-bit hash (fast but insecure)
- **SHA1** → 160-bit hash (better, not fully secure)
- **SHA256** → 256-bit hash (secure, widely used)
<img width="640" height="122" alt="Screenshot 2025-08-15 at 6 06 45 PM" src="https://github.com/user-attachments/assets/1b16e23b-2d65-4f64-971a-7a1f6b5ca3a7" />

> **Note:** Avoid MD5 and SHA1 for passwords or sensitive data.



## 4. Hash Collisions
- A **hash collision**: two different inputs produce the same hash.
- Collisions are possible because hash outputs are limited (pigeonhole principle).

**Example:**
- 21 pigeons, 16 holes → some pigeons share holes
- Many inputs, limited hashes → collisions possible
- Good hash functions make collisions extremely unlikely



## 5. Hashing vs Encryption

| Feature           | Hashing                     | Encryption                |
|------------------|-----------------------------|--------------------------|
| Key               | None                        | Required                 |
| Reversibility     | One-way                     | Reversible (with key)    |
| Purpose           | Verify integrity/passwords | Secure communication     |




## 6. Example Commands (Linux Terminal)

```bash
# Check hash of files
md5sum file1.txt file2.txt
sha1sum file1.txt file2.txt
sha256sum file1.txt file2.txt
```

----


# 🔓 Password Storage & Data Integrity 

## 1. Password Storage vs Password Managers
- **Password Storage** → Used for **authentication** (to check if user knows the password).  
- **Password Managers** → Need to **retrieve actual password** (cleartext) to log in.  
- **Key difference:** Authentication only needs to verify password, not know it.



## 2. Why Plaintext Passwords Are Dangerous
- Some websites **store passwords as plain text** (not hashed).  
- If the database is leaked, **all passwords are exposed**.  
- Many people reuse passwords → leaking one account can **jeopardize others**.  

**Example:**  
- `rockyou.txt` (Kali Linux) → leaked passwords from a company that stored passwords in plaintext.  
- File has **over 14 million passwords**.  

```bash
# Linux terminal
strategos@g5000 /usr/share/wordlists> wc -l rockyou.txt
14344392 rockyou.txt
strategos@g5000 /usr/share/wordlists> head rockyou.txt
123456
12345
123456789
```

## 3. Insecure Encryption for Passwords
- Some companies use old/deprecated encryption instead of hashing.

**Example:**  **Adobe breach**

 -Encrypted passwords in a weak format
 - Stored password hints in plain text → password could be retrieved easily
 - Lesson: Weak encryption is almost as bad as storing plaintext.


## 4. Insecure Hashing for Passwords

 - Some websites use outdated hash functions like SHA-1.
 
 - Example: LinkedIn 2012 breach
 
 - Used SHA-1 (not secure)
 
 - No salting used
 
### What is Salting?
Add a random value (salt) to password before hashing → makes each hash unique.

**Example:**

Password: "mypassword"

Salt: "X7G2"

Hash("mypasswordX7G2") → unique hash

``` bash
# Generate hash of password (SHA256)
echo -n "mypassword" | sha256sum

# Add a random salt
SALT=$(openssl rand -hex 8)
echo -n "mypassword$SALT" | sha256sum
```
---


# 🔓 Using Hashing to Store Passwords

## 1. Why Use Hashing?
- Instead of storing **actual passwords**, we store **hash values**.  
- **Hash function:** Converts input (password) into fixed-length string (hash).  
- Advantage: Even if the database leaks, attackers **cannot easily find the original password**.  



## 2. Problem: Same Passwords
- Hash functions always give **same output for same input**.  
- If two users have same password → same hash → if one is cracked, **both accounts are at risk**.  
- Also allows **Rainbow Table attacks**.

**Example: Rainbow Table**  
| Hash | Password |
|------|---------|
| 02c75fb22c75b23dc963c7eb91a062cc | zxcvbnm |
| b0baee9d279d34fa1dfd71aadb908c3f | 11111 |
| dcddb75469b4b4875094e14561e573d8 | 000000 |
| e10adc3949ba59abbe56e057f20f883e | 123456 |
| e99a18c428cb38d5f260853678922e03 | abc123 |

- Websites like **CrackStation** and **Hashes.com** use huge rainbow tables for fast hash cracking.  
<img width="987" height="536" alt="Screenshot 2025-08-15 at 6 27 41 PM" src="https://github.com/user-attachments/assets/323c8374-1a66-4c96-b6ac-a34a7f5b7863" />



## 3. Protecting Against Rainbow Tables: Salting
- **Salt:** Random value added to password **before hashing**.  
- Unique salt **for each user** → same password produces **different hash**.  
- Salt does **not need to be secret**.  

**Example:**  
```text
Password: "mypassword"
Salt: "X7G2"
Hash("mypasswordX7G2") → unique hash
```
- Even if two users have same password → different hashes.
- Hash functions like Bcrypt and Scrypt do this automatically.
## 4. Example of Secure Password Storage
Steps to store passwords safely:
1. Select secure hash function: Argon2, Scrypt, Bcrypt, PBKDF2.
2. Generate unique salt for each user: e.g., YUV*^(=g°_!.
3. Concatenate password + salt:
``` bash
Password: AL4RMc10k
Salt: YUV*^(=g°_!
Result: AL4RMc10kYUV*^(=g°_!
```
4. Calculate hash of combined string using chosen algorithm.
5. Store both the hash and the salt in the database.
**Important:** Never store the actual password. Only store hash + salt.
## 5. Why Not Use Encryption Instead?
- Encryption can also store passwords securely, but:
  
  1. You need a key to encrypt/decrypt.
  2. If the key is stolen → all passwords are compromised.
- Hashing + salt is safer because you never need to reverse it.



## 6. Quick Comparison

| Method           | Risk if DB Leaked   | Notes                           |
|-----------------|------------------|--------------------------------|
| Plaintext        | Very High         | Avoid at all costs             |
| Encryption       | High if key stolen| Need to protect key            |
| Hashing          | Lower             | Use strong hash + salt         |
| Hashing + Salt   | Very Low          | Best practice for authentication |



## 7. Terminal Example (Linux)

```bash
# Generate a random salt
SALT=$(openssl rand -hex 8)

# Hash password with salt (SHA256 example)
echo -n "mypassword$SALT" | sha256sum
```
**Tip:** Always use a modern hash function + unique salt for every user.  
# 🔓 Recognising & Cracking Hashes – Offensive Security Notes

## 1. Goal
From an **offensive security** perspective:
1. Identify the hash type.
2. Confirm using tools + manual analysis.
3. Crack the hash to recover the password.



## 2. Hash Type Recognition
- Tools like `hashID` can guess hash types, but not always accurate.
- **Prefixes** (e.g., `$2b$`, `$y$`) make detection easier.
- **Context matters**:
  - Found in a web app DB? → Likely **MD5** rather than NTLM.
  - Tools often confuse MD5 with NTLM.



## 3. Linux Password Hashes
- Stored in `/etc/shadow` (root only can read).
- Older systems stored in `/etc/passwd` (readable by everyone → insecure).
- **Each line** in `/etc/shadow` has 9 fields separated by `:`
  - **1st field** → username
  - **2nd field** → encrypted password info (hash + details)



## 4. Linux Encrypted Password Format
$prefix$options$salt$hash

### Parts:
1. **Prefix** → Algorithm ID  
   Example: `$y$` = yescrypt
2. **Options** → Parameters for the algorithm.
3. **Salt** → Random string added before hashing.
4. **Hash** → Final hashed password.



## 5. Common Linux Prefixes

| Prefix | Algorithm         | Notes |
|--------|-------------------|-------|
| `$y$`  | yescrypt           | Default in modern Linux, strong |
| `$gy$` | gost-yescrypt      | GOST + yescrypt |
| `$7$`  | scrypt             | Memory-hard, strong |
| `$2b$`, `$2y$`, `$2x$`, `$2a$` | bcrypt | Based on Blowfish cipher, very secure |
| `$6$`  | sha512crypt        | Based on SHA-512 (older) |
| `$1$`  | md5crypt           | Based on MD5 (outdated) |



## 6. Modern Linux Example
Example from `/etc/shadow`:
strategos:$y$j9T$76UzfgEM5PnymhQ7T1Jey1$/00Sg64dhfF.TigVPdzqiFang6uZA4QA1pzzegKdVm4:19965:0:99999:7:::
Breakdown:
- **Username**: `strategos`
- **Prefix**: `$y$` → yescrypt algorithm
- **Options**: `j9T` → parameters for yescrypt
- **Salt**: `76UzfgEM5PnymhQ7T1Jey1`
- **Hash**: `/00Sg64dhfF.TigVPdzqiFang6uZA4QA1pzzegKdVm4`



## 7. Windows Password Hashes
- Uses **NTLM** (based on MD4).
- Looks similar to MD4 and MD5 → need context to identify.
- Stored in **SAM (Security Accounts Manager)**.
- Windows prevents normal users from dumping SAM → tools like **Mimikatz** bypass this.
- SAM contains:
  - **NT hashes**
  - **LM hashes** (older, weaker)



## 8. How to Identify Hashes
- Use [Hashcat Example Hashes](https://hashcat.net/wiki/doku.php?id=example_hashes) for reference.
- Check:
  - **Prefix**
  - **Length** of hash
  - **Encoding type** (Base64, Hex, etc.)
  - **Research** the system/application where it came from



## 9. Quick Example
If you find:
Breakdown:
- **Username**: `strategos`
- **Prefix**: `$y$` → yescrypt algorithm
- **Options**: `j9T` → parameters for yescrypt
- **Salt**: `76UzfgEM5PnymhQ7T1Jey1`
- **Hash**: `/00Sg64dhfF.TigVPdzqiFang6uZA4QA1pzzegKdVm4`



## 10. Windows Password Hashes
- Uses **NTLM** (based on MD4).
- Looks similar to MD4 and MD5 → need context to identify.
- Stored in **SAM (Security Accounts Manager)**.
- Windows prevents normal users from dumping SAM → tools like **Mimikatz** bypass this.
- SAM contains:
  - **NT hashes**
  - **LM hashes** (older, weaker)



## 11. How to Identify Hashes
- Use [Hashcat Example Hashes](https://hashcat.net/wiki/doku.php?id=example_hashes) for reference.
- Check:
  - **Prefix**
  - **Length** of hash
  - **Encoding type** (Base64, Hex, etc.)
  - **Research** the system/application where it came from



## 12. Quick Example
If you find:
- `$2b$` = bcrypt  
- `12` = cost factor  
- `abcdefghijklmno` = salt  
- Rest = hash


## 13. Important Tips
- Combine **tools + manual analysis** for accuracy.
- Context helps avoid wrong assumptions.
- Prefix is the easiest clue in Unix/Linux hashes.
- For Windows, focus on length & location.

---

# 🔓 Cracking Hashes with Salt

## 1. How Salts Affect Cracking
- **Without salt** → Rainbow tables (precomputed hash-password lists) can be used easily.
- **With salt** → Rainbow tables become useless, because:
  - Salt changes the hash result even if password is same.
  - Each hash must be cracked separately.
- **Hashes ≠ Encryption** → You **cannot decrypt** a hash.



## 2. Cracking Process (with Salt)
1. Take a guess password from a wordlist (e.g., `rockyou.txt`).
2. Add the salt (if provided) to the guess.
3. Hash the salted password using the **same algorithm**.
4. Compare with the target hash.
5. If it matches → ✅ Password found.



## 3. Tools for Cracking
- **[Hashcat](https://hashcat.net/hashcat/)** →  
  - Fast.
  - Supports **GPU acceleration**.
- **[John the Ripper](https://www.openwall.com/john/)** →  
  - Uses CPU by default.
  - Works out-of-the-box in most systems.



## 4. Why GPUs Are Useful
- GPUs have **thousands of small cores** → Can run many hash calculations in parallel.
- Great for fast cracking of:
  - MD5
  - SHA1
  - NTLM
- Some algorithms (e.g., **Bcrypt**) are **intentionally slow** → GPU gives no big advantage.



## 5. Cracking on Virtual Machines
- **VMs** usually can’t use your real GPU unless you set up **GPU passthrough** (complicated).
- Cracking in a VM = **slower**:
  - CPU only
  - Virtualisation overhead
- **Best Practice:**
  - Run **Hashcat on the host OS** to use full GPU power.
  - John the Ripper works fine on a VM (CPU only), but is still faster on the host.



## 6. Hashcat Basics

### **Syntax:**
```bash
hashcat -m <hash_type> -a <attack_mode> <hashfile> <wordlist>
```
**Arguments:**
- -m <hash_type> → Number for hash type

- Example:

1. 1000 = NTLM
2. 3200 = Bcrypt
- Check official docs: Hashcat Example Hashes
- -a <attack_mode> → Attack mode:
  1. 0 → Straight mode (wordlist attack)
  2. 3 → Brute force (try all combinations)
- <hashfile> → File containing target hash(es).
- <wordlist> → File with password guesses (e.g., rockyou.txt).

**Example:**
``` bash
# Crack a bcrypt hash with rockyou.txt
hashcat -m 3200 -a 0 hash.txt /usr/share/wordlists/rockyou.txt
# Crack a SHA2 -256 Hash with rockyou.txt
hashcat -m 1400 -a 0 Hashing-Basics/Task-6/hash2.txt rockyou.txt
```
**Meaning:**
- -m 3200 → Bcrypt hash type.
- -a 0 → Straight wordlist attack.
- hash.txt → File containing target hash.
- /usr/share/wordlists/rockyou.txt → Wordlist of guesses.
## 7. Important Tips
- Salted hashes are safer → Take longer to crack.
- GPU cracking works best on host machine.
- Always confirm hash type before cracking → Wrong type wastes time.
- Use rockyou.txt for common passwords in practice labs.
- Strong password + long random salt → May take years to crack.
---

# 📌 Hashing for File Integrity & HMAC 


## 1. Integrity Checking with Hashing

**Purpose:** To check if a file has been changed or not.

### How it works:
- Same input → Same hash output.
- Even **1-bit change** → Completely different hash.

### Use Case:
- When you download a file (e.g., Fedora ISO), the website gives you a hash (e.g., SHA256).
- You run `sha256sum` on your downloaded file.
- If both hashes **match** → ✅ File is authentic and unmodified.
- If hashes **don’t match** → ⚠ File is corrupted or tampered with.

**Example:**
```bash
# Check the hash of a downloaded file
sha256sum Fedora.iso
```
**Output example:**
``` bash
8d3cb4d99f27eb932064915bc9ad34a7529d5d073a390896152a8a899518573f
```
If this matches the official site’s hash → ✅ File is safe.
## 2. Finding Duplicate Files with Hashing
- If two files have exactly the same hash, they are exactly the same file.
- Use case: Cleaning storage by detecting duplicate files.
**Example:**
``` bash
sha256sum file1.txt file2.txt
```
If both have the same hash → ✅ They are identical.
## 3. HMAC (Keyed-Hash Message Authentication Code)
Purpose:
- Authenticity → Confirm that the sender is genuine.
- Integrity → Confirm the message wasn’t modified.
How it works:
- Combines:
  1. Secret Key → Proves authenticity.
  2. Hashing Algorithm → Proves integrity.
**Steps in HMAC:**
1.Pad the secret key to match the hash function’s block size.
2.XOR padded key with constant 1.
3.Append the message → Hash it.
4.XOR padded key with constant 2 → Hash again.
5.Final result → HMAC value.

**Formula:**

HMAC(K, M) = H( (K ⊕ opad) || H( (K ⊕ ipad) || M ) )
Where:
K = Secret key
M = Message
opad & ipad = Constants for outer & inner padding
H = Hash function (e.g., SHA256)
|| = Concatenate
## 4. Why HMAC is Strong
1. Without the secret key, attacker cannot generate the correct HMAC even if they know the message.
2. Used in:
- Secure API authentication
- Digital signatures
- Network security protocols (e.g., TLS, IPsec)
 **Quick Example in Python**
``` bash
 import hmac, hashlib

key = b'secret123'
message = b'Hello World'


hmac_value = hmac.new(key, message, hashlib.sha256).hexdigest()
print(hmac_value)
```
 Output (example):
 
88d4266fd4e6338d13b845fcf39a1ddc0c9c0f6e...
## ✅ Key Points to Remember
- Hashing checks integrity but not authenticity.
- HMAC checks both integrity and authenticity (because of secret key).
- Always use secure hash functions (SHA256 or better) for file integrity and HMAC.
