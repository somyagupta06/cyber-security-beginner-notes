# YARA 

## 1. Introduction — what this room expects
- You should know **basic Linux**: how to install software and how to move around folders and files.
- This room is for **learning and trying things**, not for marks.
- The main goal: find out what **YARA** is and why it helps in cybersecurity.

---

## 2. What is YARA?
- YARA is a tool used by security people to **find patterns** inside files.
- People call it: "the pattern matching swiss knife for malware researchers."
- It looks for **text or binary patterns** (like words, URLs, hex bytes) inside files to decide if a file may be bad.

---

## 3. Why strings matter
- A **string** is just a piece of text inside a program (like "Hello World").
- Programs (and malware) store text as strings.
- If a program contains a suspicious string (like a bitcoin wallet or an IP), YARA can find it.

**Example:**  
Python prints a string like this:
print("Hello World!")

YARA could search many files and say: "I found 'Hello World' inside this program."

---

## 4. Why malware uses strings (simple table)

| Malware Type | Example string found | What it means (easy) |
|--------------|----------------------|----------------------|
| Ransomware   | 12t9YDPgwueZ9NyMgw519p7AA8isjr6SMw | A Bitcoin wallet address where attacker wants payment |
| Botnet       | 12.34.56.7           | IP address of the attacker's control server (C&C) |
| Info-stealer | user@example.com     | Maybe an email or credential stored in the file |
| Downloader   | http://bad.example/file | A URL from where malware downloads more files |

---

## 5. What are YARA rules?
- A **rule** tells YARA what patterns to look for.
- A rule has:
  - A name
  - Strings to search
  - Conditions (when to say “match”)

### Small YARA rule example (very simple)
rule Hello_World_Rule {
  strings:
    $a = "Hello World"
  condition:
    $a
}

**Explain line by line (easy):**
- `rule Hello_World_Rule` → this is the rule's name.
- `strings:` → here we list text or byte patterns to search.
- `$a = "Hello World"` → we call this pattern `$a` and it looks for the text Hello World.
- `condition:` → when does the rule say YES?
- `$a` → if `$a` is found anywhere in the file → rule matches.

---

## 6. How YARA helps in real life (simple)
- If many files contain a known malware string, YARA can find all of them fast.
- Security teams use YARA to **label files** (malicious / suspicious / safe).
- Researchers share YARA rules so others can detect the same malware.

---

## 7. Small practice ideas (try these while learning)
- Read a sample file and try to spot readable words (strings).
- Write one tiny YARA rule like the example and test it on a simple text file.
- Try to find a string like an IP or URL in a test file and write a rule for it.

---

## 8. Quick summary
- YARA finds patterns (strings and bytes) inside files.
- Rules say what to look for and when to call a file suspicious.
- Many malware types include readable strings (wallets, IPs, URLs) — YARA can spot them.
- Practice by writing small rules and testing on simple files.

---

# Your First YARA Rule – Very Simple Notes

## 1. What is a YARA Rule?
- YARA uses its own small language to find patterns in files.
- A rule = instructions telling YARA **what to look for**.
- Every YARA command needs two things:
  1. The rule file you wrote (example: myrule.yar)
  2. The file / folder / process you want to scan.

**Example Command:**
yara myrule.yar somedirectory

`.yar` is the standard file extension for YARA rules.

---

## 2. Making Your First Rule

### Step 1: Make a test file
touch somefile  

### Step 2: Make a rule file
touch myfirstrule.yar  

### Step 3: Edit the rule file
nano myfirstrule.yar  

Put this inside:
rule examplerule {
    condition: true
}

- `examplerule` = name of the rule.
- `condition: true` = always true (just a basic test rule).

### Step 4: Run YARA
yara myfirstrule.yar somefile  

If `somefile` exists, you’ll see:
examplerule somefile  

If file does not exist:
error scanning sometextfile: could not open file  

Congrats 🎉 you made your first YARA rule.

---

## 3. YARA Rule Keywords (basic parts)

| Keyword  | What it does (easy) |
|----------|---------------------|
| meta     | Author notes, description, comments about rule (does not affect scanning). |
| strings  | The actual patterns you want to look for (text or hex). |
| condition| The rule to decide when to “match”. |
| weight   | Used to score/weight conditions (advanced). |

---

## 4. Strings Section – Searching Text or Hex
- `strings:` = where you list your search patterns.
- Each pattern gets a variable name starting with `$`.

**Example:**  
Look for “Hello World!” in any file:

rule helloworld_checker {
  strings:
    $hello_world = "Hello World!"
  condition:
    $hello_world
}

This matches only if the file contains **exactly** “Hello World!”.

---

## 5. Searching Multiple Variants
To match Hello World in different cases:

rule helloworld_checker {
  strings:
    $hello_world = "Hello World!"
    $hello_world_lower = "hello world"
    $hello_world_upper = "HELLO WORLD"
  condition:
    any of them
}

Now it matches if file has any of these:
- Hello World!
- hello world
- HELLO WORLD

---

## 6. Using Conditions

### Common operators:
- `<=` less than or equal
- `>=` more than or equal
- `!=` not equal

**Example:**
rule helloworld_checker {
  strings:
    $hello_world = "Hello World!"
  condition:
    #hello_world <= 10
}

This will:
1. Look for “Hello World!”
2. Only match if it appears 10 times or less in the file.

---

## 7. Combine Conditions (and, or, not)

**Example:**
Check file has “Hello World!” **and** is smaller than 10KB:

rule helloworld_checker {
  strings:
    $hello_world = "Hello World!"
  condition:
    $hello_world and filesize < 10KB
}

- If file has Hello World! **and** is <10KB → match.
- If either condition fails → no match.

---

## 8. Quick Anatomy of a Rule (summary)

rule <name> {
meta:
desc = "What this rule does"
strings:
$var1 = "string or hex"
condition:
<logic here>
}

- `rule <name>` → rule name.
- `meta:` → info only (like comments).
- `strings:` → patterns to search.
- `condition:` → logic when to match.
<img width="543" height="754" alt="Screenshot 2025-10-05 at 9 24 25 PM" src="https://github.com/user-attachments/assets/7e6b7a15-88d1-4efa-b363-14f041c945af" />

---

## 9. Practice Ideas
- Write a rule to find your name inside files.
- Write a rule to find an IP or URL.
- Try `any of them` and combine with file size.

---

## 10. Cheatsheet
A researcher called **fr0gger_** made a nice YARA cheatsheet showing all parts of a rule. It’s a good visual guide for beginners.

---

## 11. Quick Recap
- YARA rule = name + condition (always).
- `meta` = description, `strings` = what to look for, `condition` = when to match.
- You can combine conditions using and/or/not.
- Test your rule using:  
  yara myrule.yar file_or_folder

---

# YARA & Related Tools - Simple Notes 🎒

## 1) Overview
- **YARA**: A tool to detect suspicious files using patterns (like fingerprints).
- You can make your own rules or use tools to find bad stuff automatically.
- Tools like **Cuckoo Sandbox** and **Python PE** help make smarter rules.

---

## 2) Cuckoo Sandbox
- Automated malware analysis environment.
- You can run malware safely and see what it does.
- Then create YARA rules based on **behaviors** (like strings it creates at runtime, files it touches).
- Example: If malware creates `secret.txt` → Cuckoo tells you → you can make YARA rule for it.

---

## 3) Python PE Module
- PE = Portable Executable (Windows program file format, e.g., `.exe` or `.dll`).
- Python PE lets you check the sections of a PE file (like `.text`, `.data`, `.rsrc`) without running the malware.
- You can create YARA rules from these sections.
- Why useful? Can detect malware doing crypto, worming, etc. without executing it.

---

## 4) YARA Tools (ready-made help)
You don’t have to write all rules yourself. Some open-source tools help you find IOCs.

### 4.1 LOKI
- Free IOC scanner by Florian Roth.
- Detection methods:
  1. File Name IOC check
  2. **YARA Rule check**
  3. Hash check
  4. C2 Back Connect check
- Works on Windows & Linux.

#### Display LOKI help
python3 loki.py -h

---

### 4.2 THOR Lite
- New multi-platform IOC + YARA scanner by Florian Roth.
- Has scan throttling (so CPU doesn’t get too hot).
- Free version: THOR Lite (paid full version for companies).

#### Display THOR Lite help
./thor-lite-linux-64 -h

---

### 4.3 FENRIR
- Bash script IOC scanner by Neo23x0.
- Can run on any system with bash (Linux, macOS, Windows nowadays).
- Detects malware indicators without complicated setup.

#### Run Fenrir
./fenrir.sh

---

### 4.4 YAYA (Yet Another YARA Automaton)
- Tool from EFF (Electronic Frontier Foundation).
- Lets you manage **multiple YARA rule sets**.
- You can:
  - Add your own rules
  - Remove or disable rules
  - Run scans on files

#### Run YAYA
yaya

Commands inside YAYA:
- `update` → update rulesets
- `edit` → ban/remove rulesets
- `add` → add custom ruleset
- `scan` → run YARA scan on folder

---

## 5) Summary
- **Cuckoo** → learn behavior → make rules.
- **Python PE** → look inside files → make rules.
- **LOKI, THOR, FENRIR, YAYA** → ready-made YARA/IOC tools for hunting malware.
- You don’t need to run all these now, but good to know.
---
# Using LOKI, yarGen & Valhalla — Simple Notes 🎒

> Level: 9th-grade friendly, desi style.  
> Commands are shown plainly so you can copy-paste into GitHub or terminal.

---

## Quick idea — what we are doing
- You found suspicious files that your tools didn't fully detect.
- **Goal:** Make YARA rules to detect those files across your network.
- Tools used:
  - **LOKI** → scan endpoints with built-in YARA & IOCs.
  - **yarGen** → automatically generate YARA rules for a suspicious file.
  - **Valhalla** → online feed of high-quality YARA rules for threat research.

---

## 1) Finding Loki and running help
- Loki is inside `tools` folder.
- See tools:
ls
- Go to Loki folder:
cd tools/Loki
- Show Loki options/help:
python loki.py -h

Notes:
- First time on your own system, run `--update` so Loki downloads `signature-base`.
- In the VM used in the room, `--update` was already run.

---

## 2) Loki signature-base structure
- After update, Loki stores signatures here:
tools/Loki/signature-base
- Inside you will see:
iocs
misc
yara
- Inspect YARA rules to understand what Loki already hunts for:
ls tools/Loki/signature-base/yara

---

## 3) Scan a suspicious file with Loki
- From the directory containing the suspicious file, run:
python ../../tools/Loki/loki.py -p .
- `-p .` means “scan the current directory”.

If Loki flags the file → good.  
If Loki does **not** flag the file → we may need a custom YARA rule.

---

## 4) When Loki misses something — create a YARA rule with yarGen
- Problem: file2 (web shell) exists but Loki didn’t catch it. Manually hunting strings is long (thousands of lines).
- Quick count of printable strings in file:
strings 1ndex.php | wc -l
- If large (example 3580), manual review is boring and error-prone.

### yarGen — what it does (simple)
- yarGen extracts strings from your suspicious file and removes strings that are common in good software (to reduce false positives).
- It needs a "goodware" database (good strings & opcodes). First run must update these DBs.

### Update yarGen (if you have internet)
python3 yarGen.py --update
- This downloads `good-opcodes` and `good-strings` DBs.

### Generate YARA rule for file2
python3 yarGen.py -m /home/cmnatic/suspicious-files/file2 --excludegood -o /home/cmnatic/suspicious-files/file2.yar
- `-m` = path to the file(s) to make rules for  
- `--excludegood` = remove strings seen in legit software (lowers false positives)  
- `-o` = output path & filename for the YARA rule

Expected output:
- It will tell you how many rules were generated and where they were written  
Example message:  
[=] Generated 1 SIMPLE rules.
[=] All rules written to /home/cmnatic/suspicious-files/file2.yar
[+] yarGen run finished

---

## 5) Why use yarGen? (short)
- Saves time: finds strings specific to the suspicious file.
- Reduces false positives with `--excludegood`.
- Produces YARA rule file ready to tune.

---

## 6) Valhalla — threat research & public YARA rules
- Valhalla is an online YARA feed with thousands of rules created by experts.
- Use Valhalla to:
  - Search rules by keyword, tag, ATT&CK technique, sha256, or rule name.
  - Read rule descriptions, references, and submission date.
- This helps you justify removal actions (gives context & sources).

---

## 7) Investigative workflow (step-by-step, simple)
1. Find suspicious files on an endpoint.  
2. Run Loki to scan the endpoint:
python ../../tools/Loki/loki.py -p .
3. If Loki flags → document IOC and start remediation.  
4. If Loki misses → extract strings, then use yarGen to make YARA rule:
python3 yarGen.py -m /path/to/suspicious --excludegood -o /path/to/output.yar
5. Inspect generated YARA rule (tune if needed).  
6. Test rule locally against known clean files to verify false positives.  
7. Add rule to your detection tool (LOKI, SIEM, etc.) and scan other servers.  
8. Use Valhalla to research similar rules and get references to support your findings.

---

## 8) Example: detect a web shell across web servers
- After yarGen gives you `file2.yar`, run Loki to use custom rule or add to signature base.  
- Or run yara directly:
yara /path/to/file2.yar /var/www/html -r
- `-r` = recursive scan through directories.

---

## 9) Tips & best practices
- Always run `--excludegood` in yarGen to reduce false positives.  
- Inspect the generated rule and remove very-generic strings. Keep unique identifiers.  
- Test rules on a set of clean files (goodware) before wide deployment.  
- Version control your custom rules (Git) and document why you made each rule (evidence + references).  
- Use Valhalla and published IOCs to strengthen your detection rationale.

---

## 10) Short summary (one-liner each)
- **LOKI** → quick endpoint IOC & YARA scanner.  
- **yarGen** → auto-generate focused YARA rules from suspicious files.  
- **Valhalla** → large community YARA feed for research and references.

---

## 11) Handy commands recap
ls
cd tools/Loki
python loki.py -h
python ../../tools/Loki/loki.py -p .
strings 1ndex.php | wc -l
cd tools/yarGen
python3 yarGen.py --update
python3 yarGen.py -m /home/cmnatic/suspicious-files/file2 --excludegood -o /home/cmnatic/suspicious-files/file2.yar
yara /path/to/file2.yar /var/www/html -r

---
