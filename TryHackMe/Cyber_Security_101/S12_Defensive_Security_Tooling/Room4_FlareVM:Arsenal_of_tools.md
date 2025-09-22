# FlareVM 

FlareVM = **Forensics, Logic Analysis, and Reverse Engineering Virtual Machine**  
Ye ek Windows-based toolkit hai jo malware analysis, incident response aur reverse engineering ke liye ready-made tools ka collection hai.  
Tumhe alag alag software install karne ki zarurat nahi – FlareVM me sab ek hi jagah pe milte hain.  



## 🧩 Categories of Tools

### 1. Reverse Engineering & Debugging
- **Goal**: Understand how a program/malware works by breaking it down.  
- **Tools**:
  - **Ghidra** → NSA ka open-source reverse engineering suite.
  - **x64dbg** → Debugger for 32/64-bit programs.
  - **OllyDbg** → Assembly-level debugger.
  - **Radare2** → Advanced reverse engineering platform.
  - **Binary Ninja** → Disassembler + decompiler tool.
  - **PEiD** → Detect compiler, packer, cryptor used in file.

🔑 **Example**: Malware crash ho raha hai. Analyst x64dbg me uska step-by-step execution dekh ke bug/malware logic samjh leta hai.



### 2. Disassemblers & Decompilers
- **Goal**: Convert binary/machine code into human-readable code.
- **Tools**:
  - **CFF Explorer** → Edit & analyze PE files.
  - **Hopper Disassembler** → Debug + disassemble + decompile.
  - **RetDec** → Open-source decompiler.

📊 **Comparison**:

| Tool            | Use Case                                   |
|-----------------|---------------------------------------------|
| CFF Explorer    | PE file analysis (Windows executables)      |
| Hopper          | Disassemble & decompile (Mac/Linux/Win)     |
| RetDec          | Convert machine code → pseudo source code   |



### 3. Static & Dynamic Analysis
- **Static Analysis** = File ko run kiye bina check karna.  
- **Dynamic Analysis** = File ko run karke uska behavior observe karna.

- **Tools**:
  - **Process Hacker** → See running processes & memory.
  - **PEview** → Inspect PE file structure.
  - **Dependency Walker** → Shows DLLs used by an exe.
  - **DIE (Detect It Easy)** → Detect packer, compiler.

📝 **Example**:  
- Malware run kiye bina PEview me check kar sakte ho ki usme suspicious imports hain ya nahi.  
- Agar run karoge to Process Hacker me uske memory changes track kar sakte ho.



### 4. Forensics & Incident Response
- **Goal**: Collect & analyze evidence after a cyberattack.  
- **Tools**:
  - **Volatility** → Memory dump analysis (RAM forensics).
  - **Rekall** → Another memory forensics framework.
  - **FTK Imager** → Capture disk images for evidence.

📊 **Example**: Attack ke baad RAM dump liya → Volatility se check kar sakte ho ki attacker ne kaunse processes aur passwords access kiye.



### 5. Network Analysis
- **Goal**: Study malicious communication in networks.  
- **Tools**:
  - **Wireshark** → Capture & analyze network packets.
  - **Nmap** → Scan network & find vulnerabilities.
  - **Netcat** → Send/receive data over network.

📝 **Example**: Agar malware kisi unknown server se connect ho raha hai, to Wireshark me uska packet capture analyze kar ke IP/Domain pata kar sakte ho.


### 6. File Analysis
- **Goal**: Inspect binary files.  
- **Tools**:
  - **FileInsight** → Edit & analyze binary files.
  - **Hex Fiend** → Lightweight hex editor.
  - **HxD** → Binary/hex file editor.



### 7. Scripting & Automation
- **Goal**: Automate repetitive analysis tasks.  
- **Tools**:
  - **Python** → Write scripts for automation.
  - **PowerShell Empire** → Post-exploitation framework.



### 8. Sysinternals Suite
- **Goal**: Monitor & troubleshoot Windows system behavior.  
- **Tools**:
  - **Autoruns** → Shows programs that auto-start with Windows.
  - **Process Explorer** → Detailed process info (like Task Manager++).  
  - **Process Monitor** → Logs all file/registry/network activities.



## 📦 Key Idea of FlareVM
- It’s like a **Swiss Army Knife** 🗡️ for malware analysts.  
- Instead of manually collecting 100s of tools, you get **all-in-one package** ready to use.  
- Useful for:
  - Reverse Engineers
  - Incident Responders
  - Digital Forensics Experts
  - Penetration Testers

---
# 🔍 FlareVM Investigation Tools 

These are the **basic tools** used in initial investigations (malware analysis, system monitoring, forensic checks).



## 🖥️ Process Monitor (Procmon)
- **What it does**: Real-time monitoring of system activity (files, registry, processes).
- **Use case**: Helpful for **malware research, troubleshooting, forensic investigations**.
- **Example**:
  - If `lsass.exe` (a Windows security process) is reading unusual files, it could mean **credential dumping attack** (e.g., Mimikatz trying to steal passwords).
  - Normally LSASS is safe, but unusual activity = red flag.

<img width="665" height="104" alt="Screenshot 2025-09-22 at 11 11 21 AM" src="https://github.com/user-attachments/assets/dab60011-37ee-475d-aa87-e12c51997483" />


## ⚙️ Process Explorer (Procexp)
- **What it does**: Shows **running processes, parent-child relationships, DLLs loaded**.
- **Use case**: Helps track which process launched another.
- **Example**:
  - If `word.exe` launches `powershell.exe` → suspicious (common attack technique with malicious docs).
  - We can check process path, ID, and linked DLLs.
<img width="668" height="513" alt="Screenshot 2025-09-22 at 11 11 39 AM" src="https://github.com/user-attachments/assets/e2256a3f-a2d9-4199-81d3-1890bbd9e3ff" />

📊 **Procmon vs Procexp**:

| Tool       | Focus                                  |
|------------|----------------------------------------|
| Procmon    | Real-time system activity (deep logs)  |
| Procexp    | Process tree (who started what)        |



## 🔢 HxD (Hex Editor)
- **What it does**: Opens files in **hexadecimal (raw data)** format.
- **Use case**: Forensics, malware file inspection, binary editing.
- **Key Point**: If a file starts with `4D 5A` → it's a **Windows executable (MZ header)**.
- **Example**:
  - Open `possible_medusa.txt` → actually contains `MZ` header → not a text file, but an EXE file disguised as `.txt`.

<img width="672" height="388" alt="Screenshot 2025-09-22 at 11 11 55 AM" src="https://github.com/user-attachments/assets/75d270bf-3af2-4fc4-b775-80b82b845f4d" />


## 📂 CFF Explorer
- **What it does**: Inspect and edit **PE (Portable Executable) files**.
- **Use case**:
  - Generate file hashes (MD5, SHA1) → check file integrity.
  - Verify source of executables.
- **Example**:
  - Investigating `cryptominer.bin` → CFF Explorer shows:
    - File created on Sep 23, 2024
    - 64-bit PE file
    - Hashes available for verification
  - Useful to confirm if file is modified or legit.

<img width="673" height="599" alt="Screenshot 2025-09-22 at 11 12 11 AM" src="https://github.com/user-attachments/assets/d3478da6-d69c-4f8f-a3bf-58371e67bdb5" />


## 🌐 Wireshark
- **What it does**: Captures & analyzes **network traffic**.
- **Use case**:
  - Detect suspicious communication (C2 servers, exfiltration).
  - Examine protocols (HTTP, DNS, TLS, TCP).
- **Example**:
  - Captured traffic shows `TLSv1.2` encrypted packets.
  - Could be safe OR attacker hiding malicious communication inside encryption.

<img width="676" height="510" alt="Screenshot 2025-09-22 at 11 12 30 AM" src="https://github.com/user-attachments/assets/926e720b-7f37-4654-b5b8-081df42adf9a" />


## 🧪 PEStudio
- **What it does**: **Static analysis** → inspect EXE without running it.
- **Use case**:
  - Check for suspicious imports, entropy (packing), digital signature.
- **Example**:
  - Analyzed `PsExec.exe` → normally an admin tool.
  - But attackers also use it for **remote code execution** in post-exploitation.
  - High entropy (6.5) may indicate packing/encryption.

⚠️ Dual-Use: A file can be **legit + dangerous** (depends on context).

<img width="673" height="457" alt="Screenshot 2025-09-22 at 11 12 43 AM" src="https://github.com/user-attachments/assets/e342e1f2-18cb-493c-9136-623ff912f3b2" />


## 🔓 FLOSS (FireEye Labs Obfuscated String Solver)
- **What it does**: Extracts & de-obfuscates **hidden strings** in malware.
- **Use case**:
  - Find hardcoded URLs, IPs, file paths, encryption keys inside binaries.
- **Example**:
PS > floss .\cobaltstrike.exe
→ Extracted 189 static strings
→ Found URLs, registry keys, API calls
- If malware uses obfuscation, FLOSS tries to decode hidden strings.

📊 **FLOSS vs strings.exe**:
- `strings.exe` = basic, finds readable text only.
- `FLOSS` = advanced, also tries to decode obfuscated strings.



# 📦 Quick Recap

| Tool          | Main Job                          | Example Use Case |
|---------------|----------------------------------|------------------|
| Procmon       | Log system activity              | Detect LSASS abuse |
| Procexp       | Process tree & DLLs              | Find parent-child processes |
| HxD           | Hex editor                       | Detect hidden EXE in TXT |
| CFF Explorer  | PE file analysis + hashes        | Check cryptominer file |
| Wireshark     | Network traffic analysis         | Spot suspicious TLS comms |
| PEStudio      | Static malware analysis          | Detect packed/malicious EXE |
| FLOSS         | Extract obfuscated strings       | Find hidden URLs/IPs |

---

# Malware Analysis Notes (FlareVM Task)

## 📌 Scenario
- A suspicious file **windows.exe** was downloaded by a user:
  - Date/Time → 09/24/2024 at 3:43 AM  
  - Location → C:\Users\Administrator\Desktop\Sample
- Task → Perform static + dynamic analysis.



## 🔎 Part 1 – Static Analysis with PEStudio

### 1. Hash Values
- MD5  → 9FDD4767DE5AEC8E577C1916ECC3E1D6
- SHA1 → A1BC55A7931BFCD24651357829C460FD3DC4828F

👉 These can be checked on **VirusTotal**.  
If no detections → high chance of new malware.
<img width="674" height="431" alt="Screenshot 2025-09-22 at 11 15 34 AM" src="https://github.com/user-attachments/assets/b088de4e-f9bd-4d23-8758-8cac7b76945b" />



### 2. Suspicious Details
- File pretends to be → **Registry Editor (regedit.exe)**
- Real regedit.exe → Located in **C:\Windows\System32**
- This file → Located in **Downloads/Desktop folder**
- ❌ Fake & suspicious

<img width="680" height="291" alt="Screenshot 2025-09-22 at 11 16 01 AM" src="https://github.com/user-attachments/assets/f9391a24-ed52-4fcd-8c6a-6b1550824d47" />


### 3. Version Info
- Russian text found → "Редактор реестра" (Registry Editor)
- If company is not Russian → Suspicious attribution
- May indicate **Russian origin**



### 4. Rich Header Missing
- Rich Header not present → File might be **packed/obfuscated**
- Malware often uses packing to avoid detection.



### 5. API Imports (IAT)
Some blacklisted functions found:

| Function Name       | Meaning | Why Suspicious |
|---------------------|---------|----------------|
| set_UseShellExecute | Runs processes via OS shell | Malware often spawns child processes |
| CryptoStream        | Used in encryption | Possible file/network encryption |
| RijndaelManaged     | AES algorithm (Rijndael) | Could mean ransomware or encrypted comms |
| CipherMode          | Crypto configuration | Used in secure channels or file encryption |
| CreateDecryptor     | Decrypt data | Possible data hiding or ransomware decryption |



## 🔎 Part 2 – String Analysis with FLOSS

### Steps
1. Go to folder → `C:\Users\Administrator\Desktop\Sample`
2. Run:
   FLOSS.exe .\windows.exe > windows.txt
<img width="1260" height="253" alt="Screenshot 2025-09-22 at 11 16 44 AM" src="https://github.com/user-attachments/assets/da174da0-a81c-4443-ae8b-6a188075b4c6" />

### Output
- Warning → It's a **.NET file**, so FLOSS can't deobfuscate fully.
- Still, it extracted **same crypto-related functions** (CryptoStream, RijndaelManaged, etc.).
- Confirms what we saw in PEStudio.



## 🔎 Part 3 – Dynamic Analysis with Process Explorer

### File: cobaltstrike.exe
1. Run cobaltstrike.exe
2. Open **Process Explorer**
3. Observe:
   - Parent process → Explorer.exe
   - Child process → cobaltstrike.exe
   - Process ID → Example: 4756
4. Right-click → Properties → **TCP/IP tab**
   - Shows **network connection** to external address

<img width="1269" height="136" alt="Screenshot 2025-09-22 at 11 17 06 AM" src="https://github.com/user-attachments/assets/65586fe7-9392-4b2f-8bc1-8da1e9b2ab22" />

<img width="1260" height="236" alt="Screenshot 2025-09-22 at 11 17 24 AM" src="https://github.com/user-attachments/assets/c97ac42b-3013-4757-a521-ef33a9e4d1f2" />

## 🔎 Part 4 – Dynamic Analysis with Process Monitor (Procmon)

### Why Procmon?
- Double-check results (never trust only one tool).

### Steps
1. Open Procmon
2. Press **CTRL + L** → Open filter
3. Create filter:
   - Process Name → contains → cobalt
   - Click **Include**, then **Add** → Apply
4. Now only cobaltstrike.exe events are visible.

### Result
- cobaltstrike.exe connects to IP:
  **47.120.46.210**  
- This is likely a **C2 server** (Command & Control).



## ✅ Key Takeaways
1. **windows.exe**
   - Fake regedit
   - Packed/obfuscated
   - Russian metadata
   - Uses AES encryption
   - Likely ransomware / data stealer

2. **cobaltstrike.exe**
   - Creates child process
   - Connects to external IP → 47.120.46.210
   - Classic behavior of a **Cobalt Strike beacon**

3. Use multiple tools (PEStudio, FLOSS, Process Explorer, Procmon) for accuracy.
