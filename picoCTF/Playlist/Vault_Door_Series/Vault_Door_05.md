# vault-door-5

## PROBLEM STATEMENT 
In the last challenge, you mastered octal (base 8), decimal (base 10), and hexadecimal (base 16) numbers, but this vault door uses a different change of base as well as URL encoding! The source code for this vault is here: VaultDoor5.java



## Step-by-Step Solution:
- this is the string from which we have to compare our password
<img width="1470" height="956" alt="Screenshot 2025-09-29 at 9 01 23 PM" src="https://github.com/user-attachments/assets/6262c962-3442-4ef0-ab3f-567c9665b464" />
- use base64 and url decoder to decode the actual string from which we have to compare
<img width="1470" height="956" alt="Screenshot 2025-09-29 at 9 02 23 PM" src="https://github.com/user-attachments/assets/40ee18ca-911a-48f8-9b11-29c2abbec0d0" />

## NOTES



## 1. What the program does
- It asks you to enter a password in the form `picoCTF{something_here}`.
- It removes the `picoCTF{` and `}` parts.
- It then checks the middle part (the real password) with a hidden check.

If your password matches the hidden check → "Access granted"  
If not → "Access denied"

---

## 2. How the check works (Step by Step)

### Step 1: URL Encode
- Every character of your password is turned into `%xx` form, where `xx` is the hex code of that character.

Example Table:

| Character | Hex Code | URL encoded form |
|-----------|----------|------------------|
| c         | 63       | %63              |
| 0         | 30       | %30              |
| n         | 6e       | %6e              |

So `"c0n"` becomes `%63%30%6e`.

### Step 2: Base64 Encode
- The string `%63%30%6e` is then Base64 encoded.
- Base64 changes a string into a special text form.
- Example: `"Hello"` becomes `"SGVsbG8="` in Base64.

### Step 3: Compare
- The program has a long Base64 string stored in `expected`.
- It Base64 encodes your URL encoded password and checks:
  - If it matches `expected` → correct password
  - If not → wrong password

---

## 3. How to reverse (to get the real password)

Because program does:
password → URL Encode → Base64 Encode → compare

We do the reverse:
expected string → Base64 Decode → URL Decode → real password

### Step A: Base64 Decode the `expected` string
- This gives you a string like `%63%30%6e%76...`

### Step B: URL Decode (percent decode) that string
- This converts each `%xx` back to its real character.

Result = real password (the text that goes inside `picoCTF{}`)

---

## 4. The actual decoded password

After reversing the process we get:

Password: c0nv3rt1ng_fr0m_ba5e_64_e3152bf4

And the full flag is:

picoCTF{c0nv3rt1ng_fr0m_ba5e_64_e3152bf4}

---

## 5. Commands in very easy English

Step 1: Copy the long `expected` Base64 string from the Java code.

Step 2: Use any Base64 decode tool online. Paste the string.  
It will show something like `%63%30%6e%76...`

Step 3: Use any URL decode tool online. Paste that result.  
It will show the real password text.

Step 4: Put the password inside `picoCTF{}`.  
This is your flag.

---

## 6. Small Recap Table

| Program action | What you do to reverse |
|----------------|------------------------|
| URL Encode     | URL Decode             |
| Base64 Encode  | Base64 Decode          |

Program: password → URL Encode → Base64 Encode  
You: expected → Base64 Decode → URL Decode

---

## 7. Final Flag

picoCTF{c0nv3rt1ng_fr0m_ba5e_64_e3152bf4}

This is the answer you can submit.
