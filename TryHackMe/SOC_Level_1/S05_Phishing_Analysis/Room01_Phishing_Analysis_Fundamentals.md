# Spam, Phishing & How Email Works 

---

# 1️⃣ Spam vs Phishing

## Spam

Spam = Unwanted email.

Example:
- Random advertisements
- Fake lottery wins
- “You won $1,000,000”

Fun fact:
First spam email was sent in **1978**.

Spam is annoying, but not always dangerous.

---

## Phishing

Phishing = Fake email trying to trick you.

Goal:
- Steal passwords
- Steal banking info
- Install malware
- Trick user into clicking malicious link

Very important point:

Even if a company has strong security,  
one careless employee clicking a bad link  
can allow attackers into the network.

That is why phishing is dangerous.

---

# 2️⃣ Who Invented Email?

The person who made email famous and introduced the @ symbol:

Ray Tomlinson

Email started in the 1970s on ARPANET.

---

# 3️⃣ What Makes an Email Address?

Example:

billy@johndoe.com

It has 3 parts:

User Mailbox (Username) → billy  
@ symbol  
Domain → johndoe.com  

---

## Easy Real-Life Example

Think of it like your home address.

Domain = Your street name  
User mailbox = Your house number + name  

This helps the "mail system" know where to deliver the message.

---

# 4️⃣ Email Protocols (Very Important for Exams)

When you press SEND, some network protocols work in the background.

There are 3 main protocols:

---

## SMTP (Simple Mail Transfer Protocol)

Purpose:
Used to SEND email.

Example:
Alexa sends email to Billy.

SMTP handles sending.

Default port:
25

---

## POP3 (Post Office Protocol)

Purpose:
Downloads email from server to one device.

Important Points:

- Emails stored on one device only
- Cannot sync across devices
- Once downloaded, email may be deleted from server
- Good for single device usage

---

## IMAP (Internet Message Access Protocol)

Purpose:
Access email from server on multiple devices.

Important Points:

- Emails stored on server
- Can access from phone, laptop, tablet
- Emails sync across devices
- Sent emails also stored on server
<img width="861" height="585" alt="Screenshot 2026-02-16 at 9 58 16 AM" src="https://github.com/user-attachments/assets/836932d8-593a-4403-bc43-8844140f9244" />

---

# POP3 vs IMAP (Quick Comparison Table)

| Feature | POP3 | IMAP |
|----------|-------|-------|
| Storage | On single device | On server |
| Multiple devices | No | Yes |
| Sync across devices | No | Yes |
| Sent mail storage | On device | On server |

Easy way to remember:

POP3 = One device  
IMAP = Internet access everywhere  

---

# 5️⃣ How Email Travels (Step-by-Step)

Example:
Alexa sends email to billy@johndoe.com

Step 1  
Alexa writes email and clicks SEND.

Step 2  
SMTP server checks where johndoe.com exists.

Step 3  
SMTP asks DNS:
"Where is johndoe.com?"

Step 4  
DNS gives correct mail server address.

Step 5  
Email travels through internet.

Step 6  
Email reaches destination SMTP server.

Step 7  
Email stored on POP3/IMAP server.

Step 8  
Billy opens his email app.

Step 9  
Email is:
- Downloaded (POP3)
OR
- Synced (IMAP)

Now Billy can read the email.

---

# 6️⃣ Why Security Analysts Must Understand This

As a Security Analyst, you must:

- Analyze suspicious emails
- Check sender domain
- Check headers
- Check links
- Check attachments
- Update security tools
- Block malicious domains
- Prevent future phishing attempts

Because:

Spam and phishing emails still bypass security tools sometimes.

When they do,
YOU must investigate them properly.

---

# Final Easy Summary

Spam = Unwanted email  
Phishing = Dangerous fake email  

Email structure:
username + @ + domain  

SMTP = Send email  
POP3 = Download to one device  
IMAP = Access from many devices  

Understanding email flow helps you:
- Detect phishing
- Investigate attacks
- Protect users

---
# Email Structure & Phishing Analysis (Very Easy Notes)

These notes will help you analyze malicious emails step by step.

---

# 1️⃣ Two Main Parts of an Email

Every email has:

1. Email Header  
2. Email Body  

---

## Email Header = Technical Information

It contains:

- Who sent it
- Who received it
- Date and time
- Mail servers involved
- Sending IP address
- Authentication results

Think of it like:
Shipping details on a courier package.

---

## Email Body = What You See

It contains:

- Text message
- HTML design
- Images
- Links
- Attachments

This is what the user reads.

---

# 2️⃣ Important Email Header Fields (Very Important for Analysis)

When analyzing a suspicious email, check these first:

From → Who sent the email  
To → Who received it  
Subject → What is the topic  
Date → When it was sent  

These are visible normally in your email client.

But attackers can fake some of this.

So we must check deeper.

---

# 3️⃣ Viewing the Raw Email (Very Important)

Raw email shows full technical details.

It may look scary at first.

But focus only on important fields.

---

## Important Advanced Header Fields

### X-Originating-IP

This shows:
The real IP address the email was sent from.

Very useful for investigation.

---

### Reply-To

Important trick used in phishing.

Example:

From: newsletters@company.com  
Reply-To: scammer@fake.com  

If victim replies, message goes to scammer.

Always compare:
From vs Reply-To

If different → suspicious.

---

### Authentication-Results

This shows:

- SPF result
- DKIM result
- DMARC result

These check if email is truly from that domain.

If they fail → high chance of spoofing.

---

# 4️⃣ Email Body Types

## 1. Plain Text Email

Only simple text.
No images.
No design.

Safer and simple.

---

## 2. HTML Email

Formatted email with:

- Colors
- Images
- Buttons
- Links

Most phishing emails use HTML.

Because it looks real (Amazon, Netflix, Bank, etc.)

---

# 5️⃣ Email Attachments (Very Important)

Attachments have special headers:

Content-Type  
Example: application/pdf  

Content-Disposition  
Shows if it's an attachment  

Content-Transfer-Encoding  
Often base64 (encoded data)

If base64:
It can be decoded to get the file.

⚠️ Warning:
Never double-click suspicious attachments.

---

# 6️⃣ Types of Malicious Emails

## Spam

Bulk unwanted emails.

---

## MalSpam

Malicious spam (contains malware).

---

## Phishing

Fake email pretending to be trusted company.

Goal:
Steal passwords or data.

---

## Spear Phishing

Targeted phishing.

Sent to specific person or company.

---

## Whaling

Targeted at CEO, CFO, high-level people.

---

## Smishing

Phishing via SMS.

---

## Vishing

Phishing via voice call.

---

# 7️⃣ Common Signs of Phishing (Very Important)

When analyzing email, check for these:

1. Fake sender address (spoofed domain)
2. Urgent subject (Invoice, Suspended, Action Required)
3. Generic greeting (Dear Sir/Madam)
4. Poor grammar
5. Fake login pages
6. Shortened URLs
7. Suspicious attachment
8. Mismatch between From and Reply-To

If many of these are present → likely phishing.

---

# 8️⃣ Defanging (Very Important Skill)

Defanging = Making link safe so it cannot be clicked.

Example:

Original:
http://malicious.com

Defanged:
hxxp[://]malicious[.]com

Replace:
http → hxxp  
. → [.]  
@ → [@]

This prevents accidental clicking.

CyberChef can help with defanging.

---

# 9️⃣ Easy Investigation Steps (When Alexa Sends Email to Billy)

Alexa = Victim  
Billy = Analyst  

Step 1:
Check From address carefully

Step 2:
Compare From and Reply-To

Step 3:
Check Subject for urgency

Step 4:
Check links (hover only, do not click)

Step 5:
Check attachment type

Step 6:
Check X-Originating-IP

Step 7:
Check SPF/DKIM/DMARC results

Step 8:
Defang suspicious URLs before sharing

---

# Final Easy Summary

Email has:

Header → Technical routing information  
Body → Message + links + attachments  

Phishing emails usually:
- Pretend to be trusted company
- Create urgency
- Use fake links
- Use malicious attachments

Always:
- View raw headers
- Compare From and Reply-To
- Defang links
- Never click directly

Understanding email structure helps you:
- Detect phishing
- Protect users
- Improve security tools

---

These notes are clean and easy for revision.


