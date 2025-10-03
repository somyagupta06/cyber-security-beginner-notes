# vault-door-6

## PROBLEM STATEMENT 
This vault uses an XOR encryption scheme. The source code for this vault is here: VaultDoor6.java


## Step-by-Step Solution:
# How the VaultDoor6 Password Works (Simple Explanation)

## 1. The Setup
- There is a Java program that asks for a vault password.
- The password must be written like this: `picoCTF{...}`
- The program removes `picoCTF{` and `}` to check only the middle part.

---

## 2. How Password is Checked
- The program has a secret array called `myBytes` with 32 numbers.
- It checks each character of your password using this formula:

(password_character XOR 0x55) == myBytes_value

- If all 32 characters match this rule, you get **Access granted**.

---

## 3. How to Find the Password
We can reverse the formula:

password_character = myBytes_value XOR 0x55

- That means for each number in `myBytes`, do XOR with 0x55.
- The result will be the real character for the password.

---

## 4. Example Step by Step
Let's do first two characters:

| Index | myBytes (hex) | XOR with 0x55 | Result ASCII Char |
|-------|---------------|---------------|-----------------|
| 0     | 0x3b          | 0x3b ^ 0x55 = 0x6e | n |
| 1     | 0x65          | 0x65 ^ 0x55 = 0x30 | 0 |

- So first two characters of password = `n0`

---

## 5. Full Password Extraction
- Do this XOR for all 32 bytes.
- Join all characters together.

Result:  
n0t_mUcH_h4rD3r_tH4n_x0r_3ce2919

- Finally, put it in `picoCTF{...}` format:

picoCTF{n0t_mUcH_h4rD3r_tH4n_x0r_3ce2919}

---

## 6. How to XOR Easily
1. **Manually (understandable)**  
   - Convert hex to binary  
   - XOR each bit with 0x55 (01010101)  
   - Convert result back to ASCII  

2. **Using Calculator**  
   - Windows / Mac / Phone → Programmer mode → Hex → XOR  

3. **Using Online Tool**  
   - Google "hex XOR calculator"  
   - Type `myByte` and `55` → Get result → Convert to ASCII

---

## 7. Quick Tip
- `0x55` = 01010101 → flips alternate bits
- That’s why XOR is used to hide password in `myBytes`
- Reverse XOR gives original password easily
