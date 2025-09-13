# 🔐 Bandit Level: Bandit11 ➝ Bandit12
## 📂 Command(s) used:
👉 echo "Gur cnffjbeq vf 7k16JArUVv5LxVuJfsSVdbbtaHGlw9D4" | tr 'A-Za-z' 'N-ZA-Mn-za-m'

## 📄 Password found:
👉 7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4

## 🧠 Notes / What I learned:
👉 
## ROT13 – Notes

### 1️⃣ Definition

ROT13 is a special case of the Caesar Cipher.
- "ROT" = rotate
- "13" = shift by 13 letters
  - It shifts every letter of the alphabet forward by 13 positions.
  - It is a symmetric cipher → the same process is used for both encryption and decryption.
### 2️⃣ Working Principle
- The English alphabet has 26 letters.
- In ROT13:
  - Each letter is shifted 13 places forward.
  - If it reaches the end, it continues again from the start.
- The special thing about ROT13 is that encryption and decryption are the same process, because:
13 + 13 = 26 → a full alphabet rotation.
  
**📌 Encryption example:**
```
echo "HELLO" | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```
**📌 Decryption example:**
```
echo "URYYB" | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```
### 3️⃣ Command Breakdown
```
echo "URYYB"
```
- `echo` prints text to standard output.
- Here, "URYYB" is a ROT13 encrypted string.
- If you run only:
```
echo "URYYB"
```
Output will simply be:
```
URYYB
```
**`|` (pipe symbol)**
- A pipe takes the output of one command and passes it as the input to another command.
- Flow here:
```
"URYYB" → goes into → tr command
```
`tr 'A-Za-z' 'N-ZA-Mn-za-m'`
- tr = translate command — replaces characters from one set with characters from another.
a) 'A-Za-z' → Input set
- A-Z → uppercase A to Z
- a-z → lowercase a to z
- Together, this covers the entire alphabet.
b) 'N-ZA-Mn-za-m' → Output set
- N-ZA-M → uppercase letters N–Z followed by A–M (shifted by 13)
- n-za-m → lowercase letters n–z followed by a–m (shifted by 13)
### 7️⃣ Use Cases
- Hiding spoilers or puzzle answers on forums.
- Teaching basic cryptography concepts.
- Not secure → easily reversible without any key.
### 8️⃣ Key Points
- Fixed key size: 13 (no variation possible).
- Only works correctly for alphabetic characters.
- Symmetric property:
- ROT13(ROT13(text)) = original text

