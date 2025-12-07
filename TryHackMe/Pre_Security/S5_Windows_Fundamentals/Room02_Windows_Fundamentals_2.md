# 🔧 System Configuration & Computer Management

Think of this like a short, friendly guide so you can remember everything fast.  
I’ll explain what each tool does, why it matters, and show small examples.

---

## 1) MSConfig (System Configuration) — What is it?
MSConfig helps you **troubleshoot startup problems**.  
You need to be an **Administrator** to open it.

You can open it from the Start Menu by searching **System Configuration**.

MSConfig has five tabs:
- **General** — choose how Windows starts (Normal / Diagnostic / Selective)
- **Boot** — change boot options (safe mode, timeout, etc.)
- **Services** — list of every service (running or stopped)
- **Startup** — on servers this may be empty; Windows suggests using Task Manager
- **Tools** — quick links to many Windows tools (Event Viewer, msinfo32, etc.)

**Tip:** On Windows Server, Startup items are usually handled differently. To see user startup shortcuts:
Open Run and type:
shell:startup

yaml
Copy code
This folder shows programs that run when a user logs in.
<img width="818" height="641" alt="Screenshot 2025-12-07 at 7 21 37 PM" src="https://github.com/user-attachments/assets/edc88508-c90f-404d-9089-c4e76dd32a5b" />

<img width="551" height="382" alt="Screenshot 2025-12-07 at 7 21 49 PM" src="https://github.com/user-attachments/assets/b8b243b4-e3c5-4ce9-9170-bde6c35ca0f1" />
<img width="558" height="379" alt="Screenshot 2025-12-07 at 7 22 07 PM" src="https://github.com/user-attachments/assets/3497061d-e38d-41d8-83bf-71147d234bde" />
<img width="544" height="372" alt="Screenshot 2025-12-07 at 7 22 22 PM" src="https://github.com/user-attachments/assets/32ebc8c1-23b8-48d0-b717-b86b00799c1e" />
<img width="545" height="378" alt="Screenshot 2025-12-07 at 7 22 32 PM" src="https://github.com/user-attachments/assets/44202a65-c207-4252-b8f8-7be301ff2af2" />
<img width="705" height="342" alt="Screenshot 2025-12-07 at 7 22 44 PM" src="https://github.com/user-attachments/assets/f1c2f875-a9b7-42f8-8f65-8c96c88afc13" />
<img width="544" height="380" alt="Screenshot 2025-12-07 at 7 22 53 PM" src="https://github.com/user-attachments/assets/4c261df9-b9c6-4e12-adf5-120ad4934181" />

---

## 2) Advanced System Settings
Search **View advanced system settings** to open System Properties.
<img width="460" height="529" alt="Screenshot 2025-12-07 at 7 23 05 PM" src="https://github.com/user-attachments/assets/059006aa-8a1b-471c-92de-504aa9ac9d72" />

### Performance → Settings
- Shows options about visual effects and performance.
- **Advanced** tab shows the **Page file** (virtual memory).
  - You can see: which drive has page file, initial size, max size, or if Windows manages it.
<img width="459" height="528" alt="Screenshot 2025-12-07 at 7 23 15 PM" src="https://github.com/user-attachments/assets/ee082eae-1bbb-4ddd-b95e-2de3dc574de5" />
<img width="478" height="661" alt="Screenshot 2025-12-07 at 7 23 26 PM" src="https://github.com/user-attachments/assets/cfbaa505-8c85-45bc-9658-f1931c25329e" />

### Startup and Recovery → Settings
- Windows can save a **crash dump** when the system blue-screens.
- **Write debugging information** dropdown selects what type of dump to save:
  - Automatic memory dump
  - Kernel memory dump
  - Small memory dump (256 KB)
  - Complete memory dump
  - None

Crash dumps help admins investigate why the computer crashed.
<img width="467" height="530" alt="Screenshot 2025-12-07 at 7 23 36 PM" src="https://github.com/user-attachments/assets/7de0e460-6b0c-48be-848a-6531d02a6f1a" />
<img width="459" height="542" alt="Screenshot 2025-12-07 at 7 23 46 PM" src="https://github.com/user-attachments/assets/0550ba05-281d-414e-ac7b-e7fdbf021f4f" />

---

## 3) User Account Control (UAC)
UAC protects the system from apps that try to change system settings.

There is a slider with four levels:

| Level | What it means (simple) |
|-------|------------------------|
| Always notify | Highest security. You are asked every time (desktop dims). |
| Notify for apps | Default. Apps asking for change will prompt you. |
| Notify without dimming | Prompts but screen doesn't dim. |
| Never notify | No prompts — not safe. |

If an app needs admin rights you’ll see a **shield icon** and a UAC prompt.
<img width="653" height="503" alt="Screenshot 2025-12-07 at 7 23 57 PM" src="https://github.com/user-attachments/assets/575f17f2-255f-4204-b6bf-0d2c0e24200c" />

---

## 4) Computer Management (compmgmt)
Computer Management groups useful tools into three sections:
- **System Tools**
- **Storage**
- **Services and Applications**
<img width="372" height="305" alt="Screenshot 2025-12-07 at 7 24 09 PM" src="https://github.com/user-attachments/assets/a7a0e630-5bb2-4fa6-8798-ee92ea0bf797" />

### System Tools
**Task Scheduler**
- Schedule tasks to run automatically (programs, scripts).
- Tasks can run at login, at a time, on an event, or repeatedly.
- To view tasks: Task Scheduler Library → click a task to see triggers and actions.
<img width="872" height="408" alt="Screenshot 2025-12-07 at 7 24 23 PM" src="https://github.com/user-attachments/assets/3b8d5270-8f91-400e-93e2-c6d78adb55a7" />
<img width="237" height="134" alt="Screenshot 2025-12-07 at 7 24 34 PM" src="https://github.com/user-attachments/assets/d3d83f8f-f9a2-471c-8071-7feb8363e441" />

**Event Viewer**
- Shows events (like a system diary). Used to diagnose problems.
- Left pane = log categories. Middle = events list. Right = actions.
- Event types: Information, Warning, Error, Critical, Audit Success/Failure.
- Important logs under **Windows Logs** (Application, Security, System).
<img width="499" height="166" alt="Screenshot 2025-12-07 at 7 25 35 PM" src="https://github.com/user-attachments/assets/a7cf8608-ea61-48bc-ba9a-31b4dede20a7" />
<img width="889" height="441" alt="Screenshot 2025-12-07 at 7 25 50 PM" src="https://github.com/user-attachments/assets/15370eab-40c6-4af6-bbbb-40f528900c72" />
<img width="873" height="312" alt="Screenshot 2025-12-07 at 7 26 01 PM" src="https://github.com/user-attachments/assets/caa06fe2-8ef6-4cd8-8fc2-e80f8de00efd" />

**Shared Folders**
- Shows network shares (like C$, ADMIN$).
- Shows who is connected (Sessions) and what files are open (Open Files).
- Right-click a share → Properties → Security to see who can access it.
<img width="520" height="123" alt="Screenshot 2025-12-07 at 7 26 12 PM" src="https://github.com/user-attachments/assets/20aa48b4-6cd2-4178-afad-21bffac4fc94" />

**Local Users and Groups**
- Tool to manage users and groups (same as lusrmgr.msc).
- Add users, change groups, set account properties.
<img width="645" height="298" alt="Screenshot 2025-12-07 at 7 26 22 PM" src="https://github.com/user-attachments/assets/44333c18-39b4-4124-b38c-27405c2f469e" />

**Performance Monitor (perfmon)**
- Real-time graphs or saved logs for CPU, disk, memory, network.
- Good for troubleshooting slow systems.
<img width="645" height="296" alt="Screenshot 2025-12-07 at 7 26 55 PM" src="https://github.com/user-attachments/assets/f88abd5f-e0b6-49c8-b531-3eb1b9a83aad" />

**Device Manager**
- View and manage hardware (disable devices, update drivers).
<img width="287" height="292" alt="Screenshot 2025-12-07 at 7 27 05 PM" src="https://github.com/user-attachments/assets/7f7cffa5-8d87-4d1d-a758-74a58667d15e" />

---

### Storage
**Disk Management**
- Create/format partitions, assign drive letters, extend or shrink volumes.
- Use for adding a new disk or changing drive letters (e.g., E:).
<img width="765" height="209" alt="Screenshot 2025-12-07 at 7 27 19 PM" src="https://github.com/user-attachments/assets/24ff2522-27d3-4dd0-b6cd-b8cbb4bea41c" />

---

### Services and Applications
**Services**
- Background programs (services) are listed with Status and Startup Type.
- Right-click → Properties to see executable path and change startup type.

| Startup Type | Meaning |
|--------------|---------|
| Automatic | Starts at boot |
| Manual | Starts when needed (or by other programs) |
| Disabled | Won’t start |
<img width="881" height="383" alt="Screenshot 2025-12-07 at 7 27 32 PM" src="https://github.com/user-attachments/assets/b6fb1bc4-7768-40c4-807d-5ab69a22a5c6" />

**WMI Control**
- Controls Windows Management Instrumentation (WMI), used for scripts and remote control.
- WMIC (old tool) is deprecated; PowerShell is preferred now.

---

## 5) System Information (msinfo32)
<img width="900" height="151" alt="Screenshot 2025-12-07 at 7 28 01 PM" src="https://github.com/user-attachments/assets/abe14e73-7f41-4faa-99d0-9707c6520b2e" />

Msinfo32 shows a full summary of hardware, components, and software environment.

Three big parts:
- **Hardware Resources** — low-level hardware info (not for beginners)
<img width="224" height="174" alt="Screenshot 2025-12-07 at 7 28 39 PM" src="https://github.com/user-attachments/assets/6235f4aa-a507-48a3-ac00-05d83894018e" />

- **Components** — devices like Display, Input, Network
<img width="182" height="510" alt="Screenshot 2025-12-07 at 7 29 05 PM" src="https://github.com/user-attachments/assets/d37c2ac0-c12d-48e4-b715-51a1ee12071e" />


- **Software Environment** — drivers, running tasks, environment variables, network connections
 <img width="210" height="313" alt="Screenshot 2025-12-07 at 7 29 33 PM" src="https://github.com/user-attachments/assets/c27d6bea-b8e9-47ce-80c3-2a4c6f3ffbdb" />


You can open it by searching **System Information**.

**To search for IP address inside msinfo32:**
1. Open System Information (msinfo32).
2. In the left pane click **Components**.
3. Under Components expand **Network** then **Adapter** or **TCPIP** (depends on Windows).
4. Use the search box at the bottom: type **IP Address** and press Enter.
5. It will show any entries that contain IP addresses in the right pane.

This is handy when you need to find the machine’s configured IPs quickly.

---

## 6) Environment Variables
Environment variables store system information like where Windows is installed or temp folder location.
<img width="989" height="645" alt="Screenshot 2025-12-07 at 7 29 55 PM" src="https://github.com/user-attachments/assets/0eba762b-655b-4d73-85c7-11fa4d9ba391" />

Common ones:
- **%windir%** — Windows installation folder (usually C:\Windows)
- **PATH** — list of folders where programs are found
- **TEMP** / **TMP** — temporary folder locations

Where to view:
- Control Panel → System and Security → System → Advanced system settings → Environment Variables  
OR
- Settings → System → About → Advanced system settings → Environment Variables

Programs read these to find system files or locations.
<img width="579" height="540" alt="Screenshot 2025-12-07 at 7 30 06 PM" src="https://github.com/user-attachments/assets/56d5f1b7-3415-4dd0-bbde-5d27bf840ed4" />
<img width="988" height="504" alt="Screenshot 2025-12-07 at 7 30 23 PM" src="https://github.com/user-attachments/assets/bbfd636b-894c-40ed-9783-f4f74a02b97f" />

---

## 7) Quick Commands / Places to Remember
(Write them exactly like this in Run or search)

msconfig
taskmgr
compmgmt.msc
msinfo32
lusrmgr.msc
shell:startup
perfmon
eventvwr
services.msc
diskmgmt.msc

yaml
Copy code

---

## 8) Short Summary Table (One-liner each)

| Tool | Quick use |
|------|-----------|
| MSConfig | Troubleshoot startup options |
| Advanced System Settings | Page file and crash dump settings |
| UAC | Control when Windows asks for approval |
| Task Scheduler | Run tasks on a schedule |
| Event Viewer | See system logs and errors |
| Shared Folders | View network shares and sessions |
| Performance Monitor | Monitor CPU/RAM/disk over time |
| Device Manager | Manage hardware devices |
| Disk Management | Manage disks and partitions |
| Services | Control background services |
| msinfo32 | Full system information + search IPs |
| Environment Variables | System paths and special locations |

---

## Final Tip (Desi advice)
- If Windows is slow: check Task Manager, then Services, then Perfmon.  
- If something won’t start at login on a server: check the Startup folder (shell:startup) and Task Scheduler.  
- For crashes: check Event Viewer and Startup and Recovery crash dump settings.

---
# 🖥️ Windows Tools (Easy Notes) — Resource Monitor, CMD, Netstat, Net Commands & Registry  
### (Simple English, Desi Style, Easy to Remember)

---

# 1) 🧠 What is Resource Monitor (resmon)?
Think of **Resource Monitor** like a “doctor” for Windows.  
It shows what your computer is doing **right now** — which program is using CPU, RAM, Disk, Network.
<img width="988" height="629" alt="Screenshot 2025-12-07 at 7 30 51 PM" src="https://github.com/user-attachments/assets/ce149ac7-8c6f-490b-992e-c4eb6c6ac8d8" />

Microsoft says it can:
- Show CPU, memory, disk, network usage **per process**
- Show which files a program is using
- Show which services are running
- Help detect stuck apps (deadlock)
- Let you stop/pause/resume services
- Close unresponsive apps

### ⭐ Tabs in Resource Monitor
There are 4 main sections (you will also see a right-side live graph):

| Tab | Simple Meaning |
|-----|----------------|
| CPU | Which apps are using the processor |
| Memory | Which apps are using RAM |
| Disk | Which apps are reading/writing files |
| Network | Which apps are using Internet/LAN |

Each tab gives extra details like:
- Process name  
- PID  
- Usage %
- File handles (which files the program opened)

---

# 2) 💻 Command Prompt (cmd) — Easy Beginner Notes
CMD looks scary, but it is actually simple once you know basic commands.

Earlier computers only had command-line.  
Now we use GUI (mouse + icons), but cmd still exists for troubleshooting.

Below are some commands you SHOULD know.

---

## 🏷️ Basic Commands

### ✔️ hostname  
Shows the **computer’s name**.
Example:
hostname

markdown
Copy code

### ✔️ whoami  
Shows **your logged-in username**.
whoami

yaml
Copy code

---

## 🌐 Network Troubleshooting Commands

### ✔️ ipconfig  
Shows your system’s **IP address**, **gateway**, **DNS**, etc.
ipconfig

css
Copy code

To see full details:
ipconfig /all

bash
Copy code

To see help for any command, use:
ipconfig /?

yaml
Copy code

---

### ✔️ netstat  
Shows:
- Which programs are connected to the internet  
- Ports in use  
- Network statistics  

Example:
netstat

yaml
Copy code

Netstat supports many parameters:
netstat -a
netstat -b
netstat -n

yaml
Copy code

Meaning:
- **-a** → show all connections  
- **-b** → show which program created the connection  
- **-n** → show IP addresses in numeric form  

---

### ✔️ net command  
Used to manage **network resources**.  
This command has **subcommands** (like chapters inside a book).

If you type only:
net

shell
Copy code
It shows a list of available options.

### ❗Important: Help command is DIFFERENT here  
/ ? will NOT work.

Correct way:
net help

pgsql
Copy code

To get help for `net user`:
net help user

yaml
Copy code

Useful subcommands:
net user
net localgroup
net use
net share
net session

yaml
Copy code

They help you manage:
- User accounts  
- User groups  
- Network drives  
- Shared folders  

---

## 🧹 Clean the screen  
cls

yaml
Copy code

---

# 3) 🏛️ Windows Registry (regedit)
The **Registry** is like Windows’ "brain storage".  
It stores all important system settings.

Microsoft says the registry stores info like:
- User profiles  
- Installed applications  
- Folder display settings  
- Hardware information  
- Used ports  
<img width="482" height="194" alt="Screenshot 2025-12-07 at 7 31 22 PM" src="https://github.com/user-attachments/assets/2d0f00c7-fbed-41d3-b222-a3345c5ad772" />

⚠️ **Warning:**  
Editing the registry wrongly can BREAK Windows.  
Only touch it if you KNOW what you are doing.

### How to open Registry Editor:
regedit

yaml
Copy code

Inside you will see 5 main “hives” (big folders):

| Hive | Meaning |
|------|---------|
| HKEY_LOCAL_MACHINE | System-wide settings |
| HKEY_CURRENT_USER | Settings for the logged-in user |
| HKEY_CLASSES_ROOT | File type associations |
| HKEY_USERS | Profiles of all users |
| HKEY_CURRENT_CONFIG | Hardware profile |

Most settings Windows uses daily come from the Registry.

---

# 🧾 Summary Table (Very Simple)

| Tool | What it Does |
|------|--------------|
| Resource Monitor | Live info about CPU, RAM, Disk, Network |
| CMD | Text-based tool to run commands |
| ipconfig | Shows IP settings |
| netstat | Shows network connections |
| net help user | Shows user management help |
| cls | Clears screen |
| regedit | Opens Windows Registry |

---

# 🧠 Super Short Memory Trick
- **resmon** → “Resource Monitor = system activity live graphs”  
- **hostname** → “Machine name”  
- **whoami** → “My username”  
- **ipconfig** → “My IP address”  
- **netstat** → “Who is connected?”  
- **net** → “Network commands for users, shares”  
- **regedit** → “Windows brain settings”  

---
