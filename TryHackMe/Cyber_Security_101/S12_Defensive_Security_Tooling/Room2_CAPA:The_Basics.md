# Static Analysis and CAPA 

## 1. Static vs Dynamic Analysis

| Feature              | Static Analysis                          | Dynamic Analysis                          |
|----------------------|------------------------------------------|-------------------------------------------|
| Definition           | Studying a file *without running it*     | Studying a file *while running it*         |
| Risk                 | Safe (file not executed)                 | Risky (file executes, may harm system)     |
| Tools Used           | CAPA, strings, PEview, IDA, Ghidra       | Sandboxes, debuggers, Process Monitor      |
| Example              | Checking malware code functions          | Running malware in sandbox to see actions  |

👉 Example:  
- **Static Analysis** = Like reading the recipe of a dish.  
- **Dynamic Analysis** = Like actually cooking and tasting it.



## 2. What is CAPA?

- **CAPA = Common Analysis Platform for Artifacts**  
- Developed by **Mandiant (FireEye Team)**.  
- Analyzes executables like:
  - PE (Portable Executable – Windows EXE/DLL)
  - ELF binaries (Linux)
  - .NET modules
  - Shellcode
  - Even sandbox reports  

👉 CAPA uses **rules** to detect what a file can do.  
It does NOT just say "this is malware" but says **what actions the program is capable of**.



## 3. What CAPA Detects (Examples)

| Capability Detected             | What It Means in Simple Words         |
|---------------------------------|---------------------------------------|
| Network Communication           | Program may connect to internet       |
| File Manipulation               | Program may read, write, delete files |
| Process Injection               | Program may insert code into another process |
| Persistence                     | Program may stay active after reboot  |
| Encryption / Decryption         | Program may encrypt or decrypt data   |

👉 Example:  
If CAPA finds "Network Communication" → malware may try to contact its Command & Control (C2) server.  



## 4. Why CAPA is Useful?

- **Encapsulates years of reverse engineering knowledge** (so beginners can use it).  
- Saves time for analysts and defenders.  
- Helps in **malware analysis, threat hunting, incident response**.  

👉 Analogy:  
- Without CAPA = You manually read and understand a book in another language.  
- With CAPA = You use Google Translate that instantly tells you the meaning.  



## 5. How CAPA Helps in Real Life

- Malware analyst quickly finds:
  - "This malware can steal passwords"
  - "This malware tries to connect to the internet"
- Incident responder:
  - Blocks malware actions in firewall or EDR.
- Threat hunter:
  - Searches for same behaviors in network logs.  



## 6. CAPA Usage Example

### Step 1: Run CAPA on a file
```
capa suspicious.exe
```
### Step 2: Output (Example)
+-------------------------+-----------------------------------+
| Capability | Details |
+-------------------------+-----------------------------------+
| communicate with server | uses HTTP requests |
| manipulate files | create, delete, write files |
| inject into process | injects into explorer.exe process |
+-------------------------+-----------------------------------+

👉 This means the file is capable of **C2 communication, file operations, and process injection**.



# Quick Recap
- **Static analysis** = no execution, safe → CAPA is used here.
- **Dynamic analysis** = run in sandbox, risky.
- **CAPA** = tells you what a binary *can do* by matching rules.
- Useful for analysts, defenders, hunters.

---

# CAPA Usage and Results 

## 1. How to Run CAPA

### Steps:
1. Open **PowerShell**
   - Sometimes it takes a little while to load.
2. Go to CAPA folder:
```
cd C:\Users\Administrator\Desktop\capa
```
3. Run CAPA on suspicious file (example: cryptbot.bin):
```
capa.exe .\cryptbot.bin
```
👉 It will load rules and analyze program.  
👉 This may take **several minutes**.



## 2. CAPA Useful Options

| Option | Meaning | Example Command |
|--------|---------|-----------------|
| `-h`   | Show help menu | `capa -h` |
| `-v`   | Verbose → gives detailed result | `capa.exe .\cryptbot.bin -v` |
| `-vv`  | Very verbose → maximum details | `capa.exe .\cryptbot.bin -vv` |

👉 More **verbose = more details**, but **slower processing**.



## 3. Example Output (cryptbot.bin)

### Basic File Info
- **Hashes (MD5, SHA1, SHA256)** → Unique IDs of the file  
- **Analysis = static** (means CAPA did not run the file)  
- **OS = Windows**  
- **Format = PE** (Portable Executable → Windows EXE/DLL)  
- **Arch = i386** (32-bit program)  


### ATT&CK Mapping
CAPA links behaviors to **MITRE ATT&CK tactics & techniques**.

| Tactic (Goal of Malware) | Example Technique |
|--------------------------|-------------------|
| **Defense Evasion** | Obfuscation, Sandbox Evasion |
| **Discovery** | File and Directory Discovery |
| **Execution** | Run PowerShell, Shared Modules |
| **Persistence** | Scheduled Tasks |
| **Impact** | Cryptocurrency Mining (Resource Hijacking) |

👉 This tells **what the malware is trying to achieve**.  
Example: Persistence = “stay alive after reboot”.



### MAEC Category
- **launcher** → Means this binary is mainly used to **launch or load other malware/code**.


### MBC Behaviors
MBC = Malware Behavior Catalog (detailed behaviors).  
Some behaviors found:

- **Anti-VM detection** → Checks if running inside VirtualBox/VMWare  
- **Obfuscation** → Hides its code using stack strings or argument tricks  
- **Communication** → HTTP communication (talk to C2 server)  
- **Data manipulation** → Encode with Base64 / XOR  
- **File System** → Create, delete, read, write files  
- **Process** → Create process, allocate memory  
- **Execution** → Run PowerShell scripts  

👉 Basically, malware = **sneaky + communicates online + manipulates files + persists**.



### CAPA Capabilities (Simplified)

| Capability | What It Means in Real Life |
|------------|----------------------------|
| Anti-VM detection | Malware tries to check if it's in a sandbox → to avoid detection |
| Obfuscated strings | Malware hides important strings from analyst |
| HTTP communication | Malware connects to internet (C2 server) |
| Base64 / XOR encoding | Malware hides data or communication |
| File operations (read/write/delete) | Can steal, drop, or modify files |
| Process creation/injection | Can inject into explorer.exe or spawn processes |
| Persistence (scheduled tasks) | Stays active after reboot |
| Crypto strings | Indicates cryptocurrency-related impact (resource hijacking) |



## 4. Alternative Way to View Output

If CAPA takes long time, use the pre-saved file:
```
Get-Content .\cryptbot.txt
```
👉 This shows same output as CAPA run.



# Quick Recap
- **Run CAPA**: `capa.exe .\cryptbot.bin`
- **Options**: `-h`, `-v`, `-vv` for more details.
- **Results show**:
  - File info (hash, OS, arch)
  - ATT&CK mapping (tactics + techniques)
  - Malware category (launcher, etc.)
  - Behaviors (anti-VM, obfuscation, file ops, persistence, comms)
- **Interpretation**: Cryptbot can **hide itself, talk to servers, manipulate files, run PowerShell, and persist**.
---

# Dissecting CAPA Results (cryptbot.bin)

## 1. Basic Information Block

This block gives us file identity and environment details.

| Field     | Meaning | Example from Output |
|-----------|---------|---------------------|
| **md5, sha1, sha256** | Cryptographic hash values. Unique "fingerprints" of the file. Useful to track or compare malware. | md5 = 3b9d26d2e7433749f2c32edb13a2b0a2 |
| **analysis** | How CAPA analyzed the file. | static (file was not executed) |
| **os** | Operating system context. | windows |
| **format** | File type. | pe (Portable Executable → EXE/DLL on Windows) |
| **arch** | Architecture type. | i386 (32-bit program) |
| **path** | Location of file on system. | /home/strategos/Room-CAPA/cryptbot.bin |

👉 Analogy: Think of this block as the "ID card" of the malware file.



## 2. MITRE ATT&CK Block

The **MITRE ATT&CK Framework** is a giant library of all known attacker techniques.  
CAPA maps malware’s behaviour to this framework.  

### Format Breakdown
- **Tactic** = Big goal of attacker  
- **Technique** = Specific method used  
- **Sub-technique** = More detailed version  
- **Identifier** = Unique ID (Txxxx)  

👉 Example:  
`Defense Evasion::Obfuscated Files or Information::T1027`  
- Defense Evasion = Goal (hide from defenders)  
- Obfuscated Files or Information = Method (hide code)  
- T1027 = ID for reference  

`Defense Evasion::Obfuscated Files or Information::Indicator Removal from Tools T1027.005`  
- Adds a **sub-technique** (Indicator Removal from Tools)  
- Identifier = T1027.005  


### Cryptbot MITRE Mapping (Simplified)

| Tactic (Goal)      | Technique(s) Found |
|--------------------|--------------------|
| **Defense Evasion** | Obfuscation [T1027], Sandbox Evasion [T1497.001] |
| **Discovery**       | File and Directory Discovery [T1083] |
| **Execution**       | PowerShell scripts [T1059.001], Shared Modules [T1129] |
| **Impact**          | Resource Hijacking (Crypto mining) [T1496] |
| **Persistence**     | Scheduled Tasks (T1053.002, T1053.005) |

👉 Easy meaning: Cryptbot can **hide itself, spy on files, run PowerShell, hijack resources, and stay active by scheduling tasks**.



## 3. MAEC Block

**MAEC = Malware Attribute Enumeration and Characterization**  
It’s a standardized language to describe malware behaviour.  

### Common MAEC Values
| Value | Meaning | Behaviours |
|-------|---------|------------|
| **Launcher** | Malware acts like a trigger or activator. | Drops payloads, enables persistence, connects to C2, runs functions |
| **Downloader** | Malware fetches other files from internet. | Downloads updates, secondary stages, configs |

### CAPA’s Result
- **cryptbot.bin → launcher**  

👉 Means cryptbot **acts as a launcher**, capable of:  
- Dropping extra malware  
- Enabling persistence  
- Contacting C2 servers  
- Running malicious functions  



# Quick Recap
1. **Basic Info Block** = File’s ID card (hashes, OS, arch, format, path).  
2. **MITRE ATT&CK Block** = Maps malware behaviour to attacker playbook.  
   - Cryptbot hides, discovers files, executes PowerShell, mines crypto, persists.  
3. **MAEC Block** = Tags malware role.  
   - Cryptbot = **Launcher** (drops payloads, persists, connects).  

👉 These results give analysts a **structured understanding** of what cryptbot does, even without running it.
---

# 📌 Malware Behavior Catalogue (MBC) 

## 🔹 What is MBC?
MBC (Malware Behavior Catalogue) is like a **dictionary for malware**.  
It helps analysts **label, classify, and report malware behaviors** in a standard way.

- MBC connects with MITRE ATT&CK but has **extra details for malware-specific stuff**.
- Every malware action is mapped to:
  - **Objective** (big goal, like avoiding detection)
  - **Behavior** (what it actually does)
  - **Method** (how it does that thing)
  - **Identifier** (unique ID like `B0009`)



## 🔹 Two Common Formats

| Format | Example | Breakdown |
|--------|---------|-----------|
| OBJECTIVE::Behavior::Method[ID] | `ANTI-STATIC ANALYSIS::Executable Code Obfuscation::Argument Obfuscation [B0032.020]` | **Objective:** Anti-Static Analysis <br> **Behavior:** Executable Code Obfuscation <br> **Method:** Argument Obfuscation <br> **Identifier:** B0032.020 |
| OBJECTIVE::Behavior::[ID] | `COMMUNICATION::HTTP Communication [C0002]` | **Objective:** Communication <br> **Behavior:** HTTP Communication <br> **Identifier:** C0002 |

👉 First format includes **METHOD (sub-technique)**, second format doesn’t.



## 🔹 Objectives (High-level goals of malware)

| Objective | Explanation |
|-----------|-------------|
| Anti-Behavioral Analysis | Malware tries to avoid detection by fooling sandboxes/debuggers. |
| Anti-Static Analysis | Malware makes its code complex or obfuscated to confuse reverse engineers. |
| Collection | Malware collects system/user info. |
| Command and Control | Malware talks to hacker’s server (C2). |
| Credential Access | Malware steals usernames/passwords. |
| Defense Evasion | Malware hides from antivirus/security tools. |
| Execution | Malware runs malicious code or scripts. |
| Discovery | Malware searches files, directories, system info. |



## 🔹 Micro-Objectives (Smaller goals / low-level focus)

| Micro-Objective | Description |
|-----------------|-------------|
| PROCESS | Deals with processes (create, kill, modify). |
| MEMORY | Deals with memory (allocate, protect, free). |
| COMMUNICATION | Deals with network (DNS, HTTP, FTP etc.). |
| DATA | Deals with strings, encoding, compression, decoding. |

⚡ CAPA shows **Objectives** + **Micro-Objectives** in the "Objective column".



## 🔹 Behaviors vs Micro-Behaviors

- **Behavior** = Bigger action (Ex: Obfuscating files, communicating via HTTP).
- **Micro-Behavior** = Small, low-level action (Ex: Allocating memory, checking a string).

### Example Behaviors:
| Objective | Behavior | ID | Explanation |
|-----------|----------|----|-------------|
| ANTI-BEHAVIORAL ANALYSIS | Virtual Machine Detection | B0009 | Checks if it’s inside Virtual Machine to avoid detection. |
| ANTI-STATIC ANALYSIS | Executable Code Obfuscation | B0032 | Makes code confusing to block analysis. |
| EXECUTION | Command & Script Interpreter | E1059 | Runs malicious commands via cmd.exe, PowerShell, etc. |
| DISCOVERY | File & Directory Discovery | E1083 | Searches for specific files/directories. |

### Example Micro-Behaviors:
| Micro-Objective | Micro-Behavior | ID | Explanation |
|-----------------|----------------|----|-------------|
| MEMORY | Allocate Memory | C0007 | Malware allocates memory to unpack/run itself. |
| PROCESS | Create Process | C0017 | Creates new process (sometimes suspended). |
| COMMUNICATION | HTTP Communication | C0002 | Can initiate HTTP requests. |
| DATA | Check String | C0019 | Reads strings (credit card numbers, ASCII content, etc.). |


## 🔹 Methods (Sub-techniques: How behavior is performed)

| Behavior | Method | ID | Explanation |
|----------|--------|----|-------------|
| Executable Code Obfuscation | Argument Obfuscation | B0032.020 | API arguments calculated at runtime. |
| Executable Code Obfuscation | Stack Strings | B0032.017 | Builds strings in stack → deletes them after use. |
| HTTP Communication | Read Header | C0002.014 | Reads HTTP header. |
| Encode Data | Base64 | C0026.001 | Encodes data using Base64. |
| Encode Data | XOR | C0026.002 | Encodes data using XOR cipher. |
| Obfuscated Files/Info | Encoding-Standard Algorithm | E1027.m02 | Uses standard encoding (like Base64) to hide. |



## 🔹 Example CAPA Result Breakdown

CAPA Output:
Malware Behavior Catalogue
┌─────────────────────────────┬──────────────────────────────────┐
│ MBC Objective │ MBC Behavior │
├─────────────────────────────┼──────────────────────────────────┤
| DATA │ Encode Data::Base64 [C0026.001] │
└─────────────────────────────┴──────────────────────────────────┘

Explanation:
| Label | Value | Meaning |
|-------|-------|---------|
| Objective | DATA | Malware works with data (strings, encoding, compression). |
| Behavior | Encode Data | Malware encodes data. |
| Method | Base64 | Uses Base64 encoding. |
| Identifier | C0026.001 | Unique ID for tracking. |

👉 In simple words: **This malware can encode its data using Base64**.

---


# 📌 CAPA: Capability and Namespace

## 🔹 What is Capability?
- **Capability** = The action malware can perform.  
  Example: "reference anti-VM strings" means malware is checking if it’s inside a virtual machine.



## 🔹 What is Namespace?
- **Namespace** = Category/folder where similar malware behaviors are grouped.
- **Top-Level Namespace (TLN)** = Main category.
- **Sub-Namespaces** = More specific actions inside TLN.

👉 Basically:
Capability (Rule Name)::TLN/Namespace

### Example:
reference anti-VM strings::anti-analysis/anti-vm/vm-detection

Breakdown:
- **Capability (Rule Name):** reference anti-VM strings  
- **TLN (Top-Level Namespace):** anti-analysis  
- **Namespace:** anti-vm/vm-detection  



## 🔹 Top-Level Namespaces (TLN)

| TLN | Explanation |
|-----|-------------|
| anti-analysis | Detects anti-debugging, anti-VM, obfuscation (malware hiding from analysis). |
| collection | Malware collects system/user data for exfiltration. |
| communication | Malware’s network behavior (HTTP, DNS, C2). |
| compiler | Detects compiler/build environment used to make the executable. |
| data-manipulation | How malware transforms/encodes data (encryption, Base64, XOR). |
| executable | PE-specific behaviors (sections like `.tls`). |
| host-interaction | Interaction with OS (file system, processes, registry). |
| load-code | Malware loading additional code (PE parsing, PowerShell, shellcode). |
| persistence | Techniques to stay on system (scheduled tasks, registry autorun). |
| impact | What damage it can cause (cryptocurrency-related, file encryption). |
| linking | Linking functions dynamically at runtime. |



## 🔹 Example from CAPA Output

| Capability | Namespace |
|------------|-----------|
| reference anti-VM strings | anti-analysis/anti-vm/vm-detection |
| contain obfuscated stackstrings | anti-analysis/obfuscation/string/stackstring |
| reference HTTP User-Agent string | communication/http |
| encode data using XOR | data-manipulation/encoding/xor |
| create directory | host-interaction/file-system/create |
| write file on Windows | host-interaction/file-system/write |
| allocate or change RWX memory | host-interaction/process/inject |
| run PowerShell expression | load-code/powershell |
| schedule task via schtasks | persistence/scheduled-tasks |



## 🔹 How Rules Fit Inside TLN → Namespace

Example: **Anti-Analysis TLN**

- **Namespace:** `anti-vm/vm-detection`  
  - Rules:
    - `reference-anti-vm-strings-targeting-virtualbox.yml`
    - `reference-anti-vm-strings-targeting-virtualpc.yml`

- **Namespace:** `obfuscation`  
  - Rules:
    - `obfuscated-with-dotfuscator.yml`
    - `obfuscated-with-smartassembly.yml`

👉 This means:
- TLN = Anti-Analysis  
- Namespace = `anti-vm/vm-detection` or `obfuscation`  
- Rules = Specific `.yml` files inside those namespaces.


## 🔹 Why Namespaces are Useful?
- They **organize malware behaviors** into categories.
- Help analysts quickly understand:
  - What the malware is trying to do
  - Where that action belongs (networking, persistence, file system, etc.)
- Acts like a **folder structure** for malware capabilities.



## 🔹 Recap in One Line
- **Capability = What malware can do.**  
- **Namespace = Where that action belongs (category).**  
- **Top-Level Namespace (TLN) = The big folder.**  
- **Rules (.yml files) = Detailed detection methods inside each namespace.**
---
# 🔎 CAPA Capabilities, Namespaces & Rules 

## 1. What is "Capability"?
- A **capability** = what malware can do.  
- Example:  
  - `reference anti-VM strings` → Malware checks if it is running inside a virtual machine.  
  - `schedule task via schtasks` → Malware tries to schedule itself in Windows tasks for persistence.

👉 Basically, **Capability is the behavior name.**



## 2. Top-Level Namespace (TLN)
- CAPA groups capabilities under **big categories** called **TLNs**.  
- Example TLNs:  
  - `Anti-Analysis` → Rules that help malware avoid detection (like anti-VM).  
  - `Communication` → Rules for network traffic (HTTP, DNS, etc.).  
  - `Persistence` → Rules for staying alive in the system (scheduled tasks, registry autorun, etc.).  
  - `Data-Manipulation` → Rules for encoding/decoding, string changes.  


## 3. Namespace
- Inside a TLN, there are **smaller folders (namespaces)**.  
- Example:  
  - TLN: `Anti-Analysis`  
    - Namespace: `anti-vm/vm-detection`  
  - TLN: `Communication`  
    - Namespace: `http/client`



## 4. Rule YAML File
- Each **capability** is detected by a **YAML rule file**.  
- Rule name = capability name (with dashes instead of spaces).  
  - Example:  
    - Capability → `reference anti-VM strings`  
    - Rule YAML → `reference-anti-vm-strings.yml`



## 5. Example Walkthrough

### Example 1: Detecting Anti-VM Behavior
| Label | Value | Explanation |
|-------|-------|-------------|
| Capability | reference anti-VM strings | Malware tries to detect VMware/VirtualBox to avoid analysis |
| TLN | Anti-Analysis | Category for behaviors that stop/avoid detection |
| Namespace | anti-vm/vm-detection | Rules for detecting VM evasion |
| Rule File | reference-anti-vm-strings.yml | Matches VM-related keywords or checks |



### Example 2: Scheduled Task Persistence
| Label | Value | Explanation |
|-------|-------|-------------|
| Capability | schedule task via schtasks | Malware creates Windows scheduled task |
| TLN | Persistence | Category for long-term survival on system |
| Namespace | scheduled-tasks | Rules for task scheduler |
| Rule File | schedule-task-via-schtasks.yml | Matches schtasks.exe usage |



### Example 3: Base64 Encoding
(From your sample result)

| Label | Value | Explanation |
|-------|-------|-------------|
| Capability | reference base64 string | Malware encodes data with base64 |
| TLN | data-manipulation | Rules for changing/transforming data |
| Namespace | encoding/base64 | Specific rules for encoding (Base64/XOR) |
| Rule File | reference-base64-string.yml | Detects base64 encoding use |



## 6. ⚠️ Exceptions (Important!)
- Sometimes a capability **does not sit under the "expected" namespace**.  
- Example:  
  - `reference cryptocurrency strings`  
  - Should be under `Impact`, **but** it is under **Nursery TLN** (a "testing/placeholder" section for not-yet-finalized rules).



## 7. Key Takeaways
- Capability name = Rule name (with dashes instead of spaces).  
- TLN = Big behavior group (Anti-Analysis, Communication, Persistence, Data-Manipulation, etc.).  
- Namespace = Sub-folder inside TLN for specific behaviors.  
- YAML Rule = Actual detection logic.  
- Exceptions exist (like Nursery TLN).  

---
# 🔍 CAPA Very Verbose (-vv) & Rule Matching Explained

## 1. Why use `-vv`?
- Normal run → only shows the capability (e.g., "detects VM", "creates scheduled task").
- `-vv` (very verbose) → shows **exact condition** that matched inside the malware.
- Helpful for understanding *why* CAPA flagged something.

Example command:
```
capa -vv cryptbot.bin
```
To save results in JSON for web analysis:
```
capa -j -vv cryptbot.bin > cryptbot_vv.json
```


## 2. Problem with Large Output
- `cryptbot_vv.txt` can have 3000+ lines (hard to read in Notepad/terminal).
- Solution → Use **CAPA Web Explorer**.
  - Upload `cryptbot_vv.json`.
  - Explore results in a nice web interface with filters and search.



## 3. Example 1: Anti-VM Detection (Targeting VMware)

### Rule File
reference-anti-vm-strings-targeting-vmware.yml

### Rule Features (simplified)
- CAPA checks if the file contains **VMware-related strings**, such as:
  - "VMWare"
  - "VMTools"
  - "vmci.sys"
  - "vmwareuser.exe"
  - "vm3dgl.dll"
  - "SOFTWARE\\VMware, Inc.\\VMware Tools"

### What it means
If these strings are found:
- Malware is **trying to detect VMware**.
- Purpose: To check if it is running inside a virtual machine (sandbox evasion).


## 4. Example 2: Scheduled Task Persistence

### Rule File
schedule-task-via-schtasks.yml

### Rule Features (simplified)
- CAPA looks for:
  - Process creation + the word "schtasks" + "/create"
  - OR the string "Register-ScheduledTask"

### What it means
If matched:
- Malware is trying to **register itself as a scheduled task**.
- Goal: **Persistence** (survive reboots).



## 5. How CAPA Rules Work (Structure)
Every rule has 3 parts:

| Part       | Purpose                                                |
|------------|--------------------------------------------------------|
| **Meta**   | Rule name, authors, namespace, ATT&CK mapping.         |
| **Scopes** | Where the rule applies (file, function, thread, etc.). |
| **Features** | Actual conditions (strings, regex, API calls).       |


## 6. CAPA Web Explorer Benefits
- Easier than scrolling text.
- Search box → find rules or strings fast.
- Filters → see only Anti-VM, Persistence, Communication, etc.



## 7. Key Idea
- **Normal run** = "Doctor says you have a fever."
- **-vv run** = "Doctor shows thermometer: 102°F, that’s why fever."
- CAPA explains not just *what* malware can do, but also *why* it reached that conclusion.

---
