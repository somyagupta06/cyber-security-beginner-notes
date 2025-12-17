# EVEN RSA CAN BE BROKEN???

## This service provides you an encrypted flag. Can you decrypt it with just N & e?

<img width="1435" height="154" alt="Screenshot 2025-12-17 at 11 08 24 PM" src="https://github.com/user-attachments/assets/7285faa9-04a5-41fa-9aa8-82168bfee797" />

- here n is even but as we know n = p*q where p and q must be very large prime numbers but as n is even so p = 2 so it is a very weak RSA key

- we can decrypt using a python program
  
``` bash
 Values provided
n = 17563001564772303236782651137209017723489881862193673792412384036306164494671540423269071302624425815183076768416402773630045879359985584813266634937632986
e = 65537
ciphertext = 17144301275327261671526026726171538924059930461973660477466706786206223734051596601997165332505683256101979866288866588752656151352578813127973925487237341

# Factors found from your FactorDB screenshot
p = 2
q = n // p

# 1. Calculate phi(n)
phi = (p - 1) * (q - 1)

# 2. Calculate the private key d 
# Python 3.8+ can do this built-in: pow(base, -1, mod)
d = pow(e, -1, phi)

# 3. Decrypt the message: M = C^d mod n
m = pow(ciphertext, d, n)

# 4. Convert the number M to text
try:
    # Convert to hex, remove '0x', and ensure even length
    hex_m = hex(m)[2:]
    if len(hex_m) % 2 != 0: hex_m = '0' + hex_m
    
    # Convert hex to bytes then decode to ASCII
    plaintext = bytes.fromhex(hex_m).decode('ascii')
    print(f"Decrypted Message: {plaintext}")
except Exception as err:
    print(f"Decrypted Integer: {m}")
    print("Could not convert to ASCII. It might be encoded differently.")
```
- what this python script does
  1. Factors nikalna ($p$ aur $q$)
     RSA ki puri security is baat par tiki hoti hai ki $n$ ko do primes mein todna (factorize karna) bahut mushkil hai. Lekin tumhare case mein $n$ even number tha, isliye FactorDB ne turant bata diya:$p = 2$$q = n // 2$Jab humein $p$ aur $q$ mil gaye, samajho lock aadha khul gaya.
  2. $\phi(n)$ Calculate karna (The Secret Number)
     Ye ek mathematical formula hai jo humein batata hai ki $n$ ke "workspace" mein kitne numbers hain jo usse coprime hain. Iska simple formula hota hai:$$\phi(n) = (p - 1) \times (q - 1)$$Ye number public key ($e$) aur private key ($d$) ke beech ka bridge (pul) hai.
  3. Private Key ($d$) dhundna
     Ab humein wo "secret key" $d$ chahiye jo ciphertext ko wapas normal text bana sake.Script mein pow(e, -1, phi) ka matlab hai: "Aisa number $d$ dhoondo jise $e$ se multiply karne par aur $\phi$ se divide karne par remainder $1$ bache."Yehi $d$ hamari asli chaabi hai.
  4. Decryption aur Text Conversion
     Ab hum final formula lagate hain:$$\text{Message} = \text{Ciphertext}^d \pmod n$$Isse humein ek bahut bada Number milta hai. Par message toh words mein hona chahiye! Isliye hum:Us number ko Hexadecimal (computer language) mein badalte hain.Phir us Hex ko ASCII characters (jaise A, B, C, @) mein convert karte hain.
 
