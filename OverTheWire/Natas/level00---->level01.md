# 🔐 Natas Level: Natas0--->Natas1



### 📂 Execution Process 

👉 
- Go to settings
- Developer Tools
<img width="489" height="354" alt="Screenshot 2025-09-25 at 9 03 35 PM" src="https://github.com/user-attachments/assets/86c56ffd-9f7a-4cb6-907e-e176a921f69d" />
<img width="1470" height="956" alt="Screenshot 2025-09-25 at 9 03 51 PM" src="https://github.com/user-attachments/assets/5965e4ae-7d6e-4f79-b2f0-593b0b2130df" />
<img width="1462" height="485" alt="Screenshot 2025-09-25 at 9 04 18 PM" src="https://github.com/user-attachments/assets/39d54c3a-f9ca-4251-884c-902cf028ef4c" />

### 📄 Password found:
👉
TguMNxKo1DSa1tujBLuZJnDUlCcUAPlI 

### Notes

# How to Check if Right-Click is Disabled

## Method 1: Try Right-Clicking
- Simply **right-click** anywhere on the page.
- If nothing happens (no context menu), right-click might be disabled.

---

## Method 2: Check Event Listeners via Browser DevTools
1. Open Developer Tools:  
   - Chrome/Edge: `F12` or `Ctrl+Shift+I`  
   - Firefox: `Ctrl+Shift+I`
2. Go to the **Console** or **Elements** tab.
3. Look for scripts that **listen for the `contextmenu` event**:
   ```javascript
   document.addEventListener('contextmenu', e => e.preventDefault())
   ```
If this exists, right-click is being blocked.
## Method 3: Test Removing Event Listeners
- Open Console in DevTools.
- Run the following command to remove all contextmenu blockers:
```
document.oncontextmenu = null;
```
Or:
```
document.removeEventListener('contextmenu', function(){})
```
- Try right-clicking again.
  - If it works now, the website was blocking right-click via JS.
## Method 4: Check HTML Attributes
- Some elements may have inline attributes like:
<body oncontextmenu="return false;">
- If present, this disables right-click on the page.
## Summary
- Disabled right-click is usually done via:
- contextmenu event in JavaScript
- oncontextmenu="return false" in HTML
- You can inspect and override these using browser DevTools and the Console.

