# 🔐 Bandit Level: Bandit10 ➝ Bandit11
## 📂 Command(s) used:
👉 echo VGhlIHBhc3N3b3JkIGlzIGR0UjE3M2ZaS2IwUlJzREZTR3NnMlJXbnBOVmozcVJyCg== | base64 --decode

## 📄 Password found:
👉 dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr

## 🧠 Notes / What I learned:
👉
## Base64 Short Notes



### 1. Definition
Base64 is an encoding scheme that converts binary data into text format.

It uses only 64 printable ASCII characters:
- A–Z (26), a–z (26), 0–9 (10), +, / → total 64 characters.
- = is used for padding.

**Role of Padding (=):**
Base64 always makes the output length a multiple of 4.
- If input is not a multiple of 3 bytes:
   - 1 extra byte → 2 = paddings
   - 2 extra bytes → 1 = padding
     
**📌 Examples:**
- "Ma" (2 bytes) → "TWE="
- "M" (1 byte) → "TQ=="
### 2. Purpose
Base64 is used to safely transmit binary data (images, files, etc.) over text-based systems such as email, JSON, XML, HTTP, etc.

**📌 Example:**

Embedding an image in HTML:
```
<img src="data:image/png;base64,...">
```
### 3. Working Principle
- Split binary data into 8-bit groups.
- Re-divide them into 6-bit groups (since 2⁶ = 64).
- Match each 6-bit group with the Base64 character set.
- If data is not a multiple of 3 bytes, use = padding.
### 4. Example
- Text: "Hi"
- ASCII → Binary:
  - H → 01001000
  - i → 01101001
- Split into 6-bit groups → Match with Base64 table → "SGk="
### 5. Common Uses
- Email attachments (MIME encoding)
- Embedding binary data in HTML/CSS
- API authentication (Basic Auth: username:password → Base64)
- Storing binary data in text-based databases
### 6. Commands & Tools
Linux CLI:
```
echo -n "Hello" | base64          # Encode
echo -n "SGVsbG8=" | base64 --decode   # Decode
```
Python:
```
import base64
base64.b64encode(b"Hello")       # Encode
base64.b64decode(b"SGVsbG8=")    # Decode
```
### 7. Important Notes
- Base64 is not encryption, just encoding → it doesn’t hide data, only changes its format.
- The encoded output is about 33% larger than the original data.
- Padding (=) ensures that the encoded string length is always a multiple of 4.
 
