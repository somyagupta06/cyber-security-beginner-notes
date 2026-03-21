# 🔍 Kibana Query Basics

## 1. Why Query Language Matters
In SOC, we search logs to find attacks.
How we write queries = what results we get.

If query is wrong → we miss attacker activity ❌

---

## 2. Types of Query Languages in Kibana

### 1. KQL (Kibana Query Language)
- Easy to use
- Best for filtering logs quickly
- Used mostly in SOC dashboards

Example:
flag: thm

---

### 2. Lucene Query
- More powerful but strict
- Needs special characters handling
- Used for advanced searching

Example:
flag: thm

---

### 3. ES|QL (Elasticsearch Query Language)
- New language
- Works like pipeline (step by step)
- Similar to Splunk SPL

Example:
FROM index-name | WHERE flag == "thm"

---

## 🧠 SOC Tip:
Use KQL for daily SOC work
Use Lucene when you need advanced matching

---

## 3. Special Characters (Very Important ⚠️)

Some characters have special meaning:
+ - && || ! ( ) { } [ ] ^ ~ * ? : /

### Problem:
If you search them normally → error or wrong result

### Solution:

#### Use Quotes:
flag: "THM{kql!}"

#### OR Escape with backslash:
flag: THM\{kql\!\}

---

### Quotes Usage
- For exact phrases
- For values with spaces

Example:
log_message: "Authentication failed"

---

### Escape Quotes inside text:
log_message: "User \"bob\" created"

---

### Dot (.)
Used in field names like:
host.name

Searching value:
KQL: file.extension: ".exe"  
Lucene: file.extension: \.exe  

---

## 4. Wildcards (*)

Used when you don't know exact value

* = matches anything

Example:
article_title: *Malware*

✔ Matches:
- Malware detected
- New Malware attack

---

### ⚠️ SOC Warning:
- Slow if starts with *
- Not good for large datasets

---

## 5. Field Types (Very Important Concept)

### 1. Text Field
- Broken into words (tokens)

Example:
"Elastic Query Languages"
→ Elastic, Query, Languages

👉 Problem:
Wildcard may fail across full sentence

---

### 2. Keyword Field
- Stored exactly same

Example:
"Elastic Query Languages"

👉 Works for exact match

---

## 🧠 SOC Tip:
Use keyword fields for:
- usernames
- file names
- IPs

---

## 6. Nested Data (Important for Logs)

Some fields contain objects inside them (JSON-like)

Example:
comments = [
  {author: Alice, text: attack},
  {author: Bob, text: logs}
]

---

### Search All Authors:
comments.author: *

✔ Returns all records

---

### Search Specific Author:
comments.author: "Alice"

✔ Returns only Alice records

---

## 7. Combine Conditions (AND / OR)

### AND → both conditions must match

Example:
comments.author: "Alice" AND comments.text: "attack"

✔ SOC Meaning:
Find logs where Alice wrote AND attack is mentioned

---

### OR → any condition can match

Example:
comments.author: ("Alice" OR "Bob")

✔ SOC Meaning:
Logs from Alice OR Bob

---

### Grouping with ()
comments.author: ("Alice" AND "Bob")

👉 Same field multiple values

---

## 🧠 Real SOC Use Case

Find attack logs written by analyst:

comments.author: "Alice" AND comments.text: "attack"

---

## 8. Key Mistakes to Avoid ❌

- Not using quotes for special characters
- Using wildcard at start (*malware)
- Confusing text vs keyword fields
- Forgetting AND / OR logic

---

## 🎯 Final Summary

- KQL = easy (use daily)
- Lucene = strict but powerful
- ES|QL = advanced (pipeline)
- Use quotes for special characters
- Wildcards are useful but slow
- Understand field types
- Use AND / OR properly

---

## 🚀 SOC Mindset

Always think:
"What exactly am I trying to find?"

Because query = investigation result

---
## Lab Questions

### Q.1 How many incidents involve the file marketing_strategy_2023_07_23.pptx?
<img width="1470" height="956" alt="Screenshot 2026-03-21 at 11 04 44 AM" src="https://github.com/user-attachments/assets/1376005d-28f0-4d1e-a6eb-4c864877ee83" />

### Q.2 Try out a wildcard search to locate files using the affected_systems.affected_files.file_name field. How many incidents involve files on a File Server with names that start with marketing_strategy?
<img width="1470" height="956" alt="Screenshot 2026-03-21 at 11 03 10 AM" src="https://github.com/user-attachments/assets/6f0129a0-62ca-476c-ab1b-8949820734e7" />


### Q.3 Which Web Server had a true positive incident where both the admin and it users were logged in?
<img width="1470" height="956" alt="Screenshot 2026-03-21 at 11 01 58 AM" src="https://github.com/user-attachments/assets/418be8d0-c8e7-4c82-bde6-04281ec56094" />

---
# 🔍 Regex & Range Queries in Kibana 

---

## 1. What is Regex? 

Regex = pattern matching

Instead of exact words → we search using patterns

👉 SOC Use:
- Find suspicious commands
- Detect malware patterns
- Match unknown variations

---

## 2. Regex Syntax in Kibana (Lucene)

⚠️ Important:
Regex must be inside / /

Example:
Event_Type: /.*/

---

## 3. Basic Regex Symbols

.  → any single character  
*  → zero or more characters  

---

### Example 1: Match Everything
Event_Type: /.*/

✔ Returns all logs

---

### Example 2: Start with S or M
Event_Type: /(S|M).*/

✔ Matches:
- Malware
- Suspicious
- Scan

---

## 🧠 SOC Tip:
Use (A|B) when multiple possibilities exist

---

## 4. Keyword vs Text (VERY IMPORTANT ⚠️)

### Keyword Field
- Full value stored as-is
- Regex works on full string

Example:
Event_Type: /(S|M).*/

✔ Matches full field

---

### Text Field
- Broken into words (tokens)

Example field:
"Malware attack detected"

Stored as:
Malware, attack, detected

---

### Problem:
Regex works on EACH WORD

Example:
Description: /(s|m).*/

✔ Matches:
- malware
- suspicious

❌ Does NOT match full sentence

---

## 🧠 SOC Tip:
- Use keyword fields for exact detection
- Be careful with text fields (tokenized)

---

## 5. Combining Regex (Powerful 💥)

Example:
Description: /(s|m).*/ AND /user.*/

✔ SOC Meaning:
- words starting with s or m
- AND word containing "user"

---

## 6. Real SOC Use Cases

Find suspicious activity:
Description: /.*attack.*/

Find malware-related logs:
Event_Type: /Malware.*/

Find multiple patterns:
Description: /(phishing|malware|attack).*/

---

## 7. Range Queries (Super Important 📊)

Used for:
- numbers
- time
- IP ranges

---

### Example Dataset:
response_time_seconds

---

### Greater than or equal:
response_time_seconds >= 100

✔ returns all >=100

---

### Less than:
response_time_seconds < 300

✔ returns values below 300

---

### Combine Range:
response_time_seconds >= 100 AND response_time_seconds <= 300

✔ SOC Meaning:
Find alerts handled within 100–300 sec

---

## 8. Time-Based Queries (VERY USEFUL ⏱️)

Find logs after a date:
@timestamp >= "2023-01-01"

---

### Between two dates:
@timestamp >= "2023-01-01" AND @timestamp < "2023-03-01"

✔ SOC Meaning:
Find incidents in a time window

---

## 🧠 SOC Tip:
Time filtering = most important in investigations

---

## 9. Common Mistakes ❌

- Forgetting / / in regex
- Using regex on text fields incorrectly
- Not understanding tokenization
- Wrong date format
- Mixing KQL and Lucene

---

## 🎯 Final Summary

- Regex = pattern search (very powerful)
- Use /pattern/ in Lucene
- Keyword → full match
- Text → word-by-word match
- Range queries → numbers + time
- Combine conditions using AND

---

## 🚀 SOC Mindset

Don't search exact words only

Think like attacker:
"What patterns can attacker use?"

Then write regex for that
---
## Lab Questions

### Q.1 How many events exist where a client_list file was affected by ransomware?

<img width="1470" height="956" alt="Screenshot 2026-03-21 at 11 24 32 AM" src="https://github.com/user-attachments/assets/75caae6e-e23b-4298-a131-1b1f38d62c17" />


### Q.2 Investigate the affected_systems.affected_files.file_name field using the regex /.*project.*/. What is the affected system name on the latest event managed by EVenis?

<img width="1470" height="956" alt="Screenshot 2026-03-21 at 11 30 59 AM" src="https://github.com/user-attachments/assets/e7676a84-dcd4-478a-ab38-534cd04cb72f" />

### Q.3 How many Data Leak incidents have a severity_level of 9 or higher?
<img width="1470" height="956" alt="Screenshot 2026-03-21 at 11 36 57 AM" src="https://github.com/user-attachments/assets/04c6eb5f-1d37-4689-b548-48b5968e8217" />


### Q.4 Check out the incidents investigated by AJohnston on or before December 1, 2022. How many events occurred on either an Email Server or a Web Server?

<img width="1470" height="956" alt="Screenshot 2026-03-21 at 11 44 42 AM" src="https://github.com/user-attachments/assets/dbae3d27-93d8-4d23-bc79-da730212331c" />

### Q.5 Investigate incident_id 1-500. Which analyst stated that the Data Leak on file-server-65 was a false positive?

<img width="1470" height="956" alt="Screenshot 2026-03-21 at 11 50 32 AM" src="https://github.com/user-attachments/assets/04062f39-1ef2-41f9-a19e-e9005a460de0" />

---
# 🔍 Regex & Range Queries in Kibana 

---

## 1. What is Regex? 

Regex = pattern matching

Instead of exact words → we search using patterns

👉 SOC Use:
- Find suspicious commands
- Detect malware patterns
- Match unknown variations

---

## 2. Regex Syntax in Kibana (Lucene)

⚠️ Important:
Regex must be inside / /

Example:
Event_Type: /.*/

---

## 3. Basic Regex Symbols

.  → any single character  
*  → zero or more characters  

---

### Example 1: Match Everything
Event_Type: /.*/

✔ Returns all logs

---

### Example 2: Start with S or M
Event_Type: /(S|M).*/

✔ Matches:
- Malware
- Suspicious
- Scan

---

## 🧠 SOC Tip:
Use (A|B) when multiple possibilities exist

---

## 4. Keyword vs Text (VERY IMPORTANT ⚠️)

### Keyword Field
- Full value stored as-is
- Regex works on full string

Example:
Event_Type: /(S|M).*/

✔ Matches full field

---

### Text Field
- Broken into words (tokens)

Example field:
"Malware attack detected"

Stored as:
Malware, attack, detected

---

### Problem:
Regex works on EACH WORD

Example:
Description: /(s|m).*/

✔ Matches:
- malware
- suspicious

❌ Does NOT match full sentence

---

## 🧠 SOC Tip:
- Use keyword fields for exact detection
- Be careful with text fields (tokenized)

---

## 5. Combining Regex (Powerful 💥)

Example:
Description: /(s|m).*/ AND /user.*/

✔ SOC Meaning:
- words starting with s or m
- AND word containing "user"

---

## 6. Real SOC Use Cases

Find suspicious activity:
Description: /.*attack.*/

Find malware-related logs:
Event_Type: /Malware.*/

Find multiple patterns:
Description: /(phishing|malware|attack).*/

---

## 7. Range Queries (Super Important 📊)

Used for:
- numbers
- time
- IP ranges

---

### Example Dataset:
response_time_seconds

---

### Greater than or equal:
response_time_seconds >= 100

✔ returns all >=100

---

### Less than:
response_time_seconds < 300

✔ returns values below 300

---

### Combine Range:
response_time_seconds >= 100 AND response_time_seconds <= 300

✔ SOC Meaning:
Find alerts handled within 100–300 sec

---

## 8. Time-Based Queries (VERY USEFUL ⏱️)

Find logs after a date:
@timestamp >= "2023-01-01"

---

### Between two dates:
@timestamp >= "2023-01-01" AND @timestamp < "2023-03-01"

✔ SOC Meaning:
Find incidents in a time window

---

## 🧠 SOC Tip:
Time filtering = most important in investigations

---

## 9. Common Mistakes ❌

- Forgetting / / in regex
- Using regex on text fields incorrectly
- Not understanding tokenization
- Wrong date format
- Mixing KQL and Lucene

---

## 🎯 Final Summary

- Regex = pattern search (very powerful)
- Use /pattern/ in Lucene
- Keyword → full match
- Text → word-by-word match
- Range queries → numbers + time
- Combine conditions using AND

---

## 🚀 SOC Mindset

Don't search exact words only

Think like attacker:
"What patterns can attacker use?"

Then write regex for that

---

## Lab Questions

### Q.1 Try out a fuzzy search to highlight variations in the incident_comments field. How many incidents has JLim handled in which the word true appears with a single character difference?
- change kql to lucene before query
<img width="1470" height="956" alt="Screenshot 2026-03-21 at 12 11 59 PM" src="https://github.com/user-attachments/assets/3fe889cd-7a5d-48c6-a28e-5be2fa8b600c" />

### Q.2 Try out a proximity search on the incident_comments field. How many events contain data-leak true-negative within three words of each other?

<img width="1470" height="956" alt="Screenshot 2026-03-21 at 12 11 33 PM" src="https://github.com/user-attachments/assets/f724c59f-c302-446a-b9cb-c2a0906c0284" />


### Q.3 How many incidents handled by AJohnston contain detected and negative within two positions?


<img width="1470" height="956" alt="Screenshot 2026-03-21 at 12 13 03 PM" src="https://github.com/user-attachments/assets/2af27fb6-77ef-4913-8522-83dc5dd95003" />



















