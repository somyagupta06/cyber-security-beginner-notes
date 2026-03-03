# Regular Expressions (Regex) 

## 1. What is Regex?

Regular Expression (Regex) is a **pattern** used to search text.

Think like this:

- If text is a big book  
- Regex is a smart search rule  

Instead of searching only one word, you can search:
- Words starting with "a"
- IP addresses
- Emails
- Suspicious file names
- Numbers of specific length

In SOC (Security Operations Center), Regex helps us:
- Detect malicious logs
- Find IP addresses
- Detect suspicious commands
- Search patterns in SIEM tools like Splunk

---

## 2. Basic Characters

### Normal Text Search

If you search:

admin

It will match:
- admin

---

## 3. Character Sets [ ]

Square brackets mean:  
"Match ANY ONE character inside this box"

### Example:

[abc]

Matches:
- a
- b
- c

[a-c]

Same as above (range from a to c)

[a-z]

Matches any lowercase letter

[a-zA-Z]

Matches any letter (uppercase + lowercase)

[0-9]

Matches any digit

---

## 4. Excluding Characters [^ ]

If ^ is inside brackets, it means "NOT".

Example:

[^a]

Matches anything except "a"

[^0-9]

Matches anything that is NOT a digit

SOC Example:
To detect lines that do NOT start with number:
^[^0-9]

---

## 5. Dot (.) – Wildcard

. means:
Match any ONE character (except line break)

Example:

a.c

Matches:
- abc
- a9c
- a@c

If you want real dot:
a\.c

Matches:
- a.c

---

## 6. Special Shortcuts (Very Important for SOC)

| Pattern | Meaning | Example Use |
|----------|----------|--------------|
| \d | any digit (0-9) | IP, ports |
| \D | not a digit | text detection |
| \w | letter, number, underscore | usernames |
| \W | not letter/number | special chars |
| \s | space, tab | log parsing |
| \S | not space | command detection |

Example:

\d\d\d\d

Matches 4 digit number.

Better way:

\d{4}

---

## 7. Repetition

Used when you want something many times.

| Pattern | Meaning |
|----------|----------|
| {3} | exactly 3 times |
| {1,5} | 1 to 5 times |
| {2,} | 2 or more times |
| * | 0 or more times |
| + | 1 or more times |

Example:

\d{4}

Matches:
- 1234
- 2025

SOC Example – Detect 5 failed login attempts word:

(failed ){5}

---

## 8. Start and End of Line

Very important in log analysis.

^  → start of line  
$  → end of line  

Example:

^ERROR

Matches lines starting with ERROR

\.exe$

Matches anything ending with .exe

SOC Example:
Detect suspicious executable:
.*\.exe$

---

## 9. Optional Character ?

? means:
Previous character is optional.

Example:

abc?

Matches:
- ab
- abc

---

## 10. Groups ( )

Used for:
- OR condition
- Repeating words

OR uses | symbol

Example:

(day|night)

Matches:
- day
- night

SOC Example:

(cmd|powershell)

Matches:
- cmd
- powershell

---

## 11. Real SOC Examples

### 1️⃣ Detect IP Address (simple version)

\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}

---

### 2️⃣ Detect Email

\w+@\w+\.\w+

---

### 3️⃣ Detect Suspicious PowerShell

powershell.*-enc

---

### 4️⃣ Detect File with .exe

.*\.exe$

---

## 12. Important Mindset for SOC

When writing regex:

1. Be specific  
   Do not use [a-z] if only [a-c] needed.

2. Do not overcomplicate  
   If many letters needed, use [a-z] instead of listing all.

3. Always test your regex.

---

## 13. Why Regex is Powerful in SOC

You will use it in:
- Splunk queries
- Log analysis
- Threat hunting
- Malware detection
- CTF challenges

Regex saves time.
Instead of reading 10,000 logs,
you write one smart pattern.

---

## Quick Revision Section

• [ ] = match one from box  
• [^ ] = NOT  
• . = any character  
• \d = digit  
• \w = letter/number  
• * = many times  
• + = at least one  
• ^ = start of line  
• $ = end of line  
• (a|b) = either a or b  

---

That’s it.

Regex is just pattern matching.

Simple rule:
Text is data.
Regex is smart filter.

Practice daily with small log samples.
In SOC career, this skill is very powerful.
