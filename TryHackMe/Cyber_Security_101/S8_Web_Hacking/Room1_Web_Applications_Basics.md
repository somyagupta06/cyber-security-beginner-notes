# Uniform Resource Locator (URL)

A **URL (Uniform Resource Locator)** is like the **address of a house**, but for the Internet.  
It tells your browser *where to go* to find a webpage, video, photo, or any other resource.

---

## Anatomy of a URL

A URL has multiple parts.  
Example:  
https://username:password@www.example.com:443/path/page.html?search=books#section2

### 1. Scheme (Protocol)
- Defines how the browser communicates with the server.  
- Common schemes:
  - `http` → Normal web pages (not encrypted).
  - `https` → Secure web pages (encrypted).  
- **Tip:** Always prefer `https` for security.

---

### 2. User (Optional)
- Sometimes URLs may include login info.  
Example:  
ftp://user123:pass@fileserver.com/file.txt
- Nowadays, this is **rare** because it’s unsafe (can expose passwords).

---

### 3. Host / Domain
- The **main name of the website** you want to visit.  
Example:  
www.example.com
- Domains must be unique and registered.  
- **Warning:** Watch out for fake look-alike domains (typosquatting).  
  - Example:  
    - Real: `facebook.com`  
    - Fake: `faceb00k.com`

---

### 4. Port
- Think of it as a **door number** for communication with the server.  
- Common ports:
  | Port | Protocol |
  |------|----------|
  | 80   | HTTP     |
  | 443  | HTTPS    |

Example:  
https://example.com:443

---

### 5. Path
- Shows the **location of a file or page** on the server.  
Example:  
https://example.com/products/mobile/iphone.html
- Here `/products/mobile/iphone.html` is the path.

---

### 6. Query String
- Starts with a **question mark (?)**.  
- Used to send extra info (like search terms, form data).  
Example:  
https://example.com/search?query=laptop&sort=price
- Here:
  - `query=laptop`
  - `sort=price`

⚠️ Security Note:  
Users can modify query strings. Always validate them to prevent attacks.

---

### 7. Fragment
- Starts with a **hash symbol (#)**.  
- Jumps to a specific section of the webpage.  
Example:  
https://example.com/docs/page.html#chapter2
- Opens directly at **Chapter 2** on the page.

---

## Quick Comparison Table

| Part       | Example Value             | Role / Use Case                          |
|------------|---------------------------|------------------------------------------|
| Scheme     | `https`                   | Defines protocol (secure / non-secure)   |
| User       | `username:password@`      | Rare login info in URL (unsafe)          |
| Domain     | `www.example.com`         | Website name / host                      |
| Port       | `:443`                    | Server communication door (80 / 443)     |
| Path       | `/products/mobile/`       | Location of file/page on server          |
| Query      | `?query=laptop&sort=asc`  | Extra info (search, form data)           |
| Fragment   | `#section2`               | Jump to a section of the page            |

---

## Easy Analogy 🏠
Think of a URL like a **house address**:
- **Scheme** = Road type (highway, secure road).  
- **User** = Person with access key.  
- **Domain** = Colony / Society name.  
- **Port** = Gate number.  
- **Path** = Street + House number.  
- **Query** = Extra directions (e.g., “look near the red shop”).  
- **Fragment** = Exact room in the house.

---
# HTTP Messages

**HTTP messages** are the packets of data exchanged between a **client (user/browser)** and a **server**.  
They are the foundation of how the web works.

There are two main types:
1. **HTTP Request** → Sent by the client (user) to the server.  
2. **HTTP Response** → Sent by the server back to the client.

---

## Structure of HTTP Messages

Every HTTP message has **4 main parts**:

1. **Start Line** → Introduces the message.  
2. **Headers** → Extra info in key-value pairs.  
3. **Empty Line** → Divider between headers and body.  
4. **Body** → Main data/content.

---

## Example: HTTP Request (Login)
``` bash
POST /login HTTP/1.1
Host: www.example.com
Content-Type: application/json
Content-Length: 34
{
"username": "john",
"password": "12345"
}
```

**Breakdown:**
- **Start Line:** `POST /login HTTP/1.1` → Method = POST, Path = `/login`, Protocol = HTTP/1.1  
- **Headers:** 
  - `Host: www.example.com` → Server’s domain  
  - `Content-Type: application/json` → Data format  
  - `Content-Length: 34` → Size of body  
- **Empty Line:** Marks the end of headers.  
- **Body:** `{ "username": "john", "password": "12345" }`  

---

## Example: HTTP Response (Login Successful)
``` bash
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 45
{
"status": "success",
"message": "Login successful"
}
```

**Breakdown:**
- **Start Line:** `HTTP/1.1 200 OK` → Protocol = HTTP/1.1, Status Code = 200, Message = OK  
- **Headers:**
  - `Content-Type: application/json` → Data format  
  - `Content-Length: 45` → Size of body  
- **Empty Line:** Divider between headers and body.  
- **Body:** `{ "status": "success", "message": "Login successful" }`

---

## Common HTTP Methods (in Requests)

| Method | Use Case |
|--------|----------|
| GET    | Request data (e.g., view a page) |
| POST   | Send data (e.g., login form) |
| PUT    | Update existing resource |
| DELETE | Remove a resource |

---

## Common HTTP Status Codes (in Responses)

| Code | Meaning                  | Example Use |
|------|--------------------------|-------------|
| 200  | OK (Success)             | Page loads correctly |
| 201  | Created                  | New resource created |
| 400  | Bad Request              | Client error (invalid input) |
| 401  | Unauthorized             | Requires login/authentication |
| 403  | Forbidden                | Access denied |
| 404  | Not Found                | Page not found |
| 500  | Internal Server Error    | Server problem |

---

## Why Understanding HTTP Messages Matters

- **Communication:** They define how client and server talk to each other.  
- **Debugging:** Helps fix issues when requests/responses fail.  
- **Security:** Ensures safe handling of data (avoiding leaks/attacks).  
- **Performance:** Properly structured messages improve speed & reliability.  

---

## Easy Analogy 📬

Think of **HTTP messages** like sending and receiving letters:  
- **Request (Client → Server):** You send a letter with instructions (like “I want to log in”).  
- **Response (Server → Client):** The server replies with a letter containing results (like “Login successful”).  

- **Start Line:** Envelope subject → tells what this letter is about.  
- **Headers:** Extra instructions → e.g., "Deliver via Express", "Handle with Care".  
- **Empty Line:** Blank space between envelope info and the actual letter.  
- **Body:** The actual content of the letter (data).

---
# HTTP Requests

An **HTTP Request** is what a **client (user/browser)** sends to a **server** to interact with a web application.  
It’s often the **first point of contact** between a user and a server, so understanding it is very important (especially for **cyber security**).
<img width="791" height="397" alt="Screenshot 2025-08-23 at 6 23 11 PM" src="https://github.com/user-attachments/assets/d87b837e-cce1-4c3c-a004-8ec02dafac63" />

---

## Structure of an HTTP Request

Every request has 3 main parts:

1. **Request Line (Start Line)**  
   → Defines the action (Method), location (Path), and protocol version.  
   Example:
``` bash     
GET /login HTTP/1.1
```
3. **Headers**  
→ Extra info in key-value pairs. Example: `Content-Type: application/json`

4. **Body (Optional)**  
→ Only in some requests (e.g., POST). Contains data sent to the server.  

---

## Example Request
``` bash
POST /login HTTP/1.1
Host: www.example.com
Content-Type: application/json
Content-Length: 48
{
"username": "john",
"password": "12345"
}
```

- **Request Line:** `POST /login HTTP/1.1`  
- **Headers:** Host, Content-Type, Content-Length  
- **Body:** `{ "username": "john", "password": "12345" }`

---

## HTTP Methods

The **method** tells the server *what action* the client wants.  

### Common Methods

| Method  | Purpose | Security Reminder |
|---------|---------|-------------------|
| GET     | Fetch data (read-only) | Don’t expose sensitive info (tokens, passwords) in GET (they appear in logs & URLs). |
| POST    | Send data (create/update) | Validate and sanitize inputs → prevent **SQL Injection/XSS**. |
| PUT     | Replace/update a resource | Check user authorisation before allowing updates. |
| DELETE  | Remove a resource | Ensure only authorised users can delete. |

---

### Other Methods

| Method  | Purpose | Notes |
|---------|---------|-------|
| PATCH   | Update part of a resource | Validate carefully to avoid inconsistent data. |
| HEAD    | Same as GET but only headers | Useful for checking metadata. |
| OPTIONS | Lists available methods for a resource | Good for debugging but can reveal too much info → disable if not needed. |
| TRACE   | Echoes request for debugging | Usually disabled → can expose sensitive info. |
| CONNECT | Creates a secure tunnel (e.g., HTTPS) | Less common, but critical for encrypted comms. |

---

## URL Path

- Defines **where the resource is located** on the server.  
Example:  
https://tryhackme.com/api/users/123
- Path = `/api/users/123`

🔒 Security Practices:
- Validate paths → prevent unauthorised access.  
- Sanitize paths → avoid injection attacks.  
- Protect sensitive endpoints (like `/admin/`, `/config/`).  

---

## HTTP Version

The **version** shows which HTTP protocol is used:  

| Version   | Year | Features |
|-----------|------|----------|
| HTTP/0.9  | 1991 | Only supported GET |
| HTTP/1.0  | 1996 | Added headers, content types |
| HTTP/1.1  | 1997 | Persistent connections, chunked transfer (still very common) |
| HTTP/2    | 2015 | Multiplexing, header compression, faster performance |
| HTTP/3    | 2022 | Based on QUIC, faster & more secure |

⚡ **Tip:** Upgrading to HTTP/2 or HTTP/3 = better speed & security.

---

## Quick Analogy 📬

Think of an **HTTP request** like sending a letter:

- **Request Line:** Subject line → “What do I want?” (e.g., GET /login)  
- **Headers:** Extra instructions → “Deliver via Express”, “Language: English”  
- **Body:** Actual content → “Here’s my login info”  

---

## Security Summary 🛡️

- Don’t put sensitive info in **GET** URLs.  
- Always validate/sanitize inputs in **POST, PUT, PATCH**.  
- Protect critical paths with authentication.  
- Disable unnecessary methods (**TRACE, OPTIONS**) if not required.  
- Prefer modern versions (**HTTP/2 or HTTP/3**) for better security & performance.  

---
# HTTP Request Headers & Request Body

When a **client (browser/user)** sends a request to a **server**, it often includes:
1. **Request Headers** → Extra info for the server.  
2. **Request Body** → Actual data (only in some methods like POST, PUT).  

---

## Request Headers

Request Headers give **additional details** about the request. They are written as **key: value** pairs.

### Common Request Headers

| Header       | Example                                                                 | Description |
|--------------|-------------------------------------------------------------------------|-------------|
| **Host**     | `Host: tryhackme.com`                                                   | Specifies which server (domain) the request is for. |
| **User-Agent** | `User-Agent: Mozilla/5.0`                                             | Tells the server info about the browser or client making the request. |
| **Referer**  | `Referer: https://www.google.com/`                                      | Shows the URL of the page where the request came from. |
| **Cookie**   | `Cookie: user_type=student; room=introtowebapplication; room_status=in_progress` | Sends stored data (cookies) back to the server (like login sessions, preferences). |
| **Content-Type** | `Content-Type: application/json`                                    | Defines the format of data in the request body. |

---

## Request Body

- Used in requests like **POST** or **PUT**.  
- Contains the **data being sent to the server**.  
- Can be formatted in multiple ways:  

---

### 1. URL Encoded (application/x-www-form-urlencoded)

- Data sent as key-value pairs.  
- Multiple pairs are separated by `&`.  
- Special characters are percent-encoded.  

**Example:**
``` bash
POST /profile HTTP/1.1
Host: tryhackme.com
User-Agent: Mozilla/5.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 33
name=Aleksandra&age=27&country=US
```

---

### 2. Form Data (multipart/form-data)

- Used for **file uploads** or sending multiple blocks of data.  
- Each block is separated by a **boundary string**.  

**Example:**
``` bash
POST /upload HTTP/1.1
Host: tryhackme.com
User-Agent: Mozilla/5.0
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW
----WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="username"

aleksandra
----WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="profile_pic"; filename="aleksandra.jpg"
Content-Type: image/jpeg

[Binary Data Here representing the image]
----WebKitFormBoundary7MA4YWxkTrZu0gW--

```
---

### 3. JSON (application/json)

- Data is structured in **name : value** pairs.  
- Enclosed within `{ }`.  
- Common in modern APIs.  

**Example:**
``` bash
POST /api/user HTTP/1.1
Host: tryhackme.com
User-Agent: Mozilla/5.0
Content-Type: application/json
Content-Length: 62
{
"name": "Aleksandra",
"age": 27,
"country": "US"
}
```

---

### 4. XML (application/xml)

- Data is wrapped in **tags** with opening `<tag>` and closing `</tag>`.  
- Supports nesting (tags inside tags).  

**Example:**
``` bash
POST /api/user HTTP/1.1
Host: tryhackme.com
User-Agent: Mozilla/5.0
Content-Type: application/xml
Content-Length: 124
<user> <name>Aleksandra</name> <age>27</age> <country>US</country> </user>
```
# Comparison of Request Body Formats

| Format       | Content-Type                       | Use Case                          |
|--------------|------------------------------------|-----------------------------------|
| URL Encoded  | `application/x-www-form-urlencoded` | Simple form submissions           |
| Form Data    | `multipart/form-data`              | Uploading files/images            |
| JSON         | `application/json`                 | Modern APIs, structured data      |
| XML          | `application/xml`                  | Legacy systems, structured/nested data |

---

## Easy Analogy 📦

Think of **Request Headers & Body** like sending a **package**:

- **Headers** → Info written on the box (address, sender, fragile, etc.).  
- **Body** → The actual content inside the package (clothes, books, files).  

---
# HTTP Responses

When you interact with a **web application**, the **server sends back an HTTP Response**.  
This response tells you whether your request was successful or if something went wrong.

---

## Status Line

The **first line** of every HTTP Response is called the **Status Line**.  
It contains 3 important parts:

1. **HTTP Version** → Which HTTP version is being used (e.g., HTTP/1.1).  
2. **Status Code** → A three-digit number showing the outcome of the request.  
3. **Reason Phrase** → A short explanation of the status code in words.  

**Example:** 
``` bash
HTTP/1.1 200 OK
```
- HTTP Version → `HTTP/1.1`  
- Status Code → `200`  
- Reason Phrase → `OK`

---

## Categories of Status Codes

HTTP status codes are divided into **5 main categories**:

| Category                     | Code Range | Meaning |
|-------------------------------|------------|---------|
| **Informational**             | 100–199    | The request is being processed, continue sending data. |
| **Successful**                | 200–299    | The request was successful, and the resource is returned. |
| **Redirection**               | 300–399    | The requested resource has moved, follow the new location. |
| **Client Error**              | 400–499    | Something is wrong with the request (user-side issue). |
| **Server Error**              | 500–599    | Something went wrong on the server. |

---

## Common Status Codes & Reason Phrases

| Code | Reason Phrase         | Meaning |
|------|-----------------------|---------|
| **100** | Continue             | The server got part of the request and is ready for the rest. |
| **200** | OK                   | The request was successful, and the server is sending the resource. |
| **301** | Moved Permanently    | The resource has moved to a new URL (use the new one from now on). |
| **404** | Not Found            | The resource could not be found at the given URL. |
| **500** | Internal Server Error| The server had a problem while processing the request. |

---

## Example Response
``` bash
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 62
{
"status": "success",
"message": "Data fetched correctly"
}
```

- **Status Line:** `HTTP/1.1 200 OK`  
- **Headers:** `Content-Type`, `Content-Length`  
- **Body:** The actual data (JSON in this case).  

---

## Easy Analogy 📬

Think of an **HTTP Response** like getting a **reply letter** from the server:  

- **Status Line:** The envelope message → “Delivered Successfully” or “Address Not Found”.  
- **Headers:** Extra notes → delivery type, content info, etc.  
- **Body:** The actual content of the letter (the data you wanted).  

---
# HTTP Response Headers & Response Body

When a **web server** sends back an **HTTP response**, it includes:

1. **Response Headers** → Extra info about the response (in key-value pairs).  
2. **Response Body** → The actual content/data sent back to the client.  

---

## Example: HTTP Response with Headers

HTTP/1.1 200 OK
Date: Fri, 23 Aug 2024 10:43:21 GMT
Content-Type: text/html; charset=utf-8
Content-Length: 1256
Server: nginx
Set-Cookie: sessionId=38af1337es7a8
Cache-Control: max-age=600

- **Status Line:** `HTTP/1.1 200 OK`  
- **Response Headers:** Date, Content-Type, Content-Length, Server, etc.  
- **Response Body:** The actual page/data (HTML, JSON, image, etc.).  

---

## Required Response Headers

These headers are **essential** for HTTP responses to work correctly.

| Header        | Example                                   | Description |
|---------------|-------------------------------------------|-------------|
| **Date**      | `Date: Fri, 23 Aug 2024 10:43:21 GMT`     | Shows when the response was created. |
| **Content-Type** | `Content-Type: text/html; charset=utf-8` | Defines the type of content (HTML, JSON, etc.) + character encoding. |
| **Server**    | `Server: nginx`                           | Shows the server software. Useful for debugging but risky (can expose info to attackers). |

---

## Other Common Response Headers

| Header        | Example                                   | Description |
|---------------|-------------------------------------------|-------------|
| **Set-Cookie** | `Set-Cookie: sessionId=38af1337es7a8` | Sends cookies to the client. Use `HttpOnly` (not accessible via JS) and `Secure` (HTTPS only) for safety. |
| **Cache-Control** | `Cache-Control: max-age=600` | Defines how long the response can be cached. Use `no-cache` for sensitive data. |
| **Location**  | `Location: /index.html` | Used in redirects (3xx responses). Points to the new location of the resource. Must be validated to avoid **open redirect attacks**. |

---

## Response Body

- The **Response Body** is where the **actual data/content** is returned.  
- Can be:
  - **HTML** → Webpages  
  - **JSON** → API responses  
  - **XML** → Legacy systems  
  - **Images / Files** → Media or downloadable files  

🔒 **Security Tip:**  
Always **sanitize & escape user-generated content** before including it in the response → prevents **Cross-Site Scripting (XSS)** and injection attacks.  

---

## Easy Analogy 📦

Think of a **server response** like receiving a **delivery package**:

- **Response Headers** → The delivery slip on the box (date, weight, fragile label, return address).  
- **Response Body** → The actual item inside the package (book, clothes, phone, etc.).  

---
# 🛡️ HTTP Security Headers

**Security Headers** improve the security of web applications by protecting against attacks like:
- Cross-Site Scripting (XSS)  
- Clickjacking  
- Data injection  
- Insecure redirects  

👉 You can test any website’s headers at: [securityheaders.io](https://securityheaders.io)

---

## 1. Content-Security-Policy (CSP)

The **CSP header** defines which domains/resources are allowed.  
It helps mitigate **XSS attacks** by blocking malicious scripts/styles from untrusted sources.  

**Example:**
``` bash
Content-Security-Policy: default-src 'self'; script-src 'self'
https://cdn.tryhackme.com; style-src 'self'
```
**Breakdown:**
- `default-src 'self'` → Only allow resources from the current domain.  
- `script-src 'self' https://cdn.tryhackme.com` → Allow scripts from current domain + trusted CDN.  
- `style-src 'self'` → Allow styles only from the current domain.  

✅ Extra layer of defense against injection attacks.

---

## 2. Strict-Transport-Security (HSTS)

Forces browsers to always use **HTTPS** instead of HTTP.  
Prevents downgrade attacks & cookie hijacking.  

**Example:**
``` bash
Strict-Transport-Security: max-age=63072000; includeSubDomains; 
preload
```
**Directives:**
- `max-age=63072000` → Valid for 2 years (time in seconds).  
- `includeSubDomains` → Apply to all subdomains too.  
- `preload` → Adds domain to browser preload list (HSTS enforced even on first visit).  

---

## 3. X-Content-Type-Options

Prevents browsers from **MIME type sniffing** (guessing content type).  
This avoids executing malicious files disguised as something else.  

**Example:**
``` bash
X-Content-Type-Options: nosniff
```
- `nosniff` → Browser must trust only the `Content-Type` header, not guess.

---

## 4. Referrer-Policy

Controls how much **referrer info** (URL of the previous page) is sent when navigating.  

**Examples:**
``` bash
Referrer-Policy: no-referrer
Referrer-Policy: same-origin
Referrer-Policy: strict-origin
Referrer-Policy: strict-origin-when-cross-origin
```
**Breakdown:**
- `no-referrer` → Don’t send any referrer info.  
- `same-origin` → Only send referrer when navigating within the same site.  
- `strict-origin` → Send referrer only if protocol is the same (HTTPS → HTTPS).  
- `strict-origin-when-cross-origin` → Send full referrer within same origin, but only origin info when going to other domains.  

---

## ⚡ Quick Analogy 📦

Imagine a **secured courier package**:
- **CSP** → Allowed delivery partners only (trusted couriers).  
- **HSTS** → Always use a secure truck (HTTPS).  
- **X-Content-Type-Options** → Don’t guess what’s inside; trust the label.  
- **Referrer-Policy** → Decide how much sender info is shown on the package.  

---
