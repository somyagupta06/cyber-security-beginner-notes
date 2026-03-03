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
