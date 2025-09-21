# 🔐 Bandit Level: Bandit5 ➝ Bandit6
## 📂 Command(s) used:
👉 - find inhere/ -type f -size 1033c ! -executable -readable
   - cat inhere/maybehere07/.file2

## 📄 Password found:
👉 HWasnPhtq9AVKe0dmk45nxy20cvUa6EG

## 🧠 Notes / What I learned:
👉 ### question is asking.  
 -  human-readable
 -   1033 bytes in size
 -   not executable

## Commands Used
- **find inhere/**	Start searching in the inhere directory
- **-type f**	      Only look for files (not folders)
- **size 1033c**  	File must be exactly 1033 bytes (c = bytes)
- **! -executable**	File must not be executable
- **-readable**    	File must be readable by you


```
 Use find --help
```
  OR
```
    man find
```
  If you get stuck. 
----------------------------------------------------------

- find — find means “search for files or folders”

- SYNTAX-- find [where to find ] [what to find]


