# 🔐 Bandit Level: Bandit17 ➝ Bandit18
## 📂 Command(s) used:
👉 
- ls
- cat
- diff passwords.new passwords.old
<img width="371" height="77" alt="Screenshot 2025-08-21 at 9 40 00 PM" src="https://github.com/user-attachments/assets/eff58cbf-34b9-45fa-87ad-7bd133986131" />

## 📄 Password found:
👉 
x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO

## 🧠 Notes / What I learned:
👉 
## Commands Used (explain) 
**diff passwords.new passwords.old**
- Output:
``` bash
42c42
< x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO
---
> gvE89l3AhAhg3Mi9G2990zGnn42c8v20
```
- **Meaning:** Line 42 is different in both files:
- < = content from passwords.new
- > = content from passwords.old
---
# 📘 diff Command in Linux

The `diff` command is used to show the **differences between two files or directories**.  
It’s mainly used for **file comparison**.

---

## 🔹 Basic Syntax
diff [options] file1 file2

---

## 🔹 Example 1: Compare Two Files
File1.txt:
``` bash
Hello World
This is File1
```
File2.txt:
``` bash
Hello World
This is File2
```
Command:
``` bash
diff file1.txt file2.txt
```
Output:
``` bash
2c2
< This is File1
---
> This is File2
```
👉 **Meaning:** Line 2 is different in both files:
- < = content from file1
- > = content from file2
## 🔹 Example 2: Ignore Case Differences
``` bash
diff -i file1.txt file2.txt
```
👉 Ignores capitalization differences (Hello and hello are treated the same).

## 🔹 Example 3: Side-by-Side Comparison
``` bash
diff -y file1.txt file2.txt
```
👉 Shows both files compared side by side.

## 🔹 Example 4: Compare Directories
``` bash
diff dir1 dir2
```
👉 Compares the contents of both directories.

## 🔹 Example 5: Unified Format (Patch Style)
``` bash
diff -u file1.txt file2.txt
``` 
- Output (like Git patches):
``` bash
--- file1.txt   2025-08-21
+++ file2.txt   2025-08-21
@@ -1,2 +1,2 @@
 Hello World
-This is File1
+This is File2
```
👉 This format is commonly used by programmers and Git to show code changes.
## 🔹 Quick Summary
- < = line only exists in file1
- > = line only exists in file2
- c = changed
- a = added
- d = deleted
