# Search & Reporting App

The **Search & Reporting App** is the main place in
**:contentReference[oaicite:1]{index=1}**
where analysts **search and analyze data**.

This app makes searching **easy, fast, and organized**.

----------------------------------

## 1) Search Head

The **Search Head** is the main search box.

### What do we do here?
- Write search queries
- Use SPL (Search Processing Language)
- Find events from logs

### Simple meaning:
This is the place where you **ask questions to your data**.

----------------------------------

## 2) Time Duration

The **Time Duration** option lets you choose
**which time data you want to see**.

### Examples:
- All Time  
  Shows all events (including real-time data)

- Last 60 Minutes  
  Shows only events from the last 1 hour

### Why it is important:
- Saves time
- Shows only relevant data

----------------------------------

## 3) Search History

The **Search History** tab saves:
- Old search queries
- Time when the search was run

### What can you do?
- Click old searches
- See past results again
- Use filter to find a specific query

### Simple meaning:
It is like a **search memory**.

----------------------------------

## 4) Data Summary

The **Data Summary** tab shows:
- Data types
- Data sources
- Hosts that generated logs

### Why this is important:
- Gives a quick idea of available data
- Helps understand network visibility
- Very useful before starting analysis

----------------------------------

## 5) Field Sidebar

The **Field Sidebar** is on the **left side** of the search page.

It helps you **understand fields inside events**.

### Two main sections:

#### Selected Fields
- Default fields:
  source
  sourcetype
  host
- These fields appear in every event
- You can add more important fields here

#### Interesting Fields
- Fields automatically found by Splunk
- Helps explore data easily

----------------------------------

## Field Symbols (Very Important)

- α  (Alpha symbol)  
  Means the field has **text values**

- #  (Hash symbol)  
  Means the field has **number values**

- Count  
  Shows how many events contain that field
  in the selected time range

----------------------------------

## One Line Revision

- Search Head = Write searches
- Time Duration = Select time
- Search History = Old searches
- Data Summary = Data overview
- Field Sidebar = Field details

----------------------------------

## Easy Memory Trick

**S T S D F**
- S = Search Head
- T = Time Duration
- S = Search History
- D = Data Summary
- F = Field Sidebar

----------------------------------
# SPL (Search Processing Language) 

**SPL** is the language used in
**:contentReference[oaicite:1]{index=1}**
to search and filter logs.

It uses:
- Operators
- Commands
- Filters

These help us find **exact data** from millions of logs.

----------------------------------

## 1) Search Field Operators

Field operators are the **basic building blocks** of SPL.
They help us filter and narrow search results.

----------------------------------

## Comparison Operators

Used to compare field values.

| Operator | Meaning | Example |
|--------|--------|--------|
| = | Equal to | UserName=Mark |
| != | Not equal to | UserName!=Mark |
| < | Less than | Age < 10 |
| <= | Less than or equal | Age <= 10 |
| > | Greater than | Outbound_traffic > 50 |
| >= | Greater or equal | Outbound_traffic >= 50 |

### Example Search
Show events where AccountName is NOT System.

Search Query:
index=windowslogs AccountName!=SYSTEM

----------------------------------

## Boolean Operators

Used to combine conditions.

| Operator | Meaning |
|--------|--------|
| AND | Both conditions must be true |
| OR | Any one condition is true |
| NOT | Excludes a value |

### Example
Show events from James account but not System.

Search Query:
index=windowslogs AccountName!=SYSTEM AND AccountName=James

----------------------------------

## Wildcard Operator

Wildcard symbol:
*

Used to match multiple values.

### Example
Search all status values starting with "fail".

Search Query:
status=fail*

This matches:
- failed
- failure

### Example with IP
Show Destination IP starting with 172.

Search Query:
index=windowslogs DestinationIp=172.*

----------------------------------

## Why Filters Are Important

Large networks create **thousands of logs per minute**.
Without filters, finding issues is very hard.

Filters help:
- Remove unwanted data
- Focus on important events

----------------------------------

## Fields Command

Used to **show or hide fields**.

### Rules:
- + means show field
- - means remove field

### Syntax
| fields <fieldname>

### Example
Show only host, User, and SourceIp.

Search Query:
index=windowslogs | fields + host + User + SourceIp

----------------------------------

## Search Command

Used to search **raw text** in events.

### Syntax
| search keyword

### Example
Show all events containing Powershell.

Search Query:
index=windowslogs | search Powershell

----------------------------------

## Dedup Command

Used to **remove duplicate values**.

### Syntax
| dedup fieldname

### Example
Show unique EventIDs.

Search Query:
index=windowslogs | table EventID User Image Hostname | dedup EventID

----------------------------------

## Rename Command

Used to **change field name** in output.

### Syntax
| rename oldname as newname

### Example
Rename User field to Employees.

Search Query:
index=windowslogs | fields + host + User + SourceIp | rename User as Employees

----------------------------------

## One Line Revision

- Comparison = Compare values
- Boolean = Combine conditions
- Wildcard = Match many values
- Fields = Show or hide fields
- Search = Find text
- Dedup = Remove duplicates
- Rename = Change field name

----------------------------------

## Easy Memory Trick

**C B W F S D R**
- C = Comparison
- B = Boolean
- W = Wildcard
- F = Fields
- S = Search
- D = Dedup
- R = Rename

----------------------------------

# SPL Ordering & Transform Commands

In **:contentReference[oaicite:1]{index=1}**,
SPL commands help us **organize, arrange, and summarize** search results.

These commands are very useful during **log investigation**.

----------------------------------
ORDERING COMMANDS
----------------------------------

## 1) Table Command

Not all fields are important.
The table command shows **only selected fields**.

### Syntax
| table field1 field2 field3

### Example
Create a table with selected columns.

Search Query:
index=windowslogs | table EventID Hostname SourceName

----------------------------------

## 2) Head Command

Shows events from the **top of the result list**.

### Default
- Shows first 10 events

### Syntax
| head number

### Example
Show only first 5 events.

Search Query:
index=windowslogs | table _time EventID Hostname SourceName | head 5

----------------------------------

## 3) Tail Command

Shows events from the **bottom of the result list**.

### Default
- Shows last 10 events

### Syntax
| tail number

### Example
Show last 5 events.

Search Query:
index=windowslogs | table _time EventID Hostname SourceName | tail 5

----------------------------------

## 4) Sort Command

Sorts results in **ascending or descending order**.

### Default
- Ascending order

### Syntax
| sort field_name

### Example
Sort by Hostname.

Search Query:
index=windowslogs | table _time EventID Hostname SourceName | sort Hostname

----------------------------------

## 5) Reverse Command

Reverses the **current order** of events.

### Syntax
| reverse

### Example
Reverse result order.

Search Query:
index=windowslogs | table _time EventID Hostname SourceName | reverse

----------------------------------
TRANSFORMATIONAL COMMANDS
----------------------------------

These commands **change raw data into numbers or charts**.
They are used for **statistics and visualization**.

----------------------------------

## 6) Top Command

Shows **most frequent values**.

### Default
- Top 10 values

### Syntax
| top field_name
| top limit=number field_name

### Example
Show top 7 processes.

Search Query:
index=windowslogs | top limit=7 Image

----------------------------------

## 7) Rare Command

Shows **least frequent values**.

### Syntax
| rare field_name
| rare limit=number field_name

### Example
Show rare processes.

Search Query:
index=windowslogs | rare limit=7 Image

----------------------------------

## 8) Highlight Command

Highlights fields in **raw event view**.

### Syntax
| highlight field1 field2

### Example
Highlight important fields.

Search Query:
index=windowslogs | highlight User host EventID Image

----------------------------------
STATS COMMANDS
----------------------------------

Used to calculate **numbers and values**.

| Command | Use |
|------|-----|
| avg | Average value |
| max | Maximum value |
| min | Minimum value |
| sum | Total value |
| count | Number of events |

### Examples
stats avg(product_price)  
stats max(user_age)  
stats count(source_IP)

----------------------------------
CHART COMMANDS
----------------------------------

Used to create **tables and visual charts**.

----------------------------------

## 9) Chart Command

Creates charts or tables using stats.

### Syntax
| chart function by field

### Example
Count events by user.

Search Query:
index=windowslogs | chart count by User

----------------------------------

## 10) Timechart Command

Creates **time-based charts**.

### Syntax
| timechart function by field

### Example
Show process activity over time.

Search Query:
index=windowslogs | timechart count by Image

----------------------------------
ONE LINE REVISION
----------------------------------

- table = select fields
- head = top events
- tail = bottom events
- sort = order results
- reverse = flip order
- top = most common
- rare = least common
- stats = calculations
- chart = visual table
- timechart = time graph

----------------------------------
EASY MEMORY TRICK
----------------------------------

**T H T S R | T R H | S C T**

----------------------------------
