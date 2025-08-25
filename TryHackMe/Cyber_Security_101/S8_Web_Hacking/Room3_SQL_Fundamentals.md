# DATABASES — EASY NOTES

## 1. WHAT IS A DATABASE?
- A database is like a digital cupboard where data is stored in an organized way.
- You can easily **add, find, edit, and analyze** information in it.
- Examples:
  - Login data: usernames & passwords
  - Social media: posts, comments, likes
  - Netflix: watch history & recommendations
  - Shopping apps: orders & payments



## 2. TYPES OF DATABASES

### a) Relational Database (SQL)
- Data stored in **tables** (rows + columns).
- Fixed structure → called a **schema**.
- Relationships between tables are possible.
- Best when data format is consistent.
- Example:
Table: Users
+----+----------+---------------+
| id | username | email |
+----+----------+---------------+
| 1 | JohnDoe | john@mail.com |
| 2 | AliceW | alice@mail.com|
+----+----------+---------------+

### b) Non-Relational Database (NoSQL)
- Data stored in **flexible format** (documents, key-value, graphs).
- Each record doesn’t have to follow the same structure.
- Best when data is varied.
- Example (document in MongoDB):
{
_id: "4556712cd2b2397ce1b47661",
name: { first: "Thomas", last: "Anderson" },
date_of_birth: "1964-09-02",
occupation: ["The One"],
steps_taken: 4738947387743977493
}



## 3. SQL vs NoSQL — Quick Comparison

| Feature             | SQL (Relational DB)                           | NoSQL (Non-Relational DB)                 |
|---------------------|-----------------------------------------------|------------------------------------------|
| Structure           | Tables (rows & columns)                      | Flexible (documents, key-value, graphs)   |
| Schema              | Fixed (predefined)                           | Dynamic (can change anytime)              |
| Relationships       | Supported (joins, foreign keys)              | Not built-in (usually app handles it)     |
| Scaling             | Vertical (bigger server)                     | Horizontal (many servers)                 |
| Best For            | Transactions, accuracy, consistent data       | Large, varied, unstructured data          |
| Examples            | MySQL, PostgreSQL, Oracle, SQL Server        | MongoDB, Cassandra, CouchDB, Redis        |

---

## 4. TABLES, ROWS, COLUMNS (SQL ONLY)
<img width="807" height="438" alt="Screenshot 2025-08-25 at 11 06 19 AM" src="https://github.com/user-attachments/assets/270b053a-ecd1-446c-b13b-46cceb7c9278" />

- **Table** = like an Excel sheet.
- **Columns** = categories (id, name, date).
- **Rows** = individual records.
- Example:
Table: Books
+----+--------------------------+--------------+
| id | Name | Published_on |
+----+--------------------------+--------------+
| 1 | Android Security Intern. | 2014-10-14 |
| 2 | Learn SQL Basics | 2020-05-10 |
+----+--------------------------+--------------+



## 5. PRIMARY KEY vs FOREIGN KEY
<img width="1251" height="407" alt="Screenshot 2025-08-25 at 11 06 58 AM" src="https://github.com/user-attachments/assets/f02d27be-d182-4ce4-a6dc-49e5059cddf3" />

### Primary Key
- A unique column that **identifies each row**.
- Example: Student roll number, Book ID.
- Only **one** primary key per table.
Table: Authors
+----+-----------+
| id | name |
+----+-----------+
| 1 | John Wick |
| 2 | Neo |
+----+-----------+

### Foreign Key
- A column that **links to a primary key** in another table.
- Creates a relationship between tables.
- Example:
Table: Books
+----+----------------------+-----------+
| id | Name | author_id |
+----+----------------------+-----------+
| 1 | Android Internals | 1 |
| 2 | Matrix Explained | 2 |
+----+----------------------+-----------+
Here:

- author_id in Books → refers to id in Authors
- This connects books with their authors
---
# SQL (Structured Query Language) — EASY NOTES

## 1. What is SQL?
- SQL = Structured Query Language
- It is a **language to talk with relational databases**.
- You can use SQL to:
  - **Query** data (search/filter records)
  - **Insert** new data
  - **Update** existing data
  - **Delete** data
  - **Define** database structure (create tables, columns, relationships)



## 2. DBMS (Database Management System)
- A DBMS is software that manages databases.
- It acts as an **interface between user & database**.
- Popular DBMS examples:
  - MySQL
  - MariaDB
  - Oracle Database
  - PostgreSQL
  - MongoDB (for NoSQL)



## 3. Benefits of SQL & Relational Databases
- **Fast** → SQL databases return data very quickly.
- **Easy to Learn** → commands look like English.
- **Reliable** → strict structure keeps data accurate.
- **Flexible** → powerful queries & analysis possible.



## 4. SQL is Easy to Read
- Example query:
``` bash
SELECT name, email FROM Users WHERE id = 1;
```
Reads like English:
→ "Select name and email from Users where id equals 1."



## 5. Getting Hands-On with MySQL

### Step 1: Open MySQL
``` bash
user@tryhackme$ mysql -u root -p
```
- `-u root` → login as root user
- `-p` → prompt for password

### Step 2: Enter Password
``` bash
user@tryhackme$ tryhackme
```
### Step 3: Expected Output
``` bash
Welcome to the MySQL monitor. Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.0.39-0ubuntu0.20.04.1 (Ubuntu)
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```
---
# SQL DATABASE & TABLE STATEMENTS 

## 1. DATABASE STATEMENTS

### CREATE DATABASE
- Used to create a new database.
- Syntax:
``` bash
CREATE DATABASE database_name;
```
- Example:
``` bash
CREATE DATABASE thm_bookmarket_db;
```


### SHOW DATABASES
- Used to list all databases available in MySQL.
- Syntax:
``` bash
SHOW DATABASES;
```
- Output will include:
- Your created DB → `thm_bookmarket_db`
- Default system DBs → `mysql`, `information_schema`, `performance_schema`, `sys`



### USE DATABASE
- Tells MySQL which database you want to work with.
- Syntax:
``` bash
USE database_name;
```
- Example:
``` bash
USE thm_bookmarket_db;
```


### DROP DATABASE
- Deletes a database permanently.
- Syntax:
``` bash
DROP DATABASE database_name;
```
⚠️ Be careful: this removes **everything** inside the database.



## 2. TABLE STATEMENTS

### CREATE TABLE
- Creates a new table inside the current active database.
- Syntax:
``` bash
CREATE TABLE table_name (
column1 data_type,
column2 data_type,
column3 data_type
);
```
- Example:
``` bash
CREATE TABLE book_inventory (
book_id INT AUTO_INCREMENT PRIMARY KEY,
book_name VARCHAR(255) NOT NULL,
publication_date DATE
);
```
Explanation:
- `book_id INT AUTO_INCREMENT PRIMARY KEY`
- INT → number only
- AUTO_INCREMENT → automatically increases (1, 2, 3, …)
- PRIMARY KEY → unique identifier for each row
- `book_name VARCHAR(255) NOT NULL`
- Text up to 255 characters
- NOT NULL → cannot be empty
- `publication_date DATE`
- Stores a date (YYYY-MM-DD)



### SHOW TABLES
- Lists all tables inside the current active database.
- Syntax:
``` bash
SHOW TABLES;
```


### DESCRIBE (DESC)
- Shows details of a table → columns, data types, keys, default values.
- Syntax:
``` bash
DESCRIBE table_name;
```
- Example:
``` bash
DESCRIBE book_inventory;
```
Output Example:
``` bash
+------------------+--------------+------+-----+---------+----------------+
| Field | Type | Null | Key | Default | Extra |
+------------------+--------------+------+-----+---------+----------------+
| book_id | int | NO | PRI | NULL | auto_increment |
| book_name | varchar(255) | NO | | NULL | |
| publication_date | date | YES | | NULL | |
+------------------+--------------+------+-----+---------+----------------+
```


### ALTER TABLE
- Used to modify an existing table (add, delete, or change columns).
- Syntax (Add Column):
``` bash
ALTER TABLE table_name
ADD column_name data_type;
```
- Example (adding page_count column):
``` bash
ALTER TABLE book_inventory
ADD page_count INT;
```


### DROP TABLE
- Deletes a table permanently.
- Syntax:
``` bash
DROP TABLE table_name;
```


## QUICK RECAP

1. **Create a DB**
``` bash
CREATE DATABASE thm_bookmarket_db;
```
2. **Use DB**
``` bash
USE thm_bookmarket_db;
```
3. **Create Table**
``` bash
CREATE TABLE book_inventory (
book_id INT AUTO_INCREMENT PRIMARY KEY,
book_name VARCHAR(255) NOT NULL,
publication_date DATE
);
```
4. **List Tables**
``` bash
SHOW TABLES;
```
5. **Check Table Structure**
``` bash
DESCRIBE book_inventory;
```
6. **Modify Table**
``` bash
ALTER TABLE book_inventory ADD page_count INT;
```
7. **Delete Table**
``` bash
DROP TABLE book_inventory;
```
---
# SQL CRUD OPERATIONS — EASY NOTES

CRUD = **Create, Read, Update, Delete**  
👉 These are the four basic operations for managing data inside a database.

---

## 1. CREATE → (INSERT)

- Used to **add new records** into a table.
- Syntax:
``` bash
INSERT INTO table_name (col1, col2, col3)
VALUES (value1, value2, value3);
```
- Example:
``` bash
INSERT INTO books (id, name, published_date, description)
VALUES (1, "Android Security Internals", "2014-10-14",
"An In-Depth Guide to Android's Security Architecture");
```
✅ Adds a new row into the `books` table.



## 2. READ → (SELECT)

- Used to **fetch records** from a table.
- Syntax (all columns):
``` bash
SELECT * FROM table_name;
```
- Example:
``` bash
SELECT * FROM books;
```
Output:
``` bash
+----+----------------------------+----------------+------------------------------------------------------+
| id | name | published_date | description |
+----+----------------------------+----------------+------------------------------------------------------+
| 1 | Android Security Internals | 2014-10-14 | An In-Depth Guide to Android's Security Architecture |
+----+----------------------------+----------------+------------------------------------------------------+
```
- Syntax (specific columns):
``` bash
SELECT name, description FROM books;
```
Output:
``` bash
+----------------------------+------------------------------------------------------+
| name | description |
+----------------------------+------------------------------------------------------+
| Android Security Internals | An In-Depth Guide to Android's Security Architecture |
+----------------------------+------------------------------------------------------+
```


## 3. UPDATE → (UPDATE)

- Used to **modify existing records**.
- Syntax:
``` bash
UPDATE table_name
SET column_name = new_value
WHERE condition;
```
- Example:
``` bash
UPDATE books
SET description = "An In-Depth Guide to Android's Security Architecture."
WHERE id = 1;
```
✅ Updates the `description` for the book with `id = 1`.



## 4. DELETE → (DELETE)

- Used to **remove records** from a table.
- Syntax:
``` bash
DELETE FROM table_name WHERE condition;
```
- Example:
``` bash
DELETE FROM books WHERE id = 1;
```
✅ Deletes the row where `id = 1`.

⚠️ WARNING: If you forget the `WHERE` clause → it deletes **all rows** from the table.



## 5. QUICK SUMMARY (CRUD in SQL)

- **Create** → `INSERT INTO` → Add new record
- **Read** → `SELECT` → Retrieve data
- **Update** → `UPDATE ... SET ... WHERE` → Change existing record
- **Delete** → `DELETE FROM ... WHERE` → Remove record
---
# SQL CLAUSES 

👉 A clause is a part of an SQL statement that **specifies criteria or conditions**  
   (decides *what data*, *how it should look*, *in what order*, etc.).

We already know:
- **FROM** → chooses the table
- **WHERE** → filters rows before processing

Now let’s learn other useful clauses:



## 1. DISTINCT
- Removes **duplicate rows** from results.
- Syntax:
``` bash
SELECT DISTINCT column_name FROM table_name;
```
- Example:
``` bash
SELECT DISTINCT name FROM books;
```
✅ Returns each book name only once, even if duplicates exist.



## 2. GROUP BY
- Groups rows that have the same value in a column.
- Often used with **aggregate functions** (COUNT, SUM, AVG, MAX, MIN).
- Syntax:
``` bash
SELECT column, aggregate_function(*)
FROM table
GROUP BY column;
```
- Example:
``` bash
SELECT name, COUNT(*)
FROM books
GROUP BY name;

```
✅ Shows each book name with how many times it appears.



## 3. ORDER BY
- Sorts results in **ascending (ASC)** or **descending (DESC)** order.
- Default = ASC (ascending).
- Syntax:
``` bash
SELECT * FROM table
ORDER BY column ASC; -- ascending
ORDER BY column DESC; -- descending
```
- Example:
``` bash
SELECT * FROM books ORDER BY published_date ASC;
```
✅ Oldest book first.
``` bash
SELECT * FROM books ORDER BY published_date DESC;
```
✅ Latest book first.



## 4. HAVING
- Similar to `WHERE` but used for **aggregated results** (with GROUP BY).
- Filters groups after aggregation is done.
- Syntax:
``` bash
SELECT column, COUNT(*)
FROM table
GROUP BY column
HAVING condition;
```
- Example:
``` bash
SELECT name, COUNT(*)
FROM books
GROUP BY name
HAVING name LIKE '%Hack%';
```
✅ Shows only grouped results where book name contains “Hack”.



## QUICK RECAP
- **DISTINCT** → remove duplicates  
- **GROUP BY** → group rows (often with COUNT/SUM/etc.)  
- **ORDER BY** → sort results (ASC/DESC)  
- **HAVING** → filter after GROUP BY  
---
# 📘 SQL Operators Notes (Easy Explanation)

## 1. LIKE Operator
- `LIKE` ka use tab hota hai jab hume kisi column ke andar **pattern ya word search** karna ho.  
- `%` ka matlab hota hai **kuch bhi ho sakta hai** (wildcard).  

```sql
SELECT *
FROM books
WHERE description LIKE "%guide%";
```

👉 Ye query un books ko dikhayegi jinke **description** me "guide" word aata hai.  

---

## 2. AND Operator
- `AND` tab use hota hai jab **dono conditions sahi** honi chahiye.  

```sql
SELECT *
FROM books
WHERE category = "Offensive Security" AND name = "Bug Bounty Bootcamp";
```

👉 Ye query sirf wahi book dikhayegi jo **Offensive Security** category me hai **aur** jiska naam "Bug Bounty Bootcamp" hai.  

---

## 3. OR Operator
- `OR` tab use hota hai jab **kam se kam ek condition sahi** ho.  

```sql
SELECT *
FROM books
WHERE name LIKE "%Android%" OR name LIKE "%iOS%";
```

👉 Ye query un books ko dikhayegi jinke naam me **Android** ya **iOS** aata hai.  

---

## 4. NOT Operator
- `NOT` ka use kisi condition ko **exclude** karne ke liye hota hai.  

```sql
SELECT *
FROM books
WHERE NOT description LIKE "%guide%";
```

👉 Ye query un books ko dikhayegi jinke description me **"guide" word** nahi aata hai.  

---

## 5. BETWEEN Operator
- `BETWEEN` ka use tab hota hai jab hume kisi **range ke andar values** filter karni ho.  

```sql
SELECT *
FROM books
WHERE id BETWEEN 2 AND 4;
```

👉 Ye query un books ko dikhayegi jinki `id` **2 se 4 ke beech** hai.  

---

## 6. Equal To (=) Operator
- `=` ka use tab hota hai jab hume **exact match** chahiye.  

```sql
SELECT *
FROM books
WHERE name = "Designing Secure Software";
```

👉 Ye query sirf wahi book dikhayegi jiska naam **exactly** "Designing Secure Software" hai.  

---

## 7. Not Equal To (!=) Operator
- `!=` ka use tab hota hai jab hume **match na hone wali values** chahiye.  

```sql
SELECT *
FROM books
WHERE category != "Offensive Security";
```

👉 Ye query un books ko dikhayegi jo **Offensive Security** category me **nahi** hai.  

---

## 8. Less Than (<) Operator
- `<` ka use tab hota hai jab hume **chhoti (smaller)** values chahiye.  

```sql
SELECT *
FROM books
WHERE published_date < "2020-01-01";
```

👉 Ye query un books ko dikhayegi jo **2020 se pehle publish** hui hain.  

---

## 9. Greater Than (>) Operator
- `>` ka use tab hota hai jab hume **badi (greater)** values chahiye.  

```sql
SELECT *
FROM books
WHERE published_date > "2020-01-01";
```

👉 Ye query un books ko dikhayegi jo **2020 ke baad publish** hui hain.  

---

## 10. Less Than or Equal To (<=) Operator
- `<=` ka use tab hota hai jab hume values **chhoti ya barabar** chahiye.  

```sql
SELECT *
FROM books
WHERE published_date <= "2021-11-15";
```

👉 Ye query un books ko dikhayegi jo **15 Nov 2021 se pehle ya us din publish** hui hain.  

---

## 11. Greater Than or Equal To (>=) Operator
- `>=` ka use tab hota hai jab hume values **badi ya barabar** chahiye.  

```sql
SELECT *
FROM books
WHERE published_date >= "2021-11-02";
```

👉 Ye query un books ko dikhayegi jo **2 Nov 2021 ke baad ya us din publish** hui hain.  
---
# 📘 SQL Functions Notes (Easy Explanation)

Functions help us to **manipulate data** aur queries ko **simplify** karte hain. Ye mainly do types ke hain:  
- **String Functions** → strings pe kaam karte hain (text manipulation).  
- **Aggregate Functions** → multiple rows ke values ko combine karke ek result dete hain.  

---

## 🧵 String Functions

### 1. CONCAT() Function
- `CONCAT()` ka use multiple strings ko **add (join)** karne ke liye hota hai.  
- Useful jab hume text ko combine karna ho.  

```sql
SELECT CONCAT(name, " is a type of ", category, " book.") AS book_info
FROM books;
```

👉 Ye query `name` aur `category` column ko mila ke ek naya column banati hai **book_info**.  



### 2. GROUP_CONCAT() Function
- `GROUP_CONCAT()` multiple rows ke values ko **ek hi string me merge** kar deta hai.  
- Mostly `GROUP BY` ke sath use hota hai.  

```sql
SELECT category, GROUP_CONCAT(name SEPARATOR ", ") AS books
FROM books
GROUP BY category;
```

👉 Ye query books ko **category ke hisaab se group** karegi aur unke names ko ek line me dikhayegi.  



### 3. SUBSTRING() Function
- `SUBSTRING()` ek string ka **chhota part (substring)** nikalne ke liye use hota hai.  
- Hum start position aur length define kar sakte hain.  

```sql
SELECT SUBSTRING(published_date, 1, 4) AS published_year
FROM books;
```

👉 Ye query `published_date` se sirf **year (first 4 characters)** nikal ke `published_year` me dikhayegi.  



### 4. LENGTH() Function
- `LENGTH()` string ke andar **kitne characters** (letters + spaces + punctuation) hain, uski length return karta hai.  

```sql
SELECT LENGTH(name) AS name_length
FROM books;
```

👉 Ye query `name` column ki **length** calculate karegi.  



## 📊 Aggregate Functions

### 1. COUNT() Function
- `COUNT()` total number of rows ya records batata hai.  

```sql
SELECT COUNT(*) AS total_books
FROM books;
```

👉 Ye query total books ki ginti karegi.  



### 2. SUM() Function
- `SUM()` ek numeric column ke **saare values add** kar deta hai.  

```sql
SELECT SUM(price) AS total_price
FROM books;
```

👉 Ye query sabhi books ke prices ka total nikal ke `total_price` me dikhayegi.  



### 3. MAX() Function
- `MAX()` kisi column ka **sabse bada (largest)** value return karta hai.  

```sql
SELECT MAX(published_date) AS latest_book
FROM books;
```

👉 Ye query sabse **latest published date** dikhayegi.  



### 4. MIN() Function
- `MIN()` kisi column ka **sabse chhota (smallest)** value return karta hai.  

```sql
SELECT MIN(published_date) AS earliest_book
FROM books;
```

👉 Ye query sabse **pehle published book date** dikhayegi.  
## Example Questions
Using the tools_db database, what is the tool with the longest name based on character length?
<img width="729" height="353" alt="Screenshot 2025-08-25 at 12 19 42 PM" src="https://github.com/user-attachments/assets/8956e7b2-634b-4003-b079-305393b6ae91" />
Using the tools_db database, what are the tool names where the amount does not end in 0, and group the tool names concatenated by " & ".
<img width="599" height="189" alt="Screenshot 2025-08-25 at 12 20 23 PM" src="https://github.com/user-attachments/assets/31faf73d-7568-4d98-8294-6d36539505d1" />
