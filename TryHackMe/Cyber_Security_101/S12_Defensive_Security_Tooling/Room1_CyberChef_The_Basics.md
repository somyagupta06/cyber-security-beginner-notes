# CyberChef Basics 

CyberChef = Swiss Army knife for data.  
It is a **web-based tool** used for many cyber operations like encoding, decoding, encryption, and decryption.  
Think of it as a big toolbox where each tool (operation) does one specific job.



## Ways to Use CyberChef

1. **Online Access**
   - Just open CyberChef in your browser (internet required).
   - No installation needed.

2. **Offline / Local Copy**
   - Download CyberChef from GitHub (latest stable release).
   - Works on both Windows and Linux.
   - Good if you want to use it without internet.



## CyberChef Interface (Main 4 Areas)

1. **Operations Area**
2. **Recipe Area**
3. **Input Area**
4. **Output Area**


## 1. Operations Area
- Contains all the tools (operations).
- Well-categorized list, searchable.
- Hover on an operation → description + Wikipedia link.

### Examples of Operations:

| Operation        | Description | Example Input → Output |
|------------------|-------------|-------------------------|
| From Morse Code  | Converts Morse to text | `.... .-. . .- - ...` → `THREATS` |
| URL Encode       | Converts special chars for URLs | `https://tryhackme.com` → `https%3A%2F%2Ftryhackme%2Ecom` |
| To Base64        | Encodes text into Base64 | `This is fun!` → `VGhpcyBpcyBmdW4h` |
| To Hex           | Converts text to Hex values | `Hex is cool` → `48 65 78 20 69 73 20 63 6f 6f 6c` |
| To Decimal       | Converts text to ASCII decimal numbers | `Hi` → `72 105` |
| ROT13            | Caesar cipher (shift by 13) | `CyberChef` → `PlorePur` |


## 2. Recipe Area
- The **heart** of CyberChef.
- Here you drag & arrange operations to create your recipe.
- You can set arguments/options for each operation.

### Features:
- **Save Recipe** → Save your custom operations list.
- **Load Recipe** → Load previously saved recipe.
- **Clear Recipe** → Remove all selected operations.
- **BAKE! Button** → Runs the recipe on input.
- **Auto Bake** → Automatically runs recipe after any change.



## 3. Input Area
- Place where you enter text, file, or folder for processing.

### Features:
- **Add new input tab** → Use multiple different inputs.
- **Open folder as input** → Upload entire folder.
- **Open file as input** → Upload single file.
- **Clear input/output** → Reset both input & output.
- **Reset pane layout** → Go back to default view.



## 4. Output Area
- Shows the result of your recipe.
- Clean display of processed data.

### Features:
- **Save output to file** → Export result as `.dat`.
- **Copy raw output** → Copy processed data to clipboard.
- **Replace input with output** → Overwrite input with new result.
- **Maximize output pane** → Expand result window.


# Summary
- **Operations = Tools** (Base64, Hex, ROT13, etc.)
- **Recipe = How you combine operations**
- **Input = Your data**
- **Output = Your result**

CyberChef makes encoding/decoding easy by dragging operations into the recipe and processing your data.


---
# CyberChef Thought Process (Step-by-Step)

Before using CyberChef operations, you should first **plan** what you are trying to achieve.  
This process has **4 steps**:



## Step 1: Set a Clear Objective
- Ask yourself: **"What do I want to achieve?"**
- This gives direction and focus.
- Example:  
  - During a security investigation, you find a strange **gibberish string**.  
  - Your objective = "Find out if there is a hidden message inside this string."



## Step 2: Put Data into Input
- Take your data (text, string, file, folder) and put it into **CyberChef’s Input Area**.
- Example:  
  - Copy & paste the gibberish string into the input box.


## Step 3: Select Operations
- Decide which CyberChef operations to apply.
- This can be tricky if you don’t know the type of data.  
- Categories to explore include:  
  - **Encoding/Decoding** (Base64, Base85, ROT13, ROT47, Hex, etc.)  
  - **Encryption/Decryption**  
  - **Data Format Conversion**

- Example:  
  - From research, you find the string might be **encoded/encrypted**.  
  - Try operations like `ROT13`, `Base64`, or `Base85`.



## Step 4: Check Output
- Look at the output to see if your **objective is achieved**.
- Ask: **"Did I get the result I wanted?"**
- Example:  
  - If the output shows a readable hidden message → Success ✅  
  - If it’s still gibberish → Repeat process (go back to Step 1).



# Quick Flow (Visual Idea)

1. **Objective** → Define the goal (What do I want?)  
2. **Input** → Paste/upload your data  
3. **Operations** → Try different tools/recipes  
4. **Output** → Check results (if wrong, repeat)



# Example Walkthrough

- Found gibberish string: `U2FsdGVkX1+83uS3...`  
- **Step 1 (Objective):** Find hidden message.  
- **Step 2 (Input):** Paste string into CyberChef input.  
- **Step 3 (Operations):** Try `Base64 Decode` → still gibberish.  
  - Try `ROT13` → still gibberish.  
  - Try `Hex to Text` → success, message is revealed.  
- **Step 4 (Output):** Objective achieved 🎉

---

# CyberChef – Commonly Used Operation Categories

When using CyberChef, recognizing **operation categories** makes your work faster and easier.  
Here are the most common categories:



## 1. Extractors
These operations help you pull out (extract) useful information like IPs, URLs, or emails.

| Operation              | Description | Example |
|-------------------------|-------------|---------|
| Extract IP addresses    | Finds IPv4 and IPv6 addresses | `Ping 8.8.8.8` → `8.8.8.8` |
| Extract URLs            | Finds URLs from text (must have protocol like http/ftp) | `Visit https://tryhackme.com` → `https://tryhackme.com` |
| Extract email addresses | Finds email addresses in the text | `Contact us: admin@tryhackme.com` → `admin@tryhackme.com` |

**Tip:**  
- IPs = networking concept (address of machines).  
- Emails = format `anything@domain.com`.  
- URLs = internet resource address (like `https://example.com`).  



## 2. Date and Time
These operations are useful when working with **timestamps**.

| Operation          | Description | Example |
|--------------------|-------------|---------|
| From UNIX Timestamp | Converts UNIX timestamp → readable date | `1725654622` → `Fri Sep 6 20:30:22 +04 2024` |
| To UNIX Timestamp   | Converts readable date → UNIX timestamp | `Fri Sep 6 20:30:22 +04 2024` → `1725654622` |

**What is UNIX Timestamp?**  
- A number that counts seconds from **1 Jan 1970 UTC**.  
- Used by systems to represent time.


## 3. Data Format (Base Encodings & Decodings)

### Common Operations:

| Operation     | Description | Example |
|---------------|-------------|---------|
| From Base64   | Decode Base64 → raw text | `V2VsY29tZSB0byB0cnloYWNrbWUh` → `Welcome to tryhackme!` |
| URL Decode    | Decode `%` encoded URLs → raw values | `https%3A%2F%2Fgchq%2Egithub%2Eio%2FCyberChef%2F` → `https://gchq.github.io/CyberChef/` |
| From Base85   | Decode Base85 → text | `BOu!rD]j7BEbo7` → `hello world` |
| From Base58   | Decode Base58 → text (removes confusing chars l,I,0,O) | `AXLU7qR` → `Thm58` |
| To Base62     | Encode text → Base62 (shorter strings) | `Thm62` → `6NiRkOY` |

**What is Base Encoding?**  
- Converts **binary data** (0s & 1s) into **text format**.  
- Makes data safe to share in emails, URLs, or files.  



## Manual Base64 Example (Encoding "THM")

### Step 1: Convert to Binary
Using ASCII table:  
- T = `01010100`  
- H = `01001000`  
- M = `01001101`  
Combined = `010101000100100001001101` (24 bits)

### Step 2: Break into 6-bit Chunks
`010101 000100 100001 001101`

Convert each to decimal:
- `010101` = 21  
- `000100` = 4  
- `100001` = 33  
- `001101` = 13  

### Step 3: Use Base64 Index Table
- 21 → V  
- 4 → E  
- 33 → h  
- 13 → N  

**Result:** `"THM"` → `"VEhN"` in Base64 ✅



## URL Decode Example

| Character | Encoded Value |
|-----------|---------------|
| :         | %3A |
| /         | %2F |
| .         | %2E |
| =         | %3D |
| #         | %23 |

So,  
`https%3A%2F%2Ftryhackme.com%2Froom` → `https://tryhackme.com/room`



# Summary

- **Extractors** → Grab IPs, Emails, URLs from text.  
- **Date/Time** → Convert between human-readable and UNIX time.  
- **Data Format** → Base64, Base85, Base58, Base62, URL encode/decode.  
- **Base64 Example** → "THM" = "VEhN".  
