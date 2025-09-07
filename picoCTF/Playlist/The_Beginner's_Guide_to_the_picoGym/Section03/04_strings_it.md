# STRINGS IT

## PROBLEM STATEMENT 
Can you find the flag in file without running it?

## Step-by-Step Solution:


- File provided: `strings` (ELF binary)  
- Hint from binary:

Maybe try the 'strings' function? Take a look at the man page

- Meaning: The binary contains **readable text or secret message** embedded, running it directly will not show the flag.

---

## 2️⃣ Commands Used

### a) Extract all readable strings from the binary

```bash
strings strings
```
- The strings command extracts all human-readable ASCII characters from a binary.
- Output may contain text, messages, or hidden flags.

### b) Filter for the flag
```
strings strings | grep pico
```

 <img width="608" height="163" alt="Screenshot 2025-09-07 at 12 43 00 PM" src="https://github.com/user-attachments/assets/a1bf0cf7-708e-4278-92f8-8b6a6e928878" />

- | (pipe) sends the output of the first command to the next command
 
- grep pico searches for lines containing "pico" (CTF flags usually start with picoCTF{...}).
### Output:
picoCTF{5tRIng5_1T_827aee91}
- ✅ This is the flag.
## 3️⃣ What Happened
- Running the ELF binary directly did not show the flag.
- strings extracted embedded readable text from the binary.
- grep filtered the relevant pattern → flag was revealed.
## 4️⃣ Key Linux Commands for This Type of CTF Challenge
- `ls `               |  # list files
- `file <file>`       |  # check file type
- `chmod +x <file> `   | # make file executable
- `./<file>`         |   # run executable
- `-strings <file>`   |   # extract readable ASCII strings
- `strings <file> | grep <pattern>` | # filter specific text
## 💡 Pro Tip
- Flags in CTF binaries are often hidden in strings or memory.
- strings command is the first go-to tool for binary analysis in beginner-level CTF challenges.

