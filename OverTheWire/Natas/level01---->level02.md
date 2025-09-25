# 🔐 Natas Level: Natas1--->Natas2



### 📂 Execution Process 

👉 

- Right click to inspect
- Open http://natas2.natas.labs.overthewire.org/files/pixel.png in new tab 
<img width="1470" height="956" alt="Screenshot 2025-09-25 at 9 27 48 PM" src="https://github.com/user-attachments/assets/7b385f21-58e7-47dd-9bd2-3b883f8b884f" />
- Now Open http://natas2.natas.labs.overthewire.org/files/
<img width="1470" height="956" alt="Screenshot 2025-09-25 at 9 28 46 PM" src="https://github.com/user-attachments/assets/9be6eaf8-dee4-4be0-8d10-087572094011" />
- Open user.txt
<img width="1470" height="956" alt="Screenshot 2025-09-25 at 9 29 43 PM" src="https://github.com/user-attachments/assets/f23dea1f-f547-473c-8a09-17ad90d8ff54" />


### 📄 Password found:
👉
3gqisGdR0pjm6tpkDKdIWO2hSvchLeYH

### Notes

View page source / Inspect element
**Action:** Right-click → "View Page Source" or open developer tools (F12) → Elements/Network.  
**Why:** Hidden hints are commonly embedded in HTML (images, links, comments). Viewing source shows the raw HTML so you can see file paths and references that are not obvious in rendered view.

**What to look for:** `<img src="...">`, `<a href="...">`, HTML comments, or scripts that reference files or directories.

---

## 3. Notice referenced resources (example: image path)
**Action:** If you see something like:
```html
<img src="files/pixel.png">
```
**Why:** This indicates a files/ directory is being used by the site. Attackers (or challenge designers) sometimes leave extra files in such directories. The presence of a path gives you a place to explore.


---

4. Try directory/resource enumeration

-Action: Open the directory path in the browser:
```
http://natas2.natas.labs.overthewire.org/files/
```
or use curl:
```
curl -I http://natas2.natas.labs.overthewire.org/files/
```
**Why:** Many basic challenges intentionally leave directory listings enabled or contain known files (like users.txt). Visiting the path directly may reveal a list of files or specific files that contain the password.


---

5. Look for text files or obvious files (e.g., users.txt)

- Action: If you see users.txt in the listing, open it:
```
http://natas2.natas.labs.overthewire.org/files/users.txt
```
or:
```
curl http://natas2.natas.labs.overthewire.org/files/users.txt
```
**Why:** Text files often contain credentials or hints. In Natas, the next level's password is frequently hidden in such files.


---



6. How this teaches fundamentals

- Inspecting source HTML teaches you to look for non-obvious references (images, scripts, hidden links).

- Directory enumeration demonstrates that exposed directories/files on a webserver are a common information leak.

- Using simple HTTP tools (curl, browser devtools) is enough for many low-level web challenges.
