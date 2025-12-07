# 🌐 Putting It All Together — How Websites ACTUALLY Work 

When you open a website, MANY things happen secretly in the background.  
Let’s go step-by-step.
<img width="716" height="168" alt="Screenshot 2025-12-07 at 6 06 41 PM" src="https://github.com/user-attachments/assets/0ed34c57-c8c6-4329-a089-48bc6209a905" />

---

# 🟦 Step 1: DNS → Find the Server's IP
Your computer asks:
What is the IP address of this website?
DNS gives the IP so your browser knows **where to connect**.

---

# 🟥 Step 2: HTTP/HTTPS → Talk to the Server
Your browser uses HTTP/HTTPS to request:
- HTML  
- CSS  
- JavaScript  
- Images  
- Videos  

The server responds with these files.

Your browser then **renders** (displays) the page.

---

# 🟩 Extra Components That Help the Web Run Better

## 🔵 Load Balancers
Used when:
- A website is very busy  
- Or needs high availability  

The load balancer:
- Receives your request FIRST  
- Chooses which server should handle your request  

Algorithms used:
- **Round Robin** – every server gets a turn  
- **Least Busy** – send to the free server  

Also performs **health checks** (checks if servers are alive).  
If a server is dead → it stops sending traffic there.
<img width="697" height="321" alt="Screenshot 2025-12-07 at 6 06 57 PM" src="https://github.com/user-attachments/assets/7034b102-5c18-4466-b417-1af6015c482d" />

---

## 🟣 CDN (Content Delivery Network)
Stores **static files** across thousands of global servers:
- Images
- CSS  
- JavaScript  
- Videos  

When a user requests a file:
- CDN sends it from the **nearest** physical location → super fast load time.

---

## 🟠 Databases
Websites need to store data:
- Users  
- Passwords  
- Posts  
- Messages  

Common databases:
- MySQL  
- PostgreSQL  
- MongoDB  
- MSSQL  

Databases can be:
- Small (single file) OR  
- Huge cluster of servers for speed + reliability.

---

## 🔥 WAF (Web Application Firewall)
A security layer that sits **before** the web server.
<img width="684" height="141" alt="Screenshot 2025-12-07 at 6 07 09 PM" src="https://github.com/user-attachments/assets/f5a166f1-4beb-4a34-a004-a34e3f929615" />

Its job:
- Block hacking attempts  
- Block bots  
- Check if the request looks suspicious  
- Enforce **rate limiting** (limit requests per second)  

If bad → request is dropped.

---

# 🖥 How Web Servers Work

## What is a Web Server?
A **software** that listens for connections and sends web content using HTTP.

Common web servers:
- **Apache**
- **Nginx**
- **IIS** (Windows)
- **NodeJS**

Default root folders:
- Linux → `/var/www/html`
- Windows → `C:\inetpub\wwwroot`

Example:  
If you request:
http://example.com/picture.jpg
The server will send:
/var/www/html/picture.jpg

---

# 🧩 Virtual Hosts (Hosting Multiple Websites)

One web server can host MANY websites.

The server checks the **Host** header:
Host: one.com
Host: two.com
And serves the correct site based on configuration.

Each virtual host has its own folder:
one.com → /var/www/website_one
two.com → /var/www/website_two

No limit to how many websites you can host.

---

# 📄 Static vs Dynamic Content

## 🟦 Static Content
Never changes.

Examples:
- Pictures  
- CSS  
- JavaScript  
- Unchanging HTML  

Served *directly* from the server with no processing.

---

## 🟥 Dynamic Content
Changes depending on:
- User  
- Time  
- Database content  

Example:
- A blog homepage with latest posts  
- Search results  

This is done in the **backend**, using programming languages.

You **cannot see backend code** in your browser.  
You only see the final output HTML.

---

# 🧪 Backend Scripting Languages

Backend languages add logic & interactivity:
- PHP  
- Python  
- NodeJS  
- Ruby  
- Perl  

They can:
- Talk to databases  
- Process user input  
- Communicate with other services  
- Create dynamic pages  

Example:  
Request:
http://example.com/index.php?name=adam

PHP code:
```php
<html><body>Hello <?php echo $_GET["name"]; ?></body></html>
Output to the user:
Hello adam
User never sees the PHP code — only the final HTML.
