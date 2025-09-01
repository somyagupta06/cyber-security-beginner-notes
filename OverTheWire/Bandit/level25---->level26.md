# 🔐 Bandit Level: Bandit25 ➝ Bandit26

## 📂 Command(s) used:
👉 
- ls
- ssh -i bandit26.sshkey bandit26@localhost -p 2220
- v ---> press enter
- ":set shell =/bin/bash" ---> press enter
- ":shell"
- cat /etc/bandit_pass/bandit26
<img width="573" height="340" alt="Screenshot 2025-09-01 at 2 07 19 PM" src="https://github.com/user-attachments/assets/ce3c2370-d319-4b1c-9498-f8056c3db6ab" />

<img width="586" height="83" alt="Screenshot 2025-09-01 at 2 08 30 PM" src="https://github.com/user-attachments/assets/f690df52-b7ec-4525-a9a6-42591f3f9125" />
<img width="587" height="86" alt="Screenshot 2025-09-01 at 2 09 21 PM" src="https://github.com/user-attachments/assets/8ad078ea-0d88-4378-886d-e473b9c214dd" />
<img width="585" height="105" alt="Screenshot 2025-09-01 at 2 08 10 PM" src="https://github.com/user-attachments/assets/0e78dd38-3033-4639-97c2-0e528d78fd4f" />

<img width="576" height="91" alt="Screenshot 2025-09-01 at 2 09 49 PM" src="https://github.com/user-attachments/assets/9b17a553-4981-42ee-91c6-891d6bb33a76" />
<img width="581" height="89" alt="Screenshot 2025-09-01 at 2 10 26 PM" src="https://github.com/user-attachments/assets/1af3327d-1a60-43a9-ab65-68a7f29334ed" />

## 📄 Password found:
👉 s0773xxkk0MXfdqOfPRVr9L3jJBUOgCZ

## Notes 
#📝 Notes: Vim Editor in Bandit Level 26
## 1. What is Vim?
- Vim = "Vi Improved" → a text editor used in Linux/Unix systems.
- Think of it like Notepad, but inside the terminal.
- You can open files, edit, save, and run commands from inside Vim.
## 2. How to open Vim?
- Normally, if you type:
```
vim filename
```
→ Vim opens the file in full screen editor mode.
- But in Bandit Level 26, you don’t open it normally. Instead, you just type:
```
v
```
→ This starts Vim but in a restricted way.
## 3. Why does it look "minimized"?
When you typed v, Vim didn’t open like a big editor. It looked like a small "minimized window" at the bottom of the terminal.

👉 This happens because:
- **Bandit server is restricted:** It doesn’t allow full editor mode, only a small window inside the terminal.
- **Visual mode vs Editor mode:** v doesn’t directly mean "open full editor". Instead, it starts Vim in a way that looks like a small section at the bottom (like a split or minimized window).
- **Challenge trick:** The Bandit creators made it this way so you have to figure out how to escape into a proper shell.
## 4. Why use :set shell=/bin/bash and :shell?
- By default, Vim uses a very restricted shell (like a locked room).
- :set shell=/bin/bash tells Vim:
→ "Hey, when I ask for a shell, please use /bin/bash (a normal Linux shell) instead of the restricted one."
- :shell then opens that bash shell from inside Vim.
- From that bash shell, you can run commands like cat /etc/bandit_pass/bandit26.
## 5. Why not just open the file directly?
Good question!
👉 You couldn’t just cat the password file directly because your normal Bandit26 login doesn’t have permission to do that.

The trick was:
1. Open Vim in that weird minimized mode.
2. Change its shell setting to bash.
3. Escape into a real shell using :shell.
4. Now you are inside as Bandit26 with more freedom.
5. Finally, cat /etc/bandit_pass/bandit26 shows the password.
## 🎯 Easy Example
Think of Vim here like a hidden door.
- Normally, the Bandit level doesn’t let you touch the password.
- But they left a "tiny hidden window" (minimized Vim).
- You set the correct key (:set shell=/bin/bash) → open the hidden door (:shell) → and boom, you’re inside the house.
## ✅ Summary
- v = opens Vim but in restricted/minimized way.
- :set shell=/bin/bash = tell Vim to use real bash shell.
- :shell = open that bash shell from inside Vim.
- From there, you can run commands normally.
