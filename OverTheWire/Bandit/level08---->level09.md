# 🔐 Bandit Level: Bandit8 ➝ Bandit9
## 📂 Command(s) used:
👉 sort data.txt | uniq -u

## 📄 Password found:
👉 4CKMh1JI91bUIZZPXDqGanal4xvAg0JM

## 🧠 Notes / What I learned:
👉 -u flag in uniq means:

"Print only those lines which are unique in the input — those appear only once."




For default normal uniq prints the first occurence of duplicate lines ( prints unique + first of duplicates).
But when we use -u flag than it will print only the first occurance not tha duplicates.


### 1. Redirecting to a File (> and >>)
Normally, when we run a command, the output appears on the screen.

But sometimes, we want to save that output into a file.
- > → Saves the output into a file. If the file already exists, the old data is deleted and replaced with the new output.
- >> → Appends the output at the end of the file. The old data is not deleted.
**📌 Example:**
```
ls > myoutput
```
This command will save the list of the current folder into the file myoutput.
```
ls >> myoutput
```
This will append the output at the end of myoutput without deleting the old data.
### 2. Redirecting from a File (<)
This works in the opposite way — instead of giving output to a file, it takes input from a file.
**📌 Example:**
```
wc -l < barry.txt
```
Here, the wc (word count) command will take content from barry.txt, but it won’t display the file name in the output.
### 3. Redirecting Errors (2> and 2>&1)
In the terminal, there are two types of output:
- STDOUT → normal output
- STDERR → error messages
- 2> → Redirects error messages into a file.
- 2>&1 → Combines errors with normal output and saves both into a file.
📌 Example:
```
ls -l video.mpg blah.foo 2> errors.txt
```
Here, since blah.foo doesn’t exist, its error message will be saved in errors.txt.
### 4. Piping (|)
Piping means sending the output of one command directly as the input of another command.
 
It works just like a water pipe — water (data) flows from one pipe into the next.
**📌 Example:**
```
ls | head -3
```
- ls → lists the folder contents
- head -3 → shows the first 3 lines
**Result:** Displays the first 3 files/folders.
You can also create a chain of pipes:
```
ls | head -3 | tail -1
```
This will take the first 3 files from the list and then display only the last one of them.
### 5. Mix and Match
You can use > and | together.
**📌 Example:**
```
ls | head -3 | tail -1 > myoutput
```
Here, the output will be saved into the file myoutput.
