# 🔐 Bandit Level: Bandit6 ➝ Bandit7
## 📂 Command(s) used:
👉 find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null


## 📄 Password found:
👉 morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj

## 🧠 Notes / What I learned:
👉 
## Command used explaination
- **find /**	        Search from root folder (poora system)
- **-type f**	        Only search Files (not Folders)
- **-user bandit7**	  Owner of the file must be bandit 7
- **-group bandit6**	File group must be bandit 6
- **-size 33c**	      File size must be exactly 33bytes (c = characters/bytes)
- **2>/dev/null**   	Hide errors (permissions errors etc.)


## Type of Outputs in Linux
What is Output Type?
- stdout (1>)	  Normal output, file name, result, etc.
- stderr (2>)	  Error messages, example "Permission denied"
- stdin	        Input 

1. 2> means: "error output (stderr) ..."
2. /dev/null means: "...put into bin"
