# 🔐 Bandit Level: Bandit9 ➝ Bandit10
## 📂 Command(s) used:
👉 strings data.txt

Or 

  Strings data.txt | grep "==..." 

## 📄 Password found:
👉 FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey

## 🧠 Notes / What I learned:
👉  

##** STRINGS **


Strings is a simple but powerful Linux command that extracts readable text (ASCII characters) from inside a binary or non-text file.
Imagine you have a file filled with random binary data, but it contains some hidden words, passwords, or messages — strings will pull out those readable parts.
**Basic syntax**
```
strings <filename>
```
**Useful options**
- `-n <number>`
Show only words with a minimum length (default is 4).

Example:
```
strings -n 8 file.bin
```
→ shows only words of length 8 or greater.
- `-f`
Also print the filename in the output (useful when scanning multiple files).
- `-t x`
Show the offset (position) of the found string in hexadecimal.
- `-e`
Select encoding (for example, use -e l for UTF-16 little-endian).
