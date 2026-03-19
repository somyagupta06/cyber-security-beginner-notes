# ELK Stack (Logstash, Elasticsearch, Kibana) 

## 1. What is ELK Stack?

ELK = Elasticsearch + Logstash + Kibana

Think of it like this:

- Logstash → collects and cleans logs
- Elasticsearch → stores and searches logs
- Kibana → shows logs in dashboards (graphs, charts)

👉 In SOC:
You use ELK to monitor attacks, detect suspicious activity, and investigate incidents.

---

## 2. Logstash (Data Collector + Processor)

### What it does:
- Takes logs from different sources (system logs, network logs, etc.)
- Cleans and formats them
- Sends them to Elasticsearch

### Pipeline Structure:

input {
}

filter {
}

output {
}

### Simple Idea:
- input → where logs come from
- filter → clean/modify logs
- output → where logs go (Elasticsearch)

👉 SOC Example:
- Input: Windows logs
- Filter: Extract IP, username
- Output: Send to Elasticsearch

---

## 3. Elasticsearch (Storage + Search Engine)

### What it does:
- Stores all logs
- Lets you search them fast
- Works in real-time

👉 SOC Example:
You can search:
- "failed login"
- "suspicious IP"
- "new admin account"

### Important Folder:
- /etc/elasticsearch/

### Important File:
- elasticsearch.yml

### Key Settings:
- network.host → where it runs (localhost = same machine)
- http.port → default is 9200

👉 Why important in SOC?
- This is your **log database**
- All investigation happens here

---

## 4. Logstash Configuration (Important for SOC)

### Folder:
- /etc/logstash/

### Important Folder:
- conf.d/ → where pipeline configs are stored

### Important File:
- logstash.yml

### Key Settings:
config.reload.automatic: true  
config.reload.interval: 3s  

### Meaning:
- Logstash checks every 3 seconds for changes
- Useful during SOC testing and tuning

👉 SOC Use:
- You can update parsing rules without restarting service

---

## 5. Kibana (Visualization Tool)

### What it does:
- Shows logs in dashboard form
- Helps you analyze visually

👉 SOC Example:
- Graph of failed logins
- Map of attacker IPs
- Timeline of attack

### Runs on:
- Port: 5601

### Important File:
- kibana.yml

### Key Settings:
server.port: 5601  
server.host: "0.0.0.0"  
elasticsearch.hosts: ["http://localhost:9200"]  

👉 Why important?
- Connects Kibana to Elasticsearch

---

## 6. How Everything Works Together (Very Important)

Flow:

Logs → Logstash → Elasticsearch → Kibana

### Step-by-step:
1. Logs come from systems
2. Logstash processes them
3. Elasticsearch stores them
4. Kibana shows them

👉 SOC Analyst Workflow:
- Monitor dashboards in Kibana
- Search logs in Elasticsearch
- Tune parsing in Logstash

---

## 7. Services (Start + Enable Concept)

Each tool runs as a service:
- Elasticsearch
- Logstash
- Kibana

### Two important things:
- enable → start on boot
- start → run now

👉 SOC Tip:
Always check service status if logs are not coming

---

## 8. Important SOC Use Cases

### 1. Detect Brute Force
Search:
- multiple failed logins

### 2. Find Suspicious IP
- filter logs for unknown IPs

### 3. Detect Privilege Escalation
- new admin user created

### 4. Malware Activity
- unusual processes or connections

---

## 9. Quick Revision Table

| Tool          | Role                  | SOC Use                        |
|--------------|----------------------|--------------------------------|
| Logstash     | Collect + Process    | Parse logs                     |
| Elasticsearch| Store + Search       | Investigate logs               |
| Kibana       | Visualize            | Monitor dashboards             |

---

## 10. Easy Memory Trick

👉 "Collect → Store → Show"

- Logstash → Collect
- Elasticsearch → Store
- Kibana → Show

---

## Final Tip (Important for Exams + SOC)

- Logstash = pipeline
- Elasticsearch = database
- Kibana = dashboard

If logs are not visible:
1. Check Logstash config
2. Check Elasticsearch running
3. Check Kibana connection

---

# Logstash Deep Dive 

## 1. Big Picture 

Think of Logstash like a **machine in SOC**:

- It **collects logs**
- It **cleans and understands them**
- It **sends them to storage (Elasticsearch)**

👉 Structure always same:

input → filter → output

---

## 2. INPUT (Where logs come from)

### Simple Meaning:
Input = **source of logs**

👉 SOC Example:
- Windows logs
- Firewall logs
- Network logs
- Web server logs

---

### Common Input Plugins 

| Plugin     | What it does (Simple)                | SOC Example |
|-----------|-------------------------------------|------------|
| file      | Reads logs from files               | system.log |
| beats     | Gets logs from agents (Filebeat)    | endpoint logs |
| tcp       | Receives logs over TCP              | network devices |
| udp       | Receives logs over UDP              | syslog traffic |
| syslog    | Collects syslog logs                | routers/firewalls |
| http      | Receives logs via API/webhook       | alerts |
| snmp trap | Alerts from network devices         | switch alerts |
| s3        | Reads logs from cloud storage       | AWS logs |

---

### Example (File Input)

input {
  file {
    path => "/logs/auth.log"
    start_position => "beginning"
    sincedb_path => "/dev/null"
  }
}

👉 Simple meaning:
- Read file
- Start from beginning
- Don’t remember old position

👉 SOC Use:
- Read authentication logs to detect brute force

---

### Example (TCP Input)

input {
  tcp {
    port => 5055
    codec => json
  }
}

👉 Meaning:
- Listen on port 5055
- Expect JSON logs

👉 SOC Use:
- Receive logs from network tools

---

## 3. FILTER (Brain of Logstash)

### Simple Meaning:
Filter = **clean + extract + modify logs**

👉 This is MOST IMPORTANT in SOC

---

### Why filters matter?

Raw log:
"Failed login from 192.168.1.5 user admin"

After filter:
- user = admin
- ip = 192.168.1.5
- event = failed login

👉 Now easy to detect attack

---

### Common Filters 

| Filter     | What it does                  | SOC Use |
|-----------|------------------------------|--------|
| grok      | Extract data from text       | get IP, username |
| mutate    | Modify fields               | rename, lowercase |
| date      | Fix timestamps              | timeline analysis |
| json      | Parse JSON logs             | API logs |
| geoip     | Add location from IP        | attacker country |
| useragent | Detect browser/device       | phishing detection |
| drop      | Remove unwanted logs        | noise reduction |
| translate | Convert codes to names      | status codes |
| dns       | Resolve IP to domain        | threat hunting |

---

### Example 1: Add Field

filter {
  mutate {
    add_field => { "alert" => "suspicious" }
  }
}

👉 Adds new field to every log

---

### Example 2: Lowercase

filter {
  mutate {
    lowercase => ["username"]
  }
}

👉 Useful because:
Admin = admin (same after normalization)

---

### Example 3: GROK (Very Important 🔥)

filter {
  grok {
    match => { "message" => "%{IP:source_ip}" }
  }
}

👉 Extracts IP from log

👉 SOC Use:
- Find attacker IP
- Track suspicious activity

---

### Example 4: Drop Logs

filter {
  if [status] == "success" {
    drop { }
  }
}

👉 Removes normal logs

👉 SOC Use:
- Keep only suspicious logs
- Reduce noise

---

### Example 5: KV Parsing

filter {
  kv {
    field_split => "&"
    value_split => "="
  }
}

👉 Converts:
user=admin&ip=1.1.1.1

➡️ into:
- user = admin
- ip = 1.1.1.1

---

## 4. OUTPUT (Where logs go)

### Simple Meaning:
Output = **destination**

---

### Common Output Plugins

| Output        | What it does              | SOC Use |
|--------------|--------------------------|--------|
| elasticsearch| Store logs               | main storage |
| file         | Save logs locally        | backup |
| stdout       | print logs               | debugging |
| kafka        | send to pipeline         | large systems |
| s3           | cloud storage            | archive |

---

### Example (Elasticsearch Output)

output {
  elasticsearch {
    hosts => ["localhost:9200"]
  }
}

👉 Sends logs to Elasticsearch

👉 SOC Use:
- Store logs for investigation

---

### Example (STDOUT)

output {
  stdout { }
}

👉 Prints logs on screen

👉 SOC Use:
- Debug pipeline

---

## 5. Full Pipeline Example (Very Important)

input {
  file {
    path => "/logs/auth.log"
  }
}

filter {
  grok {
    match => { "message" => "%{IP:ip}" }
  }
}

output {
  elasticsearch {
    hosts => ["localhost:9200"]
  }
}

---

### What happens here?

1. Read logs from file
2. Extract IP
3. Send to Elasticsearch

👉 SOC Meaning:
- Now you can search attacker IP in Kibana

---

## 6. SOC Thinking (Most Important Section)

When you see Logstash → always think:

### 1. What is the log source? (input)
- Windows?
- Network?
- Web?

### 2. What should I extract? (filter)
- IP
- username
- event type

### 3. Where to send? (output)
- Elasticsearch (mostly)

---

## 7. Quick Revision (1 Minute)

👉 Input = logs come in  
👉 Filter = clean + extract  
👉 Output = send logs  

👉 Most important filter = GROK

👉 Goal in SOC:
- Convert messy logs → useful data

---

## 8. Easy Memory Trick

👉 "RAW → CLEAN → STORE"

- RAW = input
- CLEAN = filter
- STORE = output

---

# Logstash Output 

## 1. What is OUTPUT? 

👉 Output = **Where logs go after processing**

Think like this:
- Input → logs come in
- Filter → logs cleaned
- Output → logs sent somewhere

👉 In SOC:
Output is where you **store or forward logs for investigation**

---

## 2. Most Important Output (Remember This 🔥)

### Elasticsearch (MOST USED)

output {
  elasticsearch {
    hosts => ["localhost:9200"]
    index => "my_index"
  }
}

👉 Meaning:
- Send logs to Elasticsearch
- Store inside "my_index"

👉 SOC Use:
- Search logs
- Detect attacks
- Investigate incidents

---

## 3. Common Output Plugins (Easy Table)

| Output        | What it does (Simple)        | SOC Use |
|--------------|-----------------------------|--------|
| elasticsearch| Store logs                  | investigation |
| file         | Save logs to file           | backup |
| stdout       | Print logs                  | debugging |
| kafka        | Send to pipeline            | big systems |
| tcp          | Send to another system      | log forwarding |
| syslog       | Send to syslog server       | network logs |
| s3           | Store in cloud              | long storage |

---

## 4. Important Examples (Easy Understanding)

### 1. Save logs in file

output {
  file {
    path => "/logs/output.txt"
  }
}

👉 SOC Use:
- Store logs for later analysis

---

### 2. Print logs (Debugging)

output {
  stdout {}
}

👉 SOC Use:
- Check if pipeline is working
- See parsed data

---

### 3. Send logs to another system

output {
  tcp {
    host => "destination_host"
    port => 5000
  }
}

👉 SOC Use:
- Forward logs to SIEM or another server

---

### 4. Send logs to Syslog server

output {
  syslog {
    host => "syslog_server"
    port => 514
    protocol => "udp"
  }
}

👉 SOC Use:
- Central log server

---

## 5. VERY IMPORTANT (Real SOC Understanding)

👉 You can use **multiple outputs together**

Example:

output {
  elasticsearch { hosts => ["localhost:9200"] }
  file { path => "/logs/backup.log" }
}

👉 Meaning:
- Store logs in Elasticsearch
- Also keep backup in file

---

## 6. Where configs are stored?

👉 Folder:
- /etc/logstash/conf.d/

👉 Important:
- All pipeline files go here
- Logstash reads them automatically

---

## 7. Simple Example (stdin → stdout)

input {
  stdin {}
}

output {
  stdout {}
}

👉 What happens:
- You type something
- Logstash prints it back

Example:
HELLO → output → HELLO

👉 SOC Use:
- Testing only

---

## 8. Real Example (Auth Logs — SOC Important 🔥)

input {
  file {
    path => "/var/log/auth.log"
    start_position => "beginning"
    sincedb_path => "/dev/null"
  }
}

filter {
  date {
    match => [ "message", "MMM d HH:mm:ss" ]
    target => "@timestamp"
  }
}

output {
  file {
    path => "/desktop/output.log"
  }
}

---

### What is happening here?

1. Read auth logs (login activity)
2. Extract timestamp
3. Save processed logs

---

### SOC Meaning:

👉 You can detect:
- Failed login
- Brute force attacks
- Suspicious users

---

## 9. Full Pipeline Thinking (Exam + SOC)

input → filter → output

👉 Always ask:

1. Where logs coming from? (input)
2. What to extract? (filter)
3. Where to send? (output)

---

## 10. Quick Revision (1 Minute)

👉 Output = destination  
👉 Most important = Elasticsearch  
👉 Debugging = stdout  
👉 Backup = file  

---

## 11. Easy Memory Trick

👉 "Process → Store → Analyze"

- Logstash → process
- Elasticsearch → store
- Kibana → analyze

---

## Final SOC Tip 🚨

If logs not visible in Kibana:
1. Check output → elasticsearch
2. Check Elasticsearch running
3. Check index name

---

