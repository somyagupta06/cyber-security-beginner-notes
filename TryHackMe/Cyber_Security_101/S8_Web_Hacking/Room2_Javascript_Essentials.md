# 🚀 JavaScript Basics 

JavaScript (JS) is a **programming language** that makes websites **interactive**.  

- HTML → Structure (skeleton of the page)  
- CSS → Styling (colors, fonts, layout)  
- JS → Behavior (interactivity, logic, animations, validation)  

Example:  
- HTML makes a button  
- CSS styles the button  
- JS decides what happens when you click the button  



## 🔑 1. Variables

A variable is like a container that stores data.  
Think of it like a bucket with a label. You put data in the bucket and use the label (name) to find it later.  

In JavaScript, there are 3 ways to declare variables:  

| Keyword | Scope           | Change Allowed? | Example |
|---------|-----------------|-----------------|---------|
| var     | Function-level  | Yes             | var name = "Alex"; |
| let     | Block-level     | Yes             | let age = 20; |
| const   | Block-level     | No (fixed)      | const pi = 3.14; |

Example:
``` javascript
var city = "Delhi";
let age = 25;
const country = "India";
console.log(city);
console.log(age);
console.log(country);
```

## 🔑 2. Data Types

Data types tell what kind of value is stored in a variable.  

Example:
``` javascript
let name = "Alice"; // String
let age = 25; // Number
let isStudent = true; // Boolean
let score = null; // Null
let grade; // Undefined
let person = { // Object
id: 1,
name: "Bob"
};
```


## 🔑 3. Functions

A function is a block of code that performs a specific task.  
It helps to reuse code instead of writing it again and again.  

Example:
``` javascript
function greet(name) {
console.log("Hello, " + name + "!");
}
greet("Alice");
greet("Bob");


```

## 🔑 4. Loops

Loops let you repeat a block of code multiple times until a condition is false.  

Example:
``` javascript
for (let i = 1; i <= 5; i++) {
console.log("Student Roll No: " + i);
}
```


## 🔑 5. Request-Response Cycle

In web development:  
- Client (Browser) sends a request to the Server  
- Server processes it and sends a Response back  

Example: When you open a website, your browser requests the server for the webpage, and the server sends back the HTML, CSS, JS files.



## 🔑 6. Hello World Program

Example:
``` javascript
console.log("Hello, World!");
```


## 🔑 7. Control Flow (if-else)

Example:
``` javascript
let age = 18;
if (age >= 18) {
console.log("You are an adult.");
} else {
console.log("You are a minor.");
}

```


## 🔑 8. Simple Program (Addition)

Example:
``` javascript
let x = 5;
let y = 10;
let result = x + y;
console.log("The result is: " + result);
```
---
# JavaScript in HTML 

JavaScript (JS) makes webpages interactive.  
It works together with **HTML (structure)** and **CSS (style)**.  

There are 2 main ways to add JS into HTML:
1. Internal JavaScript
2. External JavaScript



## 1) Internal JavaScript
👉 Means writing JS code **inside the same HTML file**.

- Written between `<script> </script>`.
- Can be placed in `<head>` (before page loads) or `<body>` (after page content).

### Example (Internal)
File name: internal.html

Steps:
1. Create a new file → internal.html
2. Open in text editor
3. Paste code:
   - Title: "Internal JS"
   - Heading: "Addition of Two Numbers"
   - Paragraph with id="result"
   - Script that adds two numbers and shows the result

Output in browser:
The result is: 15



## 2) External JavaScript
👉 Means writing JS code **in a separate .js file** and then linking it to HTML.

Benefits:
- HTML looks clean
- Easy to maintain, especially in big projects

### Example (External)
File name: script.js
- Write JS code inside this file (same code as internal example).

File name: external.html
- Same HTML as before
- At bottom: `<script src="script.js"></script>` (this links the JS file)

Output in browser:
The result is: 15  
(Same as internal)



## Comparison: Internal vs External

| Feature        | Internal JS                    | External JS                      |
|----------------|--------------------------------|----------------------------------|
| Location       | Inside HTML file               | In a separate .js file           |
| Use Case       | Good for beginners, small code | Best for large projects          |
| Code Cleanliness | HTML + JS mixed together      | HTML and JS are separated        |
| Maintenance    | Harder to manage               | Easier to manage                 |



## How to Check Internal or External JS in a Website

1. Open the page in Chrome.
2. Right-click → **View Page Source**.
3. Look for `<script>` tags:
   - If you see code directly inside `<script> ... </script>` → **Internal JS**.
   - If you see `<script src="something.js"></script>` → **External JS**.

### Example:
- Internal JS:
  `<script> alert("Hello!"); </script>`
- External JS:
  `<script src="app.js"></script>`



## Practice
Open [https://tryhackme.com](https://tryhackme.com) in Chrome:
1. Right-click → View Page Source
2. Search for `<script>`
3. You will find both internal and external JS used.
---
# JavaScript Dialogue Boxes (Easy Notes)

One main use of JavaScript is **interaction with users** using dialogue boxes.  
These boxes can:
- Show messages
- Ask for input
- Ask for confirmation

JS provides 3 built-in functions:
1. alert()
2. prompt()
3. confirm()

⚠️ Warning: If misused, these functions can be exploited by attackers (e.g., XSS attacks).  
So, always use them securely.

---

## 1) Alert

- Shows a **message box with only an "OK" button**.  
- Used to display information or warnings.

### Example:
```
alert("Hello THM");
```

👉 Steps:
1. Open Chrome
2. Press **Ctrl + Shift + J** (open console)
3. Type: `alert("Hello THM")`
4. Press Enter → You will see a pop-up message box.

---

## 2) Prompt

- Shows a **message box with an input field**.  
- Used to ask the user for input.  
- Returns:
  - User’s input (if OK clicked)
  - `null` (if Cancel clicked)

### Example:
```
name = prompt("What is your name?");
alert("Hello " + name);
```

👉 Steps:
1. Open Chrome console
2. Paste the code
3. A box will appear → type your name
4. After clicking OK → alert will say `Hello <yourname>`

---

## 3) Confirm

- Shows a **message box with "OK" and "Cancel" buttons**.  
- Used to ask user for confirmation.  
- Returns:
  - `true` (if OK clicked)
  - `false` (if Cancel clicked)

### Example:
```
confirm("Do you want to proceed?");
```

👉 Steps:
1. Open Chrome console
2. Type `confirm("Do you want to proceed?")`
3. Press Enter
4. Pop-up box appears → If you click OK → returns true, else false.

---

## How Hackers Can Exploit These

Hackers can misuse alert/prompt/confirm to annoy or trick users.  

### Example: Malicious File
File: invoice.html

```
<!DOCTYPE html>
<html>
<head><title>Hacked</title></head>
<body>
<script>
   for (let i = 0; i < 3; i++) {
      alert("Hacked");
   }
</script>
</body>
</html>
```

👉 When you open this file, it shows 3 alert pop-ups saying "Hacked".  
If the loop was set to 500, you’d be forced to close 500 alerts one by one 😵.

This is just a **basic prank**, but real hackers can use similar tricks for harmful attacks.

---

## Safety Tip
✅ Only run JS files from **trusted sources**.  
❌ Never open unknown HTML/JS files from strangers.  
---
# Minification, Obfuscation, and Deobfuscation (Easy Notes)

We already know how normal JavaScript works.  
But sometimes, when you look at JS files on websites, the code is **not human-readable**.  
That’s because of **Minification** or **Obfuscation**.



## 🔹 Minification
**Definition:** Minification means **compressing JS code** by removing:
- Extra spaces
- Line breaks
- Comments
- Even shortening variable names

**Purpose:**  
- Makes file size smaller  
- Helps web pages load faster (important for big websites)  

👉 Example:  

Original code:
```
function hi() {
  alert("Welcome to THM");
}
hi();
```

Minified code:
```
function hi(){alert("Welcome to THM");}hi();
```

Notice: Code is smaller, but still does the same thing.



## 🔹 Obfuscation
**Definition:** Obfuscation means making JS **harder to read** on purpose.  
- Renames variables/functions into random names  
- Adds extra/dummy code  
- Looks like gibberish 🤯  
<img width="648" height="293" alt="Screenshot 2025-08-24 at 10 43 09 PM" src="https://github.com/user-attachments/assets/de67299e-7685-4b16-8c7e-6dac15c14ed7" />

**Purpose:**  
- To hide logic from humans (security through confusion)  
- Prevents attackers from easily understanding your code  

👉 Example:  

Original code:
```
function hi() {
  alert("Welcome to THM");
}
hi();
```

Obfuscated code (from tool):
```
(function(_0x114713,_0x2246f2){var _0x51a830=_0x33bf; ... }());
function hi(){
   var _0xab1127={'FvtWc':"Welcome to THM"};
   alert(_0xab1127['FvtWc']);
}
hi();
```

This looks like gibberish, but it still works exactly the same ✅



## 🔹 Practical Example

1. Create `hello.html`
```
<!DOCTYPE html>
<html>
<head>
   <title>Obfuscated JS Code</title>
</head>
<body>
   <h1>Obfuscated JS Code</h1>
   <script src="hello.js"></script>
</body>
</html>
```

2. Create `hello.js`
```
function hi() {
   alert("Welcome to THM");
}
hi();
```

3. Open `hello.html` → You see: **Welcome to THM** (alert box).  
4. Inspect → Sources tab → You can see the code clearly.  
5. Now use an online JS obfuscator → paste `hello.js` → get gibberish version.  
6. Replace the old `hello.js` with gibberish version → reload → Still works same.



## 🔹 Deobfuscation
**Definition:** Deobfuscation means converting **obfuscated code back to readable code**.
<img width="656" height="281" alt="Screenshot 2025-08-24 at 10 43 48 PM" src="https://github.com/user-attachments/assets/7bc712f0-fd63-4f59-9965-8e9f8073231c" />

- Many online tools exist for this.  
- Just paste the obfuscated code → Tool gives back original readable code.  

👉 This proves: Even if JS is obfuscated, we can often **reverse it**.



## 🔹 Comparison Table

| Feature        | Minification                  | Obfuscation                  |
|----------------|-------------------------------|-------------------------------|
| Goal           | Reduce file size (faster load) | Make code hard to understand |
| Readability    | Less readable but still ok     | Very hard to read (gibberish)|
| Example Use    | Production websites            | Protecting logic / preventing reverse-engineering |
| Execution      | Works same as original         | Works same as original        |

---
# Best Practices for JavaScript in Websites 

If you are developing a website or web application, you will use JavaScript (JS).  
But careless coding can make your site vulnerable to attacks.  
Follow these best practices to reduce risk:



## 1) Don’t Rely Only on Client-Side Validation ❌

- JS is often used for **form validation** (checking user input before sending to server).  
- But **bad users can disable JS** or **manipulate it**.  
- If you only validate in JS, attackers can bypass rules.

👉 Best Practice:  
- Do **both client-side + server-side validation**.  
- Client-side = better user experience  
- Server-side = stronger security



## 2) Don’t Add Untrusted Libraries ❌

- You can include JS libraries using `<script src="..."></script>`.  
- But if the library is from a **bad source**, it can contain malicious code.  
- Hackers upload fake libraries with names similar to real ones.  

👉 Best Practice:  
- Only use libraries from **trusted sources (CDN, official repo, etc.)**.  
- Double-check the library name before adding it.  



## 3) Avoid Hardcoding Secrets ❌

- Never put sensitive info (like API keys, access tokens, passwords) inside JS.  
- Why? Because **anyone can view your JS file** in browser → steal secrets.

⚠️ Example of Bad Practice:
```
const privateAPIKey = 'pk_TryHackMe-1337';
```

👉 Best Practice:  
- Store secrets on the **server-side** only.  
- Use environment variables or secure storage.  



## 4) Minify and Obfuscate Your Code ✅

- **Minification** → makes file smaller + faster to load  
- **Obfuscation** → makes code harder to read (adds protection)  

👉 Best Practice:  
- Always minify + obfuscate JS before deploying to production.  
- This doesn’t make code 100% safe, but slows attackers down.  



## 🔹 Summary Table

| Bad Practice ❌                         | Best Practice ✅                          |
|-----------------------------------------|-------------------------------------------|
| Only client-side validation             | Validate both client + server side         |
| Adding random libraries                 | Use trusted, official libraries only       |
| Hardcoding secrets in JS                | Keep secrets on server, not in JS          |
| Writing raw readable JS in production   | Minify + Obfuscate before deployment       |



## 🔹 Key Takeaway
JS is powerful, but careless use can expose your site to attacks.  
👉 Always combine **security + performance practices** for safe web development.


## 🔹 Key Takeaway
- **Minification** = for speed and performance 🚀  
- **Obfuscation** = for hiding and protection 🕵️  
- **Deobfuscation** = possible, so obfuscation is not 100% secure ⚠️

