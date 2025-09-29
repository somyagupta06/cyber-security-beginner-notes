# 🔐 Natas Level: Natas3--->Natas4



### 📂 Execution Process 

👉 
- click on view sourcecode
- this line contains the clue
<img width="1470" height="956" alt="Screenshot 2025-09-28 at 8 57 19 PM" src="https://github.com/user-attachments/assets/722dec79-dfb1-4fd6-8f44-74515a9a9b4f" />
include "includes/secret.inc";
- go to http://natas6.natas.labs.overthewire.org/includes/secret.inc
<img width="1470" height="956" alt="Screenshot 2025-09-28 at 8 58 04 PM" src="https://github.com/user-attachments/assets/c1da0935-b327-4bae-881b-6c97f0aa2983" />
- write the secret in the input area
<img width="1470" height="956" alt="Screenshot 2025-09-28 at 8 58 36 PM" src="https://github.com/user-attachments/assets/092d9562-8142-45b1-a4b0-3201332ee93d" />
- get the password
<img width="1470" height="956" alt="Screenshot 2025-09-28 at 8 59 12 PM" src="https://github.com/user-attachments/assets/4257c083-4ae7-474f-941c-0746ffb4ef7e" />

### 📄 Password found:
👉 
bmg8SvU1LizuWjx3y7xkNERkHxGre0GS

### NOTES
# PHP form + secret 



---

## 1 — What this small PHP app does (one-line)

It reads a secret from a file, takes what the user types in the form, compares them, and if they match it prints “Access granted”.

---

## 2 — Files involved (short)

* `index.php` — main script (checks form input).
* `includes/secret.inc` — file that stores the secret string (the value the site expects).
* `form.html` or same `index.php` can have HTML form to send user input.

---

## 3 — Example files (put these in your local folder)

`includes/secret.inc` (example)

```
<?php
$secret = "my-local-secret-123";
```

`index.php`

```
<?php
include "includes/secret.inc";

if (array_key_exists("submit", $_POST)) {
    if ($secret == $_POST['secret']) {
        print "Access granted. The password for natas7 is <censored>";
    } else {
        print "Wrong secret";
    }
}
?>

<form method="post">
  <input type="text" name="secret" placeholder="type secret here">
  <input type="submit" name="submit" value="Send">
</form>
```

Notes:

* Put `includes/secret.inc` in a folder named `includes`.
* The `index.php` shows a simple HTML form and does the check.

---

## 4 — Step-by-step flow (super simple)

1. User opens page -> sees form.
2. User types something into `secret` field and clicks Submit.
3. Server receives POST with keys `secret` and `submit`.
4. Script includes `secret.inc` so `$secret` variable is available.
5. Script compares `$secret` with `$_POST['secret']`.
6. If equal → prints success message; else → prints “Wrong secret”.

---

## 5 — Examples (input → output) — easy table

| Input typed in form   | Result shown                                          |
| --------------------- | ----------------------------------------------------- |
| `my-local-secret-123` | Access granted. The password for natas7 is <censored> |
| `hello`               | Wrong secret                                          |
| (empty)               | Wrong secret                                          |

---

## 6 — Why this is risky (in plain words)

* The secret is plain text in a file. If server is misconfigured, someone can read it.
* Using `==` (loose equality) can sometimes be risky in PHP (type juggling). Use strict check.
* If `includes/secret.inc` is inside webroot and the webserver serves `.inc` files as plain text, attacker could read it by visiting `includes/secret.inc`.
* Printing secrets in responses is bad in real apps.

I’m NOT giving steps to hack a real site. This is **for learning** how it works and how to secure it.

---

## 7 — How to practice safely on your own machine (local lab)

1. Make a new folder `php-lab`.
2. Put the example files above into that folder.
3. Run a local PHP server from that folder.

Command to run locally (type this in your terminal inside the folder)
php -S localhost:8000

4. Open browser and go to
   [http://localhost:8000](http://localhost:8000)

Now you can type the secret in the form and test safely.

(Do not try this on websites you don't own. Only on your machine or on authorized CTF labs.)

---

## 8 — Better (safer) version of the PHP code (use this if you want to learn good practices)

```
<?php
// safer include with __DIR__
include __DIR__ . "/includes/secret.inc";

// check form submit more safely
if (!empty($_POST['submit'])) {
    $user_input = $_POST['secret'] ?? '';

    // use strict compare or hash_equals for secrets
    if (hash_equals($secret, $user_input)) {
        echo "Access granted. (flag hidden)";
    } else {
        echo "Wrong secret";
    }
}
?>

<form method="post" autocomplete="off">
  <input type="text" name="secret" placeholder="type secret here">
  <input type="submit" name="submit" value="Send">
</form>
```

Why this is better:

* `__DIR__` helps include correct file path.
* `hash_equals()` avoids some timing-attack problems.
* `$_POST['secret'] ?? ''` prevents “undefined index” notices.
* We still do not show the real secret.

---

## 9 — Quick comparison table: insecure vs secure (very short)

| Thing              | Insecure (current)              | Secure (better)                                                      |
| ------------------ | ------------------------------- | -------------------------------------------------------------------- |
| Where secret saved | Plain file inside webroot maybe | Outside webroot or environment variable                              |
| Compare method     | `$secret == $_POST['secret']`   | `hash_equals($secret, $input)` or use password hashes                |
| File include       | `include "includes/secret.inc"` | `include __DIR__ . "/../secrets/secret.inc"` (outside public folder) |
| Output             | Prints password directly        | Prints safe message only                                             |

---

## 10 — Small glossary (easy)

* **webroot** = folder visible to the web server (public).
* **include** = bring code/variables from another file.
* **POST** = form data sent to server (hidden from URL).
* **hash_equals** = safe compare function for secrets.

---

