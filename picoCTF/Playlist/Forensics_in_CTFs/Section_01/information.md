# information

## PROBLEM STATEMENT 
Files can always be changed in a secret way. Can you find the flag? cat.jpg


## Step-by-Step Solution:
- use command
  ` exiftool cat.jpg `
<img width="1470" height="956" alt="Screenshot 2025-09-28 at 9 20 48 PM" src="https://github.com/user-attachments/assets/e8dcde64-f23d-4cf6-8341-3fe12b137a42" />
- use cyber chef
<img width="1470" height="956" alt="Screenshot 2025-09-28 at 9 21 51 PM" src="https://github.com/user-attachments/assets/61447167-e558-45ed-84a5-0416b385e853" />

## Notes
# Forensics in CTFs – Section 01 (Easy Notes)

This is a very simple explanation of what is happening in the screenshot you shared.

---

## 1. Running `exiftool` on an image

You ran the command:

exiftool working.jpg

This prints all hidden (metadata) information about the image file `working.jpg`.

### Key output you saw:

Field | What it means
--- | ---
File Name | The actual file name on disk.
File Size | How big the file is.
MIME Type | The file type (JPEG, PNG, etc.).
Image Width / Height | The image dimensions.
ExifTool Version Number | Version of exiftool used.
Current IPTC Digest | A checksum value.
Licenses / Comments / Copyright | Often used to hide clues or flags in CTFs.

**Example:**  
In your screenshot you saw something like  
`Licenses : picoCTF{…}`  
This means someone put a hidden flag inside the metadata.

---

## 2. Using CyberChef to decode Base64 text

From exiftool you copied a suspicious long text (letters, numbers, + and /). That’s usually Base64.

Steps you took (and you can repeat):

1. Go to CyberChef online:  
https://gchq.github.io/CyberChef/

2. In the “Operations” panel choose “From Base64”.

3. Paste the string you found into the Input box.

4. Look at the Output box.

### What happened in your screenshot:

Input (Base64)  
```
GlljbnURTAt6dGV0aHVlYHRfdm9kaWZpZWQ=
```
CyberChef decoded it to  
```
picoCTF{The_metadata_is_modified}
````
So the hidden flag was “picoCTF{The_metadata_is_modified}”.

---

## 3. Why this works in CTF Forensics

- People hide clues in **metadata** fields of files.
- `exiftool` lets you **see metadata** easily.
- Often the metadata is encoded in **Base64**.
- **CyberChef** is a quick online tool to decode many formats.

---

## 4. Quick comparison table – tools used

Tool | Purpose | Example
--- | --- | ---
exiftool | Reads metadata from files | exiftool working.jpg
strings | Finds printable strings in files | strings working.jpg
CyberChef | Decodes/encodes data online | From Base64 → readable text

---

## 5. Commands you can run locally (no bash syntax):

exiftool yourfile.jpg  
strings yourfile.jpg  

Then take any weird output and paste into CyberChef.

---

## 6. Summary (one-liner)

> In forensics CTF challenges, always check file metadata (exiftool, strings). If you see strange encoded text (Base64), decode it with CyberChef to reveal hidden messages or flags.
