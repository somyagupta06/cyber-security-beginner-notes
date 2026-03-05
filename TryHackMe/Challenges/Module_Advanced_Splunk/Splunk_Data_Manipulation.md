# Splunk Data Parsing – Simple Notes (SOC Point of View)

## 1. What is Data Parsing?

Data parsing means:
Splunk reads raw logs and converts them into structured data (with fields).

Why SOC needs this?
Because without proper parsing:
- Logs look messy
- We cannot search properly
- We may miss attacks

Example:
Raw log:
2026-03-03 User=admin IP=10.0.0.5 Action=Login

After parsing:
Time → 2026-03-03
User → admin
IP → 10.0.0.5
Action → Login

Now SOC can search:
Action=Login AND User=admin

-------------------------------------

## 2. Sourcetype (Very Important)

Sourcetype tells Splunk:
"What kind of log is this?"

Example:
apache logs
windows logs
custom app logs

Why important?
Because Splunk applies parsing rules based on sourcetype.

-------------------------------------

## 3. Important Config Files (Easy Table)

inputs.conf  
Purpose: Tells Splunk where logs are coming from.

Example:
[monitor:///var/log/app.log]
sourcetype = my_app_logs

Meaning:
Monitor this file and call it my_app_logs type.

-------------------------------------

props.conf  
Purpose: Controls how logs are parsed.

SOC uses this for:
- Event breaking
- Multi-line handling
- Field extraction
- Time format
- Masking support

Example:
[my_app_logs]
TIME_FORMAT = %Y-%m-%d %H:%M:%S
SHOULD_LINEMERGE = false

-------------------------------------

transforms.conf  
Purpose: Used for:
- Masking sensitive data
- Creating new fields
- Modifying fields

Example:
[mask_cc]
REGEX = (\d{4})\d{8}(\d{4})
FORMAT = $1XXXXXXXX$2
DEST_KEY = _raw

This hides middle 8 digits of credit card.

-------------------------------------

indexes.conf  
Purpose:
Controls storage settings.

SOC cares because:
- Retention time
- Log storage size
- Compliance rules

-------------------------------------

outputs.conf  
Purpose:
Forward logs to another Splunk server.

Used in:
- Distributed SOC environment

-------------------------------------

authentication.conf  
Purpose:
Controls login method (like LDAP).

Important for SOC access control.

-------------------------------------

## 4. Event Breaking (Very Important for SOC)

Problem:
If events are not broken correctly,
One event may contain multiple logs
OR one log may split into pieces.

Solution:

BREAK_ONLY_BEFORE
Defines when a new event starts.

Example:
BREAK_ONLY_BEFORE = ^\d{4}-\d{2}-\d{2}

Meaning:
Every line starting with date = new event.

-------------------------------------

LINE_BREAKER
Defines how lines are separated.

Example:
LINE_BREAKER = ([\r\n]+)

-------------------------------------

SHOULD_LINEMERGE

false → Each line is separate event
true → Merge lines together

SOC Tip:
Usually set to false for clean logs.

-------------------------------------

## 5. Multi-line Events

Some logs (like Java errors) look like this:

2026-03-03 Error occurred
Stack trace line 1
Stack trace line 2

If not configured properly:
Splunk will treat each line as separate event.

Solution:

SHOULD_LINEMERGE = true
BREAK_ONLY_BEFORE = ^\d{4}-\d{2}-\d{2}

Meaning:
Only break when new date starts.

-------------------------------------

## 6. Masking Sensitive Data (PCI DSS Important)

Problem:
Credit card numbers in logs.

Example:
Card=1234567812345678

We must hide it.

Use transforms.conf:

[mask_cc]
REGEX = (\d{4})\d{8}(\d{4})
FORMAT = $1XXXXXXXX$2
DEST_KEY = _raw

Then connect it in props.conf:

[my_app_logs]
TRANSFORMS-mask = mask_cc

Now log becomes:
Card=1234XXXXXXXX5678

SOC Benefit:
Compliance safe
No sensitive data leak

-------------------------------------

## 7. Extracting Custom Fields

Sometimes logs are messy.

Example:
User=admin Status=Failed IP=10.0.0.5

We extract fields using:

EXTRACT-user = User=(?<user>\w+)
EXTRACT-ip = IP=(?<ip>\d+\.\d+\.\d+\.\d+)

Now we can search:
user=admin AND Status=Failed

SOC Benefit:
Fast threat hunting

-------------------------------------

## 8. Removing Unwanted Fields

If some fields are useless,
we simply do not extract them.

OR
Use transforms to clean data before indexing.

SOC Rule:
Only keep fields useful for:
- Detection
- Investigation
- Compliance

-------------------------------------

## 9. Time Parsing (Very Important)

If time is wrong:
Events will appear in wrong timeline.

Use:

TIME_PREFIX
TIME_FORMAT

Example:
TIME_PREFIX = ^
TIME_FORMAT = %Y-%m-%d %H:%M:%S

SOC Benefit:
Correct attack timeline
Accurate investigation

-------------------------------------

## 10. Key-Value Mode

If logs are JSON:

{"user":"admin","action":"login"}

Use:

KV_MODE = json

Splunk automatically extracts fields.

-------------------------------------

# Final SOC Understanding

If parsing is wrong:
- Alerts fail
- Dashboards show wrong data
- Investigation becomes slow
- Attacks may be missed

If parsing is correct:
- Clean events
- Proper timestamps
- Sensitive data masked
- Fast search
- Better detection

As SOC Analyst:
Your job is to make logs clean and searchable.

Clean logs = Better Security.

---
# Splunk App + Event Breaking + Multi-Line Events (Simple SOC Notes)

These notes explain:
1. Creating a Splunk App
2. Generating logs using a script
3. Sending logs to Splunk
4. Fixing Event Breaking
5. Fixing Multi-Line Events

All from a SOC Analyst point of view.

--------------------------------------------------

# 1. What is a Splunk App?

A Splunk App is like a **project folder** that contains:

- scripts
- configurations
- dashboards
- parsing rules

Apps help organize logs for a specific use case.

Example:
VPN Monitoring App  
Windows Security App  
Firewall Monitoring App

Location of Apps:

/opt/splunk/etc/apps

--------------------------------------------------

# 2. Structure of a Splunk App

Example App: DataApp

Path:
/opt/splunk/etc/apps/DataApp

Important folders:

bin
Stores scripts that generate logs.

default
Stores configuration files like:
inputs.conf
props.conf
transforms.conf

local
Overrides default configs if needed.

metadata
Contains app permission information.

--------------------------------------------------

# 3. Starting Splunk

Splunk is installed here:

/opt/splunk

Start Splunk using:

/opt/splunk/bin/splunk start

Login credentials:

Username: splunk  
Password: splunk123

Open Splunk Web:

10.82.169.5:8000

--------------------------------------------------

# 4. Creating a Sample Log Generator

Inside the app directory go to:

/opt/splunk/etc/apps/DataApp/bin

Create a Python script:

samplelogs.py

Code:

print("This is a sample log...")

Run the script:

python3 samplelogs.py

Output:

This is a sample log...

Now Splunk can capture this output.

--------------------------------------------------

# 5. Sending Script Logs to Splunk

Create a file:

inputs.conf

Location:

/opt/splunk/etc/apps/DataApp/default

Configuration:

[script:///opt/splunk/etc/apps/DataApp/bin/samplelogs.py]
index = main
source = test_log
sourcetype = testing
interval = 5

Meaning:

script → run the script  
index → send logs to main index  
sourcetype → log type name  
interval → run every 5 seconds

Restart Splunk:

/opt/splunk/bin/splunk restart

Now logs appear in Splunk.

SOC Benefit:
We can simulate log sources.

--------------------------------------------------

# 6. VPN Logs Scenario

Client has a custom VPN application.

VPN logs look like this:

User: Emily Davis, Server: Server C, Action: DISCONNECT  
User: John Doe, Server: Server B, Action: DISCONNECT  
User: Bob Johnson, Server: Server B, Action: DISCONNECT  

Each line should be a separate event.

--------------------------------------------------

# 7. Sending VPN Logs to Splunk

Copy vpnlogs script into:

/opt/splunk/etc/apps/DataApp/bin

Update inputs.conf:

[script:///opt/splunk/etc/apps/DataApp/bin/vpnlogs]
index = main
source = vpn
sourcetype = vpn_logs
interval = 5

Restart Splunk.

Search query:

index=main sourcetype=vpn_logs

--------------------------------------------------

# 8. Problem: Event Breaking Issue

Splunk cannot identify where one event ends.

Instead of separate events, Splunk shows:

1 big event containing many lines.

Why?

Because Splunk only breaks events at newline by default.

But here logs need custom boundaries.

--------------------------------------------------

# 9. Fixing Event Breaking

Observation:

Every log ends with:

CONNECT  
or  
DISCONNECT

So we create regex:

(DISCONNECT|CONNECT)

Now configure props.conf.

Location:

/opt/splunk/etc/apps/DataApp/default/props.conf

Configuration:

[vpn_logs]
SHOULD_LINEMERGE = true
MUST_BREAK_AFTER = (DISCONNECT|CONNECT)

Meaning:

SHOULD_LINEMERGE = true  
Combine lines until break condition.

MUST_BREAK_AFTER  
Break event after CONNECT or DISCONNECT.

Restart Splunk.

Now each VPN log becomes a separate event.

SOC Benefit:
Correct event structure for analysis.

--------------------------------------------------

# 10. Multi-Line Log Problem

Example: Authentication logs

Example event:

[Authentication]:A login attempt was observed from the user Michael Brown and machine MAC_01
at: Mon Jul 17 08:10:12 2023 which belongs to the Custom department.

One event contains **multiple lines**.

But Splunk breaks them into two events.

This is incorrect.

--------------------------------------------------

# 11. Sending Authentication Logs

Copy script:

authentication_logs

Into:

/opt/splunk/etc/apps/DataApp/bin

Update inputs.conf:

[script:///opt/splunk/etc/apps/DataApp/bin/authentication_logs]
interval = 5
index = main
sourcetype = auth_logs
host = auth_server

Restart Splunk.

Search:

index=main sourcetype=auth_logs

--------------------------------------------------

# 12. Fixing Multi-Line Events

Observation:

Every event starts with:

[Authentication]

So we define this as the start of event.

Update props.conf:

[auth_logs]
SHOULD_LINEMERGE = true
BREAK_ONLY_BEFORE = \[Authentication\]

Meaning:

SHOULD_LINEMERGE = true  
Merge multiple lines.

BREAK_ONLY_BEFORE  
Start a new event only when "[Authentication]" appears.

Restart Splunk.

Now multi-line logs are correctly grouped.

--------------------------------------------------

# 13. SOC Analyst Importance

Correct event parsing is critical because:

If parsing is wrong:

- Alerts may fail
- Events become messy
- Detection rules break
- Investigation becomes difficult

If parsing is correct:

- Logs are structured
- Fields are searchable
- Detection rules work
- Threat hunting becomes easier

--------------------------------------------------

# Final SOC Concept

SOC Analysts must ensure:

Correct Event Breaking  
Correct Multi-Line Handling  
Proper Sourcetype Configuration  
Clean and Structured Logs

Clean Logs → Better Detection → Faster Incident Response
---

# Splunk Data Manipulation (SOC Notes)

Topics covered:
1. Masking sensitive data
2. Fixing event boundaries
3. Using SEDCMD
4. Extracting custom fields
5. Using transforms.conf and fields.conf

These tasks are common when SOC teams ingest **custom logs**.

--------------------------------------------------

# 1. Why Mask Sensitive Data?

Sensitive data like:

- Credit card numbers
- Social security numbers
- Personal data

must be protected.

Compliance standards require this:

PCI DSS → Payment card protection  
HIPAA → Health data protection

SOC Rule:

Sensitive information **must not appear in logs**.

--------------------------------------------------

# 2. Example Log With Credit Card

Sample logs from purchase system:

User William made a purchase with credit card 3714-4963-5398-4313  
User John Boy made a purchase with credit card 3530-1113-3330-0000  
User Alice Johnson made a purchase with credit card 6011-1234-5678-9012  

Problem:
Credit card numbers are visible.

This is a **compliance violation**.

--------------------------------------------------

# 3. Sending Purchase Logs to Splunk

Add configuration in inputs.conf.

[script:///opt/splunk/etc/apps/DataApp/bin/purchase-details]

interval = 5  
index = main  
source = purchase_logs  
sourcetype = purchase_logs  
host = order_server  

Meaning:

script → run script  
interval → every 5 seconds  
index → store in main index  
sourcetype → purchase_logs

Search query:

index=main sourcetype=purchase_logs

--------------------------------------------------

# 4. First Problem: Event Boundary

Events may merge together.

Example:

User William purchase...  
User John Boy purchase...  

Splunk may show them as **one big event**.

We must define where events end.

Observation:

Every event ends with a **credit card number**.

We create regex to detect event end.

Then configure props.conf.

Example configuration:

[purchase_logs]

SHOULD_LINEMERGE = true  
MUST_BREAK_AFTER = \d{4}-\d{4}-\d{4}-\d{4}

Now Splunk knows where an event ends.

--------------------------------------------------

# 5. Introducing SEDCMD

SEDCMD is used in props.conf.

Purpose:

Modify raw data **before indexing**.

It uses syntax similar to **Linux sed command**.

Structure:

SEDCMD-name = s/old_value/new_value/g

Meaning:

s → substitute  
old_value → pattern to replace  
new_value → replacement  
g → replace globally

--------------------------------------------------

# 6. Masking Credit Card Numbers

Original log:

User Alice made a purchase with credit card 6011-1234-5678-9012

Goal:

User Alice made a purchase with credit card 6011-XXXX-XXXX-XXXX

Regex pattern:

-\d{4}-\d{4}-\d{4}

Replacement:

-XXXX-XXXX-XXXX

Final props.conf configuration:

[purchase_logs]

SEDCMD-maskcc = s/-\d{4}-\d{4}-\d{4}/-XXXX-XXXX-XXXX/g

Result after masking:

User Alice made a purchase with credit card 6011-XXXX-XXXX-XXXX

SOC Benefit:

Sensitive data never reaches the index.

--------------------------------------------------

# 7. Custom Field Extraction

Some logs contain useful information but Splunk cannot detect fields automatically.

Example VPN log:

User: John Doe, Server: Server C, Action: CONNECT

Fields needed:

Username  
Server  
Action

Without field extraction:

SOC cannot search properly.

--------------------------------------------------

# 8. Extracting Username

Create regex pattern:

User:\s([\w\s]+)

Explanation:

User:\s → match "User:"  
([\w\s]+) → capture username

--------------------------------------------------

# 9. Create transforms.conf

Create file:

transforms.conf

Configuration:

[vpn_custom_fields]

REGEX = User:\s([\w\s]+)

FORMAT = Username::$1

WRITE_META = true

Explanation:

REGEX → pattern to match  
FORMAT → field name to create  
$1 → first captured group

This creates field:

Username

--------------------------------------------------

# 10. Update props.conf

We must tell Splunk to use the transform.

Add:

[vpn_logs]

TRANSFORM-vpn = vpn_custom_fields

Meaning:

Apply transform rule to vpn_logs sourcetype.

--------------------------------------------------

# 11. Create fields.conf

Now define the field.

Create:

fields.conf

Configuration:

[Username]

INDEXED = true

Meaning:

Field is extracted **during indexing time**.

Benefit:

Faster searches.

--------------------------------------------------

# 12. Extract Multiple Fields

VPN log:

User: John Doe, Server: Server C, Action: CONNECT

We want:

Username  
Server  
Action

Regex:

User:\s([\w\s]+),.+(Server.+),.+:\s(\w+)

Groups:

$1 → Username  
$2 → Server  
$3 → Action

--------------------------------------------------

# 13. Update transforms.conf

Configuration:

[vpn_custom_fields]

REGEX = User:\s([\w\s]+),.+(Server.+),.+:\s(\w+)

FORMAT = Username::$1 Server::$2 Action::$3

WRITE_META = true

--------------------------------------------------

# 14. Update fields.conf

Add fields:

[Username]
INDEXED = true

[Server]
INDEXED = true

[Action]
INDEXED = true

--------------------------------------------------

# 15. Restart Splunk

Restart required after config changes.

Command:

/opt/splunk/bin/splunk restart

Search:

index=main sourcetype=vpn_logs

Now fields will appear:

Username  
Server  
Action

--------------------------------------------------

# Final SOC Understanding

SOC analysts often deal with:

Custom applications  
Unstructured logs  
Sensitive information

Important skills:

Fix event boundaries  
Handle multi-line logs  
Mask sensitive data  
Extract useful fields

These tasks make logs **searchable and secure**.

Better logs → Better detection → Faster incident response.
