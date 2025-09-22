# keygenme-py

## PROBLEM STATEMENT 
(https://mercury.picoctf.net/static/9cc50abd5b012891d5a1132e05f15a07/keygenme-trial.py)

## Step-by-Step Solution:
Simple explanation — step by step (in plain English)

1) What the program wants
   - The program asks for a license key in this format:
     picoCTF{1n_7h3_|<3y_of_XXXXXXXX}
   - The XXXXXXXXX part (8 characters) is not random.
     It is taken from the SHA-256 hash of the username "SCHOFIELD".

2) Where the code shows a hash is used
   - In the program you can see lines like:
     hashlib.sha256(username_trial).hexdigest()[4]
     That means:
       * compute SHA-256 of the username
       * convert it to hexadecimal text (a long string of 0-9 and a-f)
       * pick one character from that text using an index (like [4])

3) Indexing: how characters are selected
   - Python string indexing starts at 0 (zero-based).
     Example:
       h = "abcd"
       h[0] = 'a'
       h[1] = 'b'
     So h[4] means "the fifth character of h".

4) The program uses these indexes (in this exact order):
   [4, 5, 3, 6, 2, 7, 1, 8]
   - That order is important — it decides which 8 characters form the license part.

5) Real example with the username "SCHOFIELD"
   - First compute SHA-256 hex of "SCHOFIELD":
     SHA256("SCHOFIELD") =
     66b8e54339dc8b7ff42eb6ea4d95561c67ad1192de710700b57c98e47c8af4b5

   - Now pick characters by index (0-based):
     index -> character
     0 -> 6
     1 -> 6
     2 -> b
     3 -> 8
     4 -> e
     5 -> 5
     6 -> 4
     7 -> 3
     8 -> 3
     ... (rest not needed)

   - Using the program's order [4,5,3,6,2,7,1,8]:
     h[4] = e
     h[5] = 5
     h[3] = 8
     h[6] = 4
     h[2] = b
     h[7] = 3
     h[1] = 6
     h[8] = 3

     Join them left-to-right: e 5 8 4 b 3 6 3
     Dynamic part = e584b363

6) Final license key
   picoCTF{1n_7h3_|<3y_of_e584b363}

7) How you can reproduce this on your own (Python code to copy-paste)
   import hashlib
   h = hashlib.sha256(b"SCHOFIELD").hexdigest()
   idx = [4,5,3,6,2,7,1,8]
   dynamic = ''.join(h[i] for i in idx)
   print("SHA256 hex:", h)
   print("dynamic part:", dynamic)
   print("license key: picoCTF{1n_7h3_|<3y_of_" + dynamic + "}")

8) Quick comparison to help remember
   - If program used [0,1,2,3,...] then dynamic would be first 8 chars.
   - But this program reorders indexes: it uses 4,5,3,... so you MUST follow the program's order.
   - Always read the exact index numbers in the code — they tell you which hash characters to pick.

9) Final note
   - The key is deterministic (always the same for "SCHOFIELD").
   - To unlock the program: enter the exact key printed above.
