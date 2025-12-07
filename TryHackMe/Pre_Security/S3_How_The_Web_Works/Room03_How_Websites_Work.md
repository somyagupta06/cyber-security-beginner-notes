# 🌍 How Websites Work 

When you open a website:
1. Your browser (Chrome/Safari) sends a **request** to a **web server**.
2. The server sends back data.
3. Your browser shows the page.

A **web server** is just a computer somewhere in the world.
<img width="1148" height="398" alt="Screenshot 2025-12-07 at 5 58 08 PM" src="https://github.com/user-attachments/assets/08a843dc-5164-4155-94ea-16eeca7855eb" />

---

# 💻 Two Main Parts of a Website

## 🟦 1. Front End (Client-Side)
This is everything you **see**:
- Buttons  
- Text  
- Images  
- Layout  

Front-end uses:
- **HTML** (structure)
- **CSS** (design)
- **JavaScript** (interactivity)

## 🟥 2. Back End (Server-Side)
This is everything you **don’t see**:
- Databases  
- Processing your login  
- Logic  
- API responses  

Back-end decides what data to send to the browser.

---

# 🧱 HTML (HyperText Markup Language)

HTML = skeleton of the website.

A simple page has:

```
<!DOCTYPE html> → Tells browser it's HTML5 <html> → Root element <head> → Info about the page <body> → Visible content <h1> → Heading <p> → Paragraph
```

## 🏷 HTML Tags & Attributes
### Tags:
- <p> → paragraph
- <img> → image
- <button> → button
### Attributes:
- class="" → used for styling
- id="" → unique name for JavaScript
- src="" → image or script location
### Example:
- <p class="highlight">Hello</p>
- <img src="cat.jpg">
- <button id="loginBtn">Login</button>
You can view ANY website’s HTML using View Page Source.
## ⚡ JavaScript (JS)
JavaScript makes pages interactive.
### Examples of things JS can do:

- Change text when you click a button
- Show alerts
- Move animations
- Update content without refreshing
### Example JS:
```
document.getElementById("demo").innerHTML = "Hack the Planet";
```
### Example JS on button click:
```
<button onclick='document.getElementById("demo").innerHTML="Button Clicked";'>
Click Me!
</button>
```
## 🔐 Sensitive Data Exposure
This happens when developers accidentally leave private info inside:
- HTML
- JavaScript
- Comments
- Hidden fields
Examples of exposed sensitive data:
- Hidden admin links
- Test usernames/passwords
- API keys
- Debug information
Attackers can see all of this using “View Page Source”.
<img width="479" height="213" alt="Screenshot 2025-12-07 at 5 58 37 PM" src="https://github.com/user-attachments/assets/b575505d-930b-4839-aa7d-b698ee490704" />

Rule #1: NEVER trust the front-end. Anything on the page is visible to the user.

## 🚨 HTML Injection
HTML Injection happens when:
- A website takes user input
- Does NOT sanitize (clean) it
- And prints that input back onto the page
### Example:
- A form asks your name → shows your name on the page.
- If the site does NOT clean input, a user could type:
```
<h1>HACKED</h1>
```
- And the page would display a big “HACKED” heading.
- This happens because the browser treats your text as real HTML.
<img width="1131" height="594" alt="Screenshot 2025-12-07 at 5 58 53 PM" src="https://github.com/user-attachments/assets/04432135-f829-4f9d-8015-7c58963db82c" />

### Why is this dangerous?
- Page styling can be changed
- Fake buttons can be added
- JavaScript can sometimes be executed (leading to XSS)
- Rule #2: Developers must sanitize input
### Example sanitization:
- Remove < >
- Allow plain text only
