# Gobuster Room Notes (Simple English)

## 1. What is Gobuster?

Gobuster is a tool used in offensive security to find hidden stuff on websites.

It can do 3 main tasks:

Task | What it does | Example
-----|-------------|--------
Directory Enumeration | Finds hidden folders/files on a website | /admin, /uploads
Subdomain Enumeration | Finds other subdomains of a site | blog.example.com, shop.example.com
Virtual Host Enumeration | Finds extra websites hosted on the same server | site1.example.com, site2.example.com

Think of Gobuster like a smart detective that guesses possible URLs or domains using a wordlist.



## 2. Our Lab Setup

- Web Server: Ubuntu 20.04 VM
- CMS installed: Wordpress + Joomla
- AttackBox: Pre-installed Gobuster, used to run commands
- DNS fix required to resolve machine domains



## 3. Fixing DNS on AttackBox

This step makes sure the AttackBox can find the server domains.

1. Open terminal on AttackBox
2. Edit the file /etc/resolv-dnsmasq
```
sudo nano /etc/resolv-dnsmasq
```
3. Add the machine IP at the top:
```
nameserver MACHINE_IP
nameserver 169.254.169.253
```
> Replace MACHINE_IP with your VM’s IP

4. Save & exit:
CTRL+O → ENTER → CTRL+X

5. Restart DNS service:
```
/etc/init.d/dnsmasq restart
```
6. Check content:
```
cat /etc/resolv-dnsmasq
```
It should look like:
```
nameserver MACHINE_IP
nameserver 169.254.169.253
```
✅ DNS fixed.



## 4. How Gobuster Works 

Gobuster is like guessing hidden doors on a website using a list of possible words.

- Input: Wordlist (list of possible directories/subdomains)
- Target: URL or domain of the web server
- Output: Found directories, subdomains, or virtual hosts

Example:

- Target website: http://example.com
- Wordlist: common.txt (contains /admin, /login, /uploads)

Gobuster will try:

http://example.com/admin
http://example.com/login
http://example.com/uploads

and show which ones exist.



## 5. Common Gobuster Commands

### 5.1 Directory Enumeration

gobuster dir -u http://TARGET -w WORDLIST

- dir → directory mode
- -u → target URL
- -w → wordlist path

Example:
```
gobuster dir -u http://10.10.10.5 -w /usr/share/wordlists/dirb/common.txt
```

### 5.2 Subdomain Enumeration

gobuster dns -d DOMAIN -w WORDLIST

- dns → subdomain mode
- -d → domain
- -w → wordlist path

Example:
```
gobuster dns -d example.com -w /usr/share/wordlists/subdomains.txt

```

### 5.3 Virtual Host Enumeration

gobuster vhost -u http://TARGET -w WORDLIST

- vhost → virtual host mode
- -u → target URL
- -w → wordlist path

Example:
```
gobuster vhost -u http://10.10.10.5 -w /usr/share/wordlists/vhosts.txt

```

## 6. Summary

- Gobuster is a tool for **reconnaissance**
- Can find **directories, subdomains, virtual hosts**
- Works using **wordlists**
- Requires proper DNS setup for local lab
- AttackBox already has Gobuster installed
---
# Gobuster: Overview and Concepts

## 1. What is Gobuster?

Gobuster is an open-source offensive security tool written in Golang.  
It is used to enumerate:

- Web directories
- DNS subdomains
- Virtual hosts (vhosts)
- Amazon S3 buckets
- Google Cloud Storage

It works by **brute-forcing** possible entries from **wordlists** and analyzing responses.  

### Who uses Gobuster?

- Security professionals
- Penetration testers
- Bug bounty hunters
- Cyber security assessments

### Where in ethical hacking?

Gobuster is used between **reconnaissance** and **scanning** phases.



## 2. Key Concepts

### Enumeration

Enumeration is the act of listing all available resources, whether accessible or not.  
Example: Gobuster enumerates web directories like `/admin` or `/uploads`.

### Brute Force

Brute force is trying every possibility until a match is found.  
Example: You have 10 keys and try all until one fits.  
Gobuster uses **wordlists** to perform brute force on directories, subdomains, or virtual hosts.



## 3. Gobuster Help Page

Gobuster comes pre-installed in Kali Linux and AttackBox.  
You can view its help page using:
```
gobuster --help
```
**Sample output:**
```
Usage:
  gobuster [command]

Available Commands:
  completion  Generate autocompletion script
  dir         Directory/file enumeration
  dns         DNS subdomain enumeration
  fuzz        Fuzzing mode (replaces FUZZ in URL/headers/body)
  gcs         Google Cloud Storage bucket enumeration
  help        Help for commands
  s3          AWS S3 bucket enumeration
  tftp        TFTP enumeration
  version     Show current version
  vhost       Virtual Host enumeration (use IP as URL)

Flags:
  --debug           Enable debug output
  --delay duration  Wait time between requests (e.g., 1500ms)
  -h, --help        Show help
  --no-color        Disable colored output
  --no-error        Don't display errors
  -z, --no-progress Don't display progress
  -o, --output      Output file for results
  -p, --pattern     File containing replacement patterns
  -q, --quiet       Suppress banner and extra messages
  -t, --threads     Number of concurrent threads (default 10)
  -v, --verbose     Verbose output (errors)
  -w, --wordlist    Path to wordlist (- for STDIN)
  --wordlist-offset Resume from a specific position

Use "gobuster [command] --help" for more info on a command.

```

## 4. Common Flags Explained

Flag | Description | Example
-----|------------|--------
-t, --threads | Number of concurrent threads. More threads = faster scan | -t 64
-w, --wordlist | Wordlist used for brute force | -w /usr/share/wordlists/dirb/small.txt
--delay | Delay between requests to avoid detection | --delay 500ms
--debug | Shows detailed debug output | --debug
-o, --output | Save results to a file | -o results.txt



## 5. Example: Directory Enumeration

Command:
```
gobuster dir -u "http://www.example.thm/" -w /usr/share/wordlists/dirb/small.txt -t 64
```
Explanation:

- `dir` → directory/file enumeration mode  
- `-u "http://www.example.thm/"` → target URL  
- `-w /usr/share/wordlists/dirb/small.txt` → wordlist used for brute force  
- `-t 64` → use 64 threads for faster scanning  

Example workflow:

1. First word in wordlist: `images`  
2. Gobuster sends GET request to: `http://www.example.thm/images/`  
3. Checks response and reports if directory exists  



## 6. Summary

- Gobuster is a **Golang tool** for offensive security  
- Performs **directory, subdomain, and vhost enumeration**  
- Works using **wordlists** and **brute force**  
- Helps in **reconnaissance and scanning** phases  
- Flags like `-t`, `-w`, `--delay`, `--output` customize scans
---
# Gobuster Dir Mode 

## 1. What is Dir Mode?

Gobuster has a `dir` mode to **enumerate website directories and files**.  
This is useful during penetration testing to see the **directory structure** and **files** on a web server.  

Example: WordPress directory structure
```
root@tryhackme:~# tree -L 3 -d
.
└── html
    └── wordpress
        ├── wp-admin
        ├── wp-content
        └── wp-includes

Gobuster is powerful because it **returns status codes**, which tell you whether a directory or file can be accessed or not.
```


## 2. Dir Mode Help

To see all options for `dir` mode, use:
```
gobuster dir --help
```
Key flags we often use:

Flag | Long Flag | Description
-----|-----------|------------
-c | --cookies | Pass a cookie with each request (e.g., session ID)
-x | --extensions | Scan for specific file types (.php, .js, etc.)
-H | --headers | Pass custom headers with each request
-k | --no-tls-validation | Skip HTTPS certificate check
-n | --no-status | Hide status codes to keep output clean
-P | --password | Used with --username for authenticated requests
-s | --status-codes | Show only specific HTTP status codes
-b | --status-codes-blacklist | Hide specific HTTP status codes (overrides -s)
-U | --username | Used with --password for authenticated requests
-r | --followredirect | Follow HTTP redirects (301, 302)



## 3. How to Use Dir Mode

Basic command format:
```
gobuster dir -u "http://www.example.thm" -w /path/to/wordlist
```
- `dir` → directory/file mode  
- `-u` → target URL (protocol required: http:// or https://)  
- `-w` → path to wordlist  



### Example 1: Basic Directory Scan
```
gobuster dir -u "http://www.example.thm" -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -r
```
Explanation:

- `gobuster dir` → directory enumeration  
- `-u http://www.example.thm` → scan from the root web directory  
- `-w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt` → wordlist used for brute force  
- `-r` → follow HTTP redirects (301/302)

Notes:

- The URL can be **IP or hostname**. Use hostname to target the correct site if multiple sites share an IP.  
- Gobuster **does not scan recursively**. You need to run a new scan on discovered directories if needed.  



### Example 2: Directory Scan with File Extensions
```
gobuster dir -u "http://www.example.thm" -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x .php,.js
```
Explanation:

- Same as Example 1, but also looks for files ending with `.php` or `.js`  
- Helps find scripts or web files that may be sensitive  



## 4. Summary

- `dir` mode is used to **enumerate directories and files**  
- Returns **status codes** to indicate accessible paths  
- Use **wordlists** to brute-force possible directories  
- Flags like `-r`, `-x`, `-c`, and `-H` **customize the scan**  
- Scan recursively by running Gobuster again on discovered directories
---
# Gobuster DNS Mode

## 1. What is DNS Mode?

Gobuster DNS mode is used to **brute force subdomains** of a target domain.  
During penetration testing, checking subdomains is important because a **vulnerability may exist in a subdomain** that does not exist in the main domain.

Example:

- Main domain: tryhackme.thm  
- Subdomain: mobile.tryhackme.thm  

Even if the main domain is patched, the subdomain might have a vulnerability.  



## 2. DNS Mode Help

To see all options for DNS mode:
```
gobuster dns --help
```
Common flags:

Flag | Long Flag | Description
-----|-----------|------------
-c | --show-cname | Show CNAME records (cannot be used with -i)
-i | --show-ips | Show IP addresses for the domain and subdomains
-r | --resolver | Use a custom DNS server for resolving
-d | --domain | Target domain to enumerate



## 3. How to Use DNS Mode

Basic command format:
```
gobuster dns -d example.thm -w /path/to/wordlist
```
- `dns` → subdomain enumeration mode  
- `-d` → target domain  
- `-w` → wordlist path  



### Example 1: Subdomain Enumeration
```
gobuster dns -d example.thm -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt
```
Explanation:

- `gobuster dns` → DNS mode for subdomain scanning  
- `-d example.thm` → target domain  
- `-w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt` → wordlist used for brute force  

How it works:

1. First word in wordlist: `www`  
2. Gobuster queries: `www.example.thm`  
3. Next word: `shop` → `shop.example.thm`  
4. Continues for each entry in the wordlist  
5. Returns all valid subdomains it finds  



### Example Output
```
root@tryhackme:~# gobuster dns -d example.thm -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Domain:     example.thm
[+] Threads:    10
[+] Timeout:    1s
[+] Wordlist:   /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt
===============================================================
Starting gobuster in DNS enumeration mode
===============================================================
Found: www.example.thm
Found: shop.example.thm
Found: academy.example.thm
Found: primary.example.thm
Progress: 4989 / 4990 (99.98%)
===============================================================
Finished
===============================================================

```

## 4. Summary

- DNS mode is used to **enumerate subdomains**  
- Uses **wordlists** for brute force  
- Common flags: `-d` (domain), `-w` (wordlist), `-i` (show IPs), `-c` (show CNAMEs), `-r` (custom resolver)  
- Important during pentesting because **subdomains may contain unpatched vulnerabilities**  

---
# Gobuster VHOST Mode 

## 1. What is VHOST Mode?

Gobuster VHOST mode is used to **brute force virtual hosts** on a web server.  
Virtual hosts are **different websites hosted on the same server**.  

### Difference between VHOST and DNS mode:

- **VHOST mode:** Combines `-u` (hostname) with wordlist entries and navigates to URLs  
- **DNS mode:** Combines `-d` (domain) with wordlist entries and performs DNS lookups  



## 2. VHOST Mode Help

To see all options for VHOST mode:
```
gobuster vhost --help
```
Common flags:

Flag | Long Flag | Description
-----|-----------|------------
-u | --url | Base URL / target domain for brute-forcing virtual hosts
--append-domain | Appends base domain to each wordlist entry (e.g., blog.example.com)
-m | --method | HTTP method for requests (GET, POST, etc.)
--domain | Appends a domain to each wordlist entry to form valid hostnames
--exclude-length | Exclude results based on response body length (filters false positives)
-r | --follow-redirect | Follow HTTP redirects  



## 3. How to Use VHOST Mode

Basic command format:
```
gobuster vhost -u "http://example.thm" -w /path/to/wordlist
```
- `vhost` → virtual host enumeration mode  
- `-u` → base URL or IP to connect to  
- `-w` → wordlist path  



### Example 1: VHOST Enumeration
```
gobuster vhost -u "http://10.201.0.79" --domain example.thm -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt --append-domain --exclude-length 250-320
```
Explanation:

- `gobuster vhost` → VHOST mode  
- `-u "http://10.201.0.79"` → target server IP  
- `--domain example.thm` → top-level and second-level domain  
- `-w ...subdomains-top1million-5000.txt` → wordlist  
- `--append-domain` → append the domain to wordlist entries (e.g., blog.example.thm)  
- `--exclude-length 250-320` → filter out false positives by response size  



### How Gobuster Sends Requests

Example GET request:
```
GET / HTTP/1.1  
Host: www.example.thm  
User-Agent: gobuster/3.6  
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8  
Accept-Language: en-US,en;q=0.5  
Accept-Encoding: gzip, deflate  
Connection: keep-alive  
```
Breakdown of Host header:

- `www` → subdomain from wordlist  
- `.example` → second-level domain (set via `--domain`)  
- `.thm` → top-level domain (set via `--domain`)  



### Example Output

root@tryhackme:~# gobuster vhost -u "http://10.201.0.79" --domain example.thm -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt --append-domain --exclude-length 250-320
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:              http://10.10.94.214
[+] Method:           GET
[+] Threads:          10
[+] Wordlist:         /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt
[+] User Agent:       gobuster/3.6
[+] Timeout:          10s
[+] Append Domain:    true
[+] Exclude Length:   250-320
===============================================================
Starting gobuster in VHOST enumeration mode
===============================================================
Found: blog.example.thm Status: 200 [Size: 1493]
Found: shop.example.thm Status: 200 [Size: 2983]
Found: www.example.thm Status: 200 [Size: 84352]
Found: chelyabinsk-rnoc-rr02.backbone.example.thm Status: 404 [Size: 304]
Found: academy.example.thm Status: 200 [Size: 434]
Progress: 4989 / 4990 (99.98%)
===============================================================
Finished
===============================================================



## 4. Summary

- VHOST mode enumerates **virtual hosts** on the same server  
- Uses **wordlists** and appends each entry to the base domain  
- Important flags: `-u`, `-w`, `--domain`, `--append-domain`, `--exclude-length`, `-r`  
- Helps find **hidden websites hosted on the same IP**  
- `--exclude-length` filters false positives based on response size  
- Requests change the `Host:` header for each wordlist entry  

---
# Offensive Tools Directory task

## Question 2: Enumerate directories of www.offensivetools.thm

**Objective:** Find directories on www.offensivetools.thm and identify interesting ones.

**Steps:**

1. **Check VPN & hosts file**
   - Ensure TryHackMe VPN is connected.
   - Add target to `/etc/hosts`:
     10.10.xxx.xxx www.offensivetools.thm
   - Test connectivity:
     ping www.offensivetools.thm

2. **Run Gobuster for directory enumeration**
```
gobuster dir -u http://www.offensivetools.thm -w /usr/share/wordlists/dirb/common.txt -t 50 -x php,html
```
- `-t 50` → use 50 threads for faster scanning  
- `-x php,html` → scan for .php and .html file extensions  

3. **Analyze output**
   - **403 responses** → hidden files like `.htaccess`, `.htpasswd` (may contain sensitive info)  
   - **301 redirects** → directories like `/administrator/`, `/api/`, `/cache/`, `/components/` (worth checking)  
   - **200 responses** → files like `/configuration.php` (important, may contain secrets)

4. **Conclusion**
   - Most interesting directories/files:
     - `/administrator/` → admin login page  
     - `/configuration.php` → sensitive configuration file  
     - `/api/` → API endpoints  
     - Optional: `/cache/` & `/components/` → further reconnaissance  

---

## Question 3: Continue enumerating the interesting directory

**Objective:** Find the .js file inside the interesting directory and retrieve the flag.

**Steps:**

1. **Identify the interesting directory**
   - From Question 2, the interesting directory is `/secret/`.

2. **Run Gobuster inside `/secret/`**
```
gobuster dir -u http://www.offensivetools.thm/secret/ -w /usr/share/wordlists/dirb/common.txt -t 50 -x js,html,php
```
- `-x js` → scan for .js files, as specified in the question  

3. **Locate the .js file**
   - Example output:
```
/secret/flag.js (Status: 200)
```
4. **Retrieve the flag**
   - Open in browser or use curl:
```
curl http://www.offensivetools.thm/secret/flag.js
```
- File content will show the flag, e.g.:

// THM{this_is_the_flag}

**Conclusion:**

- Interesting directory: `/secret/`  
- Flag found in: `.js` file (e.g., `flag.js`)  
- Submit the hardcoded flag string from the file content  



💡 **Tips for similar CTF challenges**

- Always check **403, 301, and 200 responses** carefully  
- `.js` files often contain hidden flags or secrets  
- Use Gobuster with appropriate `-x` extensions to catch all possible files  
- Always ensure **VPN connection** & **hosts file mapping** is correct for `.thm` domains  
