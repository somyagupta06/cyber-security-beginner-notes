# 🔐 Natas Level: Natas7--->Natas8



### 📂 Execution Process 

👉 
- click on view SourceCode
<img width="1470" height="956" alt="Screenshot 2025-09-29 at 2 46 15 PM" src="https://github.com/user-attachments/assets/f47aff51-6303-4f2a-af7c-db9ad1c304a6" />
- get the secret code
<img width="1470" height="956" alt="Screenshot 2025-09-29 at 2 46 42 PM" src="https://github.com/user-attachments/assets/fdf45398-fdc3-4379-ba26-f293aaf65949" />
- open cyberchef
- from hex to text and reverse string function

<img width="1470" height="956" alt="Screenshot 2025-09-29 at 2 49 59 PM" src="https://github.com/user-attachments/assets/3c6b1324-fd3b-4552-ade2-312d78c22ceb" />
- from base64

<img width="1470" height="956" alt="Screenshot 2025-09-29 at 2 51 06 PM" src="https://github.com/user-attachments/assets/934be21e-fea6-48f9-8b48-df6d8bd0e583" />


  
### 📄 Password found:
👉 
ZE1ck82lmdGIoErlhQgWND6j2Wzz6b6t

### NOTES

## NATAS8 - Easy Notes (very simple English)

- Goal: Explain what the PHP code does in very very easy English so anyone can understand.

1) Short idea (one line)
The program checks a secret sent by a web form. It encodes the secret in a special way and compares the result with a saved encoded value. If they match, it says "Access granted".

2) Important stored value
- Saved encoded value (on server):
3d3d516343746d4d6d6c315669563362
- This is the encoded form of the correct secret.

3) What the encodeSecret function does (very simple)
Function name: encodeSecret(secret)
Steps (in order):
  - base64_encode(secret)
    -> Convert the secret into Base64 text.
  - strrev(the Base64 text)
    -> Reverse the Base64 text (read it from end to start).
  - bin2hex(the reversed text)
    -> Convert reversed text into hexadecimal characters (0-9, a-f).

So final result = hexadecimal representation of reversed(Base64(secret)).

4) When does the check happen?
When the web form is submitted (POST data contains "submit"), the program takes the input named "secret" and runs encodeSecret on it. Then it compares the result with the saved encoded value. If equal -> success, else -> wrong.

5) Reverse the process (how to find the real secret)
- We know the saved encoded hex. To find the original secret you do the reverse steps:
  a) Convert hex back to raw bytes (hex to bytes).
  b) Reverse that raw text (undo the strrev step).
  c) Base64-decode that reversed text to get the original secret.

- Example commands to do this (write these exact lines in a terminal):
 ```
 echo -n "3d3d516343746d4d6d6c315669563362" | xxd -r -p | rev | base64 -d
```
(If you run the command you will get the original secret printed.)

6) The original secret (what you should send in the web form)
oubWYf2kBq

- If you send secret = "oubWYf2kBq" in the form, encodeSecret(secret) will equal the saved value and the page will print:
-  Access granted. The password for natas9 is <censored>

7) Quick mapping table (for learners)
| Step in code | Meaning in plain words |
|--------------|------------------------|
| base64_encode | Turn the secret into Base64 text (letters + numbers + + / =) |
| strrev | Reverse the order of characters in the Base64 text |
| bin2hex | Change each byte into two hexadecimal characters |

8) Short example with a small secret (toy example)
- Take secret = "hi"
 - base64_encode("hi") -> aGk=
 - strrev("aGk=") -> =kGa
 - bin2hex("=kGa") -> 3d6b4761  (this is the hex result)
- So encodeSecret("hi") would be 3d6b4761 in hex. The actual program uses the same steps but with the saved hex shown above.

9) Important notes / safety
- This code only compares values and prints a message. It does not print the real natas9 password here because it was censored.
- Do not share real credentials in public places.

10) If you want to test in PHP (small script)
Place this in a PHP file to test locally (example):
```
<?php
function encodeSecret($secret) {
  return bin2hex(strrev(base64_encode($secret)));
}
$test = "oubWYf2kBq";
echo encodeSecret($test) . "\n";
?>
```
Running the PHP script will print the same saved hex if the test secret is correct.

11) Final simple summary
- The server saved a hex string.
- The site encodes what you send with base64, reverse, then hex.
- If your encoded value equals the saved hex, you win.
- The correct plain secret is: oubWYf2kBq


