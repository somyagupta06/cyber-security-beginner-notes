# 🔐 Bandit Level: Bandit12 ➝ Bandit13
## 📂 Command(s) used:
👉 
1. make Temporary directory 
mktemp -d

2. copy data.txt in /tmp 
cp ~/data.txt /tmp/<temp_dir_name>

3. move to that directory
cd /tmp/<temp_dir_name>

4. converting hexdump to original binary
xxd -r data.txt > data1.bin

5. checking file type
file data1.bin

6. extracting from tar file
tar -xf data1.bin

7. extracting from gzip file after renaming
mv dataX.bin dataX.gz
gunzip dataX.gz

8. extracting file from bzip2
mv dataX.bin dataX.bz2
bzip2 -d dataX.bz2
# or direct extract
bzip2 -d dataX.bin

9. checking file type after every extraction
file <filename>


Basic Flow:
xxd -r → check file → extract (tar/gzip/bzip2) → repeat until plain text password mil jaye

## 📄 Password found:
👉 FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn

## 🧠 Notes / What I learned:
👉 
### 1. What is a Hexdump?
A hexdump means displaying a file’s binary data in a human-readable hexadecimal format.
- Binary data is stored in 0s and 1s, which is hard to read directly.
- Hexdump converts that binary into hexadecimal (00–FF) and also shows the ASCII characters on the side.
**📌 Example:**
```
xxd file.txt
```
Output:
```
00000000: 4865 6c6c 6f20 576f 726c 64              Hello World
```
Explanation:
- 00000000 → file offset (address in hex)
- 48 65 6c... → hexadecimal values (readable form of binary)
- Hello World → ASCII interpretation of those values
### 2. Reverse Hexdump
If you have a hexdump and want to recreate the original file:
```
xxd -r dump.hex > original.bin
```
- -r → reverse mode (hex → binary)
- dump.hex → your hexdump file
- > → redirect output to original.bin
### 3. Converting a File into a Hexdump
To create a hex dump of a file:
```
hexdump -C test.txt
```
Output:
```
00000000  48 65 6c 6c 6f 0a                              |Hello.|
```
### 4. mktemp -d
Creates a temporary folder with a randomly generated name (hard to guess).
```
mktemp -d
```
**📌 Example output:**
```
/tmp/tmp.ABcdEfGh12
```
### 5. cp
Used to copy files:
```
cp ~/data.txt /tmp/tmp.ABcdEfGh12
```
- ~/ → home directory
- /tmp/tmp.ABcdEfGh12 → destination folder
### 6. mv
Used to move or rename files:
```
mv data.bin data.gz
```
This renames data.bin to data.gz.
### 7. file
Checks the type of a file:
```
file data1.bin
```
**📌 Example output:**
```
data1.bin: gzip compressed data
```
This way you’ll know whether the file is tar, gzip, bzip2, or plain text.
### 8. Extraction Commands
**(a) tar files:**
```
tar -xf data.tar
```
- -x → extract
- -f → file name
- 
**(b) gzip files:**
```
gunzip data.gz
# or
gzip -d data.gz
```
**(c) bzip2 files:**
```
bzip2 -d data.bz2
```
