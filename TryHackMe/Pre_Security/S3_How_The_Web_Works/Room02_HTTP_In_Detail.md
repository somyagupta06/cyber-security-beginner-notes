# 🌐 What is HTTP?

**HTTP = HyperText Transfer Protocol**

- This is the rule set used for loading websites.
- Created by Tim Berners-Lee (1989–1991).
- Used to transfer webpage data like:
  - HTML
  - Images
  - Videos
  - Text

When you open any website, your browser uses **HTTP** to talk to the web server.

---

# 🔒 What is HTTPS?

**HTTPS = HTTP Secure**

It is the **secure** version of HTTP.

- Data is **encrypted**
- Hackers cannot see what you send/receive
- It confirms you are talking to the **real** website, not a fake one

Example:
http://website.com (NOT secure)
https://website.com (secure)

---

# 🌍 What is a URL?

**URL = Uniform Resource Locator**

It tells your browser:
- HOW to access something  
- WHERE to access it  
- WHICH resource to get  

Example URL:
http://user:pass@tryhackme.com:80/view-room?id=1#task3
<img width="812" height="193" alt="Screenshot 2025-12-07 at 5 48 15 PM" src="https://github.com/user-attachments/assets/189dab0f-f2ed-4438-9a71-356297515d0e" />

Let’s break down all parts:

| Part | Meaning |
|------|---------|
| **Scheme** | Protocol used → http, https, ftp |
| **User:Password** | Login info (rarely used today) |
| **Host** | Domain or IP (tryhackme.com) |
| **Port** | Which port to connect (80 for HTTP, 443 for HTTPS) |
| **Path** | Location of the resource (/view-room) |
| **Query String** | Extra info (?id=1) |
| **Fragment** | A point on the page (#task3) |

---

# 📩 Making a Request (HTTP Request)

The browser sends a **request** to the server.

Minimal request:
GET / HTTP/1.1

But real requests also include **headers** for more info.
<img width="735" height="508" alt="Screenshot 2025-12-07 at 5 48 30 PM" src="https://github.com/user-attachments/assets/89a46f2b-4d7d-497a-b568-7cc4f2100231" />

---

# 📝 Example HTTP Request

GET / HTTP/1.1
Host: tryhackme.com
User-Agent: Mozilla/5.0 Firefox/87.0
Referer: https://tryhackme.com/

### Breaking it down:

- **Line 1:**  
  `GET / HTTP/1.1`  
  → “I want the homepage (/) using HTTP 1.1 protocol.”

- **Line 2:**  
  `Host: tryhackme.com`  
  → “I want THIS website.”

- **Line 3:**  
  `User-Agent: Firefox/87`  
  → Browser info (what device/software you're using).

- **Line 4:**  
  `Referer: https://tryhackme.com/`  
  → “I came to this page from this link.”

- **Blank line:**  
  → Shows request is finished.

---

# 📨 Example HTTP Response
```
HTTP/1.1 200 OK
Server: nginx/1.15.8
Date: Fri, 09 Apr 2021 13:34:03 GMT
Content-Type: text/html
Content-Length: 98
<html> <head> <title>TryHackMe</title> </head> <body> Welcome To TryHackMe.com </body> </html>
```
### Breaking it down:
- Line 1:

HTTP/1.1 200 OK

→ Server’s HTTP version + status code (200 = success)
- Line 2:

Tells the web server software (nginx).
- Line 3:

The date/time of the server.
- Line 4:

The type of data being returned (HTML, image, video, etc.)
- Line 5:

 Length of the data in bytes. Helps check if nothing is missing.
- Blank line:

 Marks end of response headers.
- Remaining lines:

 Actual content → The webpage code.
## 🎯 Super Simple Summary

- HTTP	Talks to web servers, loads websites
- HTTPS	Secure + encrypted version of HTTP
- URL	Full address telling how/where/what to load
- Request	Browser asking for something
- Response	Server giving the result
---
# 🌐 HTTP Methods 

HTTP methods tell the server **what action** the client wants to perform.

Here are the most common ones:

---

# 🟦 GET
Used to **get information** from the server.

Example:
GET /homepage
Gets webpage data → HTML, images, etc.

---

# 🟩 POST
Used to **send data** to the server and create something new.

Example:
- Submitting a form  
- Creating a new account  
- Uploading a file  

---

# 🟧 PUT
Used to **update existing data** on the server.

Example:
- Update profile information  
- Edit a blog post  

---

# 🟥 DELETE
Used to **delete a resource** from the server.

Example:
- Remove a user  
- Delete a comment  

---

# 📊 HTTP Status Codes 

When the server replies, the first line contains a **status code**.

| Range | Meaning |
|--------|---------|
| **100–199** | Informational (rare today) |
| **200–299** | Success |
| **300–399** | Redirect (content moved somewhere else) |
| **400–499** | Client errors (you made a mistake) |
| **500–599** | Server errors (server issue) |

---

# ⭐ Common HTTP Status Codes (Easy Explanations)

| Code | Meaning | Simple Explanation |
|-------|---------|----------------------|
| **200 OK** | Success | Everything worked |
| **201 Created** | New resource created | New user/post added |
| **301 Moved Permanently** | Permanent redirect | Page moved forever |
| **302 Found** | Temporary redirect | Page moved for now |
| **400 Bad Request** | Client mistake | Missing or wrong info |
| **401 Unauthorized** | Need login | Must sign in first |
| **403 Forbidden** | Not allowed | Even login won’t help |
| **404 Not Found** | Page doesn’t exist | Broken link / wrong URL |
| **405 Method Not Allowed** | Wrong method | You used GET but server needs POST |
| **500 Internal Server Error** | Server problem | Server broke while processing |
| **503 Service Unavailable** | Overloaded/maintenance | Server too busy or down |

---

# 📬 Headers (Extra Info Sent With Requests/Responses)

Headers are like **labels** that give extra information to the server or browser.  
They make web communication richer and more useful.

---

# 🟦 Common **Request** Headers (Client → Server)

| Header | Meaning |
|--------|---------|
| **Host** | Which website you want (important for servers hosting multiple sites) |
| **User-Agent** | Your browser + version (helps server format pages correctly) |
| **Content-Length** | Size of data you're sending (for forms, uploads) |
| **Accept-Encoding** | What compression your browser supports (gzip, etc.) |
| **Cookie** | Sends saved session/login info |

---

# 🟩 Common **Response** Headers (Server → Client)

| Header | Meaning |
|--------|---------|
| **Set-Cookie** | Server gives the browser a cookie to store |
| **Cache-Control** | How long the browser should cache (save) the page |
| **Content-Type** | Tells browser what kind of data (HTML, image, video, JSON) |
| **Content-Encoding** | Shows which compression was used |

---

# 🎯  Summary

| Topic | Easy Explanation |
|--------|------------------|
| **GET** | Retrieve data |
| **POST** | Send data/create |
| **PUT** | Update data |
| **DELETE** | Remove data |
| **200** | Success |
| **404** | Page missing |
| **500** | Server broke |
| **Host Header** | Tells which website you want |
| **User-Agent** | Your browser info |
| **Content-Type** | What type of data is returned |

---
# 🍪 What Are Cookies?

Cookies are **small pieces of data** stored on your computer by websites.

Why?

Because **HTTP is stateless** —  
meaning websites cannot remember who you are between requests.

Cookies help solve this problem.

---

# 🔄 How Cookies Work (Very Simple)

1. You visit a website  
2. The server sends a **Set-Cookie** header  
3. Your browser saves that cookie  
4. Every future request you make → your browser sends the cookie back  

This allows the server to **recognize you**.
<img width="761" height="734" alt="Screenshot 2025-12-07 at 5 49 23 PM" src="https://github.com/user-attachments/assets/b72cd68d-5151-4b02-905f-c9b1ca6115b0" />

---

# 🎯 What Are Cookies Used For?

Cookies are used for:

### ✔ Authentication  
Keeping you logged in.  
The cookie does **not** store your password.  
It stores a secret **token**, like:

sessionid = 9fj3k92ks01smx8

This token identifies you.

### ✔ Personal Settings  
Dark mode, language choice, font size, etc.

### ✔ Remembering Previous Visits  
“Welcome back!”  
Or saving items in your shopping cart.

---

# 🧠 Super Simple Example

### Step 1: Server sends a cookie

HTTP/1.1 200 OK
Set-Cookie: sessionid=abc123; Path=/;

Your browser stores:
sessionid = abc123

### Step 2: Browser sends cookie back

GET /dashboard HTTP/1.1
Cookie: sessionid=abc123

Server now knows:
- Who you are  
- That you're logged in  

---

# 🔍 How to View Cookies in Browser

1. Open **Developer Tools**  
   (usually F12 or Right-click → Inspect)

2. Go to the **Network** tab

3. Refresh the page

4. Click any request → Look for “Cookies”

There, you’ll see:
- Cookies sent to the server  
- Cookies received from the server  

---

# 🎯 Why Cookies Matter in Cybersecurity?

- They keep authentication states  
- Attackers try to steal cookies (session hijacking)  
- Secure websites protect cookies with encryption & flags  

But for now, remember:
**Cookies = tiny memory system for websites.**

---
