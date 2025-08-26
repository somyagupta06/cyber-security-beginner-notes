# Burp Suite Notes (Easy Explanation)

## 1. What is Burp Suite?
- Burp Suite is a **Java-based framework** used for **Web Application Penetration Testing**.
- It is like a **middleman** between your **browser** and the **server**.
- It can **capture, view, and modify** all HTTP/HTTPS requests & responses.
- Widely used by **security researchers, bug bounty hunters, and penetration testers**.

👉 **Think of it like this:**  
If your browser is talking to a server, Burp Suite sits in between and lets you **see & change** the conversation.

<img width="710" height="284" alt="Screenshot 2025-08-26 at 11 03 16 AM" src="https://github.com/user-attachments/assets/0368af61-50a5-471d-8eac-e42c2d6b84b7" />


## 2. Why is Burp Suite Important?
- You can **intercept requests** before they reach the server.
- You can **manipulate responses** before they reach the browser.
- Helps in **finding vulnerabilities** (like SQL injection, XSS, etc.).
- Industry-standard tool for **manual testing** of web and mobile apps (including APIs).


## 3. Editions of Burp Suite

| Edition         | Cost       | Best For                       | Key Features |
|-----------------|------------|--------------------------------|--------------|
| **Community**   | Free       | Beginners, Students, Learning | - Manual testing only <br> - Limited features <br> - No scanner |
| **Professional**| Paid (License) | Security Professionals        | - Automated vulnerability scanner <br> - Fast fuzzer (no rate limit) <br> - Save projects & generate reports <br> - Burp API for integrations <br> - Extensions supported <br> - Burp Collaborator |
| **Enterprise**  | Paid (License) | Companies needing automation | - Automated & continuous scanning <br> - Runs on server <br> - Scheduled scans (like Nessus for infra) |



## 4. Simplified View of Editions
- **Community Edition** → Manual testing only (Free).
- **Professional Edition** → All features unlocked (For pros).
- **Enterprise Edition** → Automated scanning on servers (For organizations).
<img width="1210" height="635" alt="Screenshot 2025-08-26 at 11 02 39 AM" src="https://github.com/user-attachments/assets/7096fa11-581f-4ca9-81a3-07f4d40f7089" />



## 5. Example of How Burp Works
1. You open browser (say Chrome).
2. You try to login to a website.
3. Request goes from browser → Burp Suite → Server.
4. Burp Suite lets you **pause, edit** the request (like changing username/password).
5. Server sends response → Burp Suite → Browser.
6. You can also **change the response** (like making server say “Login Successful” even if wrong password).

👉 This makes Burp Suite a **powerful testing tool**.

## 6. Focus for Us
- We will study and use **Burp Suite Community Edition** (Free).
- Demos will be shown using **Windows version**, but same works on **Linux (AttackBox)** too.
---
# Burp Suite Community Edition - Key Features 

Although **Burp Suite Community** has fewer features than **Professional**, it still has powerful tools for manual testing. Let’s explore the main features:



## 1. Proxy
- **Most famous feature of Burp.**
- Acts as a **middleman** between browser and server.
- Lets you **intercept, view, and edit** HTTP/HTTPS requests & responses.

👉 **Example:**  
You send a login request → Burp Proxy stops it → You change the username/password → Then send it to the server.



## 2. Repeater
- Used for **capturing, modifying, and resending** the same request multiple times.
- Helpful when you want to test **different payloads** (like SQL Injection or XSS).
- Perfect for **trial & error testing**.

👉 **Example:**  
Send a login request to Repeater → Try different SQL injection payloads until you find the working one.



## 3. Intruder
- Used for **brute force** or **fuzzing endpoints**.
- In **Community Edition** it has **rate limitations** (slower than Pro).
- Still useful for trying multiple inputs automatically.

👉 **Example:**  
Trying hundreds of passwords for a login page → Intruder sends them one by one.



## 4. Decoder
- Converts (decode/encode) data into different formats.
- Useful for **URL decoding, Base64 decoding, or encoding payloads**.
- Saves time instead of using separate tools.

👉 **Example:**  
Captured cookie looks like `dXNlcm5hbWU9YWRtaW4=` → Decoder can show it as `username=admin`.



## 5. Comparer
- Compares two pieces of data at **word-level** or **byte-level**.
- Very helpful when responses look similar, but you need to spot small differences.

👉 **Example:**  
Login success page vs. login failed page → Comparer highlights the exact difference in responses.



## 6. Sequencer
- Tests the **randomness of tokens** (like session IDs, cookies).
- If randomness is weak → attacker can guess session tokens → account takeover possible.

👉 **Example:**  
If session IDs follow a pattern (`1001, 1002, 1003...`) → Sequencer will show weak randomness.



## 7. Extensions (Extender + BApp Store)
- Burp is **written in Java**, but supports extensions in:
  - **Java**
  - **Python (via Jython)**
  - **Ruby (via JRuby)**
- **Burp Extender module** → allows you to load your own extensions.
- **BApp Store** → download ready-made extensions (like plugins).
- Some extensions need **Professional license**, but many work in **Community Edition**.

👉 **Example Extension:**  
- **Logger++**: Advanced logging of requests/responses inside Burp.



## Quick Feature Comparison (Community vs Professional)

| Feature       | Community (Free) | Professional (Paid) |
|---------------|------------------|----------------------|
| Proxy         | ✅ Available     | ✅ Available |
| Repeater      | ✅ Available     | ✅ Available |
| Intruder      | ✅ Available (Slow) | ✅ Fast & powerful |
| Decoder       | ✅ Available     | ✅ Available |
| Comparer      | ✅ Available     | ✅ Available |
| Sequencer     | ✅ Available     | ✅ Available |
| Extensions    | ✅ Limited       | ✅ Unlimited |
| Vulnerability Scanner | ❌ Not Available | ✅ Available |



# Summary
- Even **Community Edition** is strong for manual testing.
- **Proxy + Repeater + Intruder** = most used tools for beginners.
- Extensions (like Logger++) make it more powerful.
- For **automation & faster fuzzing**, Professional edition is better.



---
# Burp Suite Community - Starting & Dashboard Overview

## 1. Launching Burp Suite in AttackBox
- Use the **pre-installed Burp Suite Community Edition** in AttackBox.
- Steps:
  1. Click **Start AttackBox**.
  2. Open **Burp Suite**.
  3. Accept the **Terms & Conditions**.



## 2. Project Setup
- After launch, you will be asked to **select project type**.
- In **Community Edition**, options are limited → just click **Next**.
- Default settings are usually best → click **Start Burp**.

👉 **Tip:** No need to change settings unless you have special requirements.



## 3. Training Screen
- First time opening Burp → you might see a **Training window**.
- Training covers basics of Burp features.
- Recommended to go through later when free.
- If not shown → you go directly to the **Dashboard**.



## 4. The Burp Dashboard
- The main screen is **Dashboard**.
- It is divided into **4 quadrants** (like 4 boxes on the screen).
- These quadrants help monitor different activities.
<img width="651" height="336" alt="Screenshot 2025-08-26 at 11 11 14 AM" src="https://github.com/user-attachments/assets/37a0b429-7173-478e-9b19-1bc3945f6d0f" />

### Quadrants (starting top-left, counter-clockwise):
1. **Tasks**
   - Defines background tasks Burp is doing.
   - In **Community Edition** → default is **Live Passive Crawl** (it logs pages you visit).
   - In **Professional Edition** → extra tasks like **on-demand scans** are available.

2. **Event Log**
   - Shows all activities performed by Burp.
   - Example: Proxy started, connections made, errors, etc.
   - Good for troubleshooting.

3. **Issue Activity** (Professional only)
   - Lists **vulnerabilities found** by the automated scanner.
   - Sorted by severity (High, Medium, Low).
   - Filter based on certainty.
   - ❌ Not available in **Community Edition**.

4. **Advisory** (Professional only)
   - Gives details about the vulnerabilities.
   - Includes references & suggested fixes.
   - Can be exported into reports.
   - ❌ Not useful in Community Edition (shows nothing).



## 5. Help Icons
- You will see **question mark (?) icons** in many sections.
- Clicking on them opens **helpful guides** related to that specific feature.
- Very useful for beginners when confused.



## 6. Getting Comfortable
- Burp may look **complex at first**, but with practice it becomes easier.
- Explore different tabs like **Proxy, Repeater, Intruder, Decoder** etc.
- Use the **Help icons** whenever stuck.



## Summary (Step-by-Step Flow)
1. Start AttackBox → Launch Burp → Accept terms.  
2. Select project type → **Next** → **Start Burp**.  
3. Training screen may appear → explore later.  
4. Dashboard appears (4 quadrants).  
   - Tasks → Logs crawling.  
   - Event Log → Shows actions.  
   - Issue Activity → (Pro only).  
   - Advisory → (Pro only).  
5. Use **Help (?) icons** for guidance.  
6. Explore tabs → Get familiar with tools.



## Quick Table: Dashboard Differences

| Section        | Community Edition | Professional Edition |
|----------------|------------------|-----------------------|
| Tasks          | ✅ Live Passive Crawl only | ✅ Extra tasks (scans, configs) |
| Event Log      | ✅ Available     | ✅ Available |
| Issue Activity | ❌ Not available | ✅ Shows vulnerabilities |
| Advisory       | ❌ Not available | ✅ Detailed info & reports |
---
# Burp Suite - Navigation 

In Burp Suite, navigation mainly happens using the **top menu bars**.  
There are two levels of navigation:



## 1. Module Selection (Top Menu Bar)
- The **top row** of Burp Suite shows all available **modules**.
- Each module represents a **feature/tool** (like Proxy, Repeater, Intruder, etc.).
- Example: If you click on **Proxy**, you enter the Burp Proxy module.
<img width="1105" height="56" alt="Screenshot 2025-08-26 at 11 17 03 AM" src="https://github.com/user-attachments/assets/0a9c0121-4f57-4f19-96ba-1d9aa657703c" />



## 2. Sub-Tabs (Second Menu Bar)
- Some modules have **multiple sub-tabs**.
- These appear just below the **main menu bar**.
- Example: Inside **Proxy module**, you’ll see sub-tabs like:
  - **Intercept**
  - **HTTP history**
  - **Options**

👉 Each sub-tab gives access to **module-specific settings**.



## 3. Detaching Tabs
- You can **separate any tab** into a new window.
- Useful if you want to view multiple tabs at once.
- How to do it:
  1. Go to **Window (top menu)**.
  2. Select **Detach tab**.
  3. Tab opens in a **new window**.
- To reattach → use the same method.

<img width="186" height="338" alt="Screenshot 2025-08-26 at 11 17 23 AM" src="https://github.com/user-attachments/assets/3af50300-68da-49a3-b2bb-5315382a0ec4" />


## 4. Keyboard Shortcuts (Default)
Burp provides shortcuts for quick access to important modules.

| Shortcut            | Opens Tab        |
|---------------------|------------------|
| **Ctrl + Shift + D** | Dashboard        |
| **Ctrl + Shift + T** | Target           |
| **Ctrl + Shift + P** | Proxy            |
| **Ctrl + Shift + I** | Intruder         |
| **Ctrl + Shift + R** | Repeater         |

👉 These save time instead of clicking manually.



## Summary
1. **Modules** = Main tools (Proxy, Repeater, Intruder, etc.) → top bar.  
2. **Sub-Tabs** = Options/settings inside a module → second bar.  
3. **Detach Tabs** = Open tab in a separate window (via Window → Detach).  
4. **Shortcuts** = Quickly switch between important tabs.  

---
# Burp Suite - Settings 
Before learning Burp Proxy, let’s understand **how settings work in Burp Suite**.  
There are **two types of settings**:



## 1. Global Settings (User Settings)
- Apply to the **entire Burp Suite installation**.
- Remain active **every time you open Burp Suite**.
- Good for setting your **default / permanent preferences**.

👉 Example: Setting proxy interception behavior once, and it stays same every time.



## 2. Project Settings
- Apply only to the **current project/session**.
- Lost when you **close Burp** (in **Community Edition** projects cannot be saved).
- Useful for **temporary changes** during a test.

👉 Example: Adding scope URLs for one test project (will reset next time).
<img width="1208" height="75" alt="Screenshot 2025-08-26 at 11 21 49 AM" src="https://github.com/user-attachments/assets/ebf05b36-0291-4ef9-8e4c-b2995648f959" />

<img width="1211" height="682" alt="Screenshot 2025-08-26 at 11 22 06 AM" src="https://github.com/user-attachments/assets/caaba38d-73e7-41dd-985f-77b9df9b04a4" />


## 3. Accessing Settings
- Click on **Settings button** in the top navigation bar.
- This opens a **separate Settings window**.

Inside Settings window (left-hand menu):
- **Search** → Search settings with keywords.
- **Type filter** → Filter between **User settings** (Global) and **Project settings**.
- **User settings** → Show permanent global settings.
- **Project settings** → Show current session/project settings.
- **Categories** → Organize settings by topic (e.g., Proxy, Repeater, etc.).
<img width="1261" height="751" alt="Screenshot 2025-08-26 at 11 22 37 AM" src="https://github.com/user-attachments/assets/aa0dca62-4919-46b1-b7e1-ff157032a3f5" />



## 4. Tool-Specific Shortcuts
- Many Burp modules have **direct buttons** to open settings for that tool.  
- Example: In **Proxy module**, there is a **Proxy settings button** → directly opens proxy-related settings.
<img width="516" height="83" alt="Screenshot 2025-08-26 at 11 22 51 AM" src="https://github.com/user-attachments/assets/e7e2ba44-94f4-4ece-a54b-23e1db912a4a" />



## 5. Search Feature
- The **search bar** in settings is very helpful.
- Instead of scrolling, just type keyword (like *proxy* or *decoder*).
- Takes you straight to that setting.


## Summary (Step-by-Step Flow)
1. **Two types of settings**:  
   - Global (User) → Permanent, for all sessions.  
   - Project → Temporary, resets after session ends (in Community Edition).  

2. **Settings window access**: Top bar → Settings button.  

3. **Settings window sections**:  
   - Search  
   - Type filter (User / Project)  
   - User settings  
   - Project settings  
   - Categories  

4. **Shortcuts available** inside modules (like Proxy → Proxy Settings).  



## Quick Comparison: User vs Project Settings

| Feature              | User (Global Settings) | Project Settings |
|----------------------|-------------------------|------------------|
| Scope                | Whole Burp installation | Only current session |
| Persistence          | Saved permanently       | Lost after closing Burp (Community) |
| Use case             | Default configs         | Temporary project-specific configs |

---
# Configuring Burp Proxy with FoxyProxy

To make Burp Suite work, your **browser traffic** must go through Burp.  
We’ll use **FoxyProxy** in Firefox for this.

-

## 1. Start the Setup
- Click **Start Machine** to begin.
- Open **Burp Suite** in your AttackBox.



## 2. Install FoxyProxy
- FoxyProxy is a Firefox extension for managing proxies.
- **Already installed** on the AttackBox.
- If not, download **FoxyProxy Basic** from Firefox Add-ons.



## 3. Access FoxyProxy Options
- Look at the **top-right corner** of Firefox → FoxyProxy icon.
- Click it → choose **Options**.
- A new tab will open with configurations.



## 4. Add Burp Proxy Configuration
1. In FoxyProxy **Options page** → click **Add**.
2. Fill in details:
   - **Title:** Burp (or any name you like)  
   - **Proxy IP:** `127.0.0.1`  
   - **Port:** `8080`  
3. Click **Save**.



## 5. Activate Proxy
- Click FoxyProxy icon → select **Burp** configuration.  
- Now, browser traffic goes through **127.0.0.1:8080**.  
- Make sure **Burp Suite is running**, otherwise requests won’t work.



## 6. Enable Intercept in Burp
- Open **Burp Suite → Proxy tab**.
- Turn **Intercept is on**.



## 7. Test the Proxy
- In Firefox, go to: `http://10.201.118.188/`
- Your browser will **hang** (request paused).
- In Burp Proxy, you’ll see the **captured HTTP request**.

🎉 Congrats! You intercepted your first request.



## Important Notes
1. When **Intercept is ON**:
   - Browser will **hang** until you **Forward or Drop** the request.
   - Don’t forget to switch it off if you want smooth browsing.

2. Right-click on a request in Proxy:
   - **Forward** → Send to server.  
   - **Drop** → Cancel it.  
   - **Send to Repeater/Intruder/Decoder/Comparer** → For deeper testing.  

3. **Close extra tabs** in AttackBox browser:
   - Otherwise, you may see extra **WebSocket requests** instead of target site traffic.



## Quick Workflow Summary
1. Open Burp + Start Machine.  
2. Set FoxyProxy → Burp (127.0.0.1:8080).  
3. Turn **Intercept on** in Burp.  
4. Visit target site → request appears in Proxy.  
5. Forward/Drop/Edit request as needed.  

---
# Burp Suite – Target Tab 

The **Target tab** helps you organize and understand your testing scope.  
It has **three sub-tabs**:



## 1. Site Map
- Displays the **web application structure** in a **tree view**.
- Every page visited while proxy is active → added to site map.
- Useful for:
  - Creating a **map of the app** during enumeration.
  - Tracking API endpoints automatically.
- **Community Edition:** Site map is **passive** (based on what you browse).
- **Professional Edition:** Includes **automated crawling** to explore more links/pages.

👉 **Example:** If the homepage links to `/about`, `/contact`, `/api/v1/users`, all appear in the site map as you browse.



## 2. Issue Definitions
- Even though **Community Edition** does not scan automatically,  
  you still get access to a **list of vulnerabilities** Burp checks for.
- Includes:
  - Description of each vulnerability.
  - References and remediation notes.
- Useful for:
  - Writing reports.
  - Understanding what a vulnerability means when you manually discover it.

👉 **Example:** If you find a reflected XSS, you can look it up in Issue Definitions for a clear explanation + references.



## 3. Scope Settings
- Lets you **include/exclude domains/URLs** from your testing scope.
- Helps Burp focus only on your target → avoids clutter from unrelated traffic.
- Useful for:
  - Targeting a specific domain or IP only.
  - Preventing accidental tests on unrelated websites.

👉 **Example:** You add `http://10.201.118.188/*` to scope → Burp ignores all other requests.



## Summary of Target Tab

| Sub-tab          | Purpose |
|------------------|---------------------------------------------------|
| **Site Map**     | Maps application structure while browsing. |
| **Issue Definitions** | Library of vulnerabilities (descriptions + references). |
| **Scope Settings** | Control which hosts/URLs are in testing scope. |



# 🔎 Challenge

1. Visit **http://10.201.118.188/** in your browser (inside AttackBox).  
2. Click **every link on the homepage** (explore all sub-pages).  
3. Open Burp → **Target tab → Site map**.  
4. Check the endpoints listed.  
   - Most will look normal (e.g., `/about`, `/contact`, `/products`).  
   - One endpoint will look **unusual / suspicious** compared to the rest.  

👉 Visit that unusual endpoint in browser, **or** check its **Response** in the site map.  

⚠️ This is a hands-on exercise. The “odd endpoint” will stand out because it doesn’t look like a typical webpage and may reveal something important.
---
# Burp Suite – Burp’s Built-in Browser (Easy Topic ✅)

So far, we configured **FoxyProxy in Firefox** to route traffic through Burp.  
But Burp makes life easier by including its own **built-in Chromium browser**, which is already pre-configured to use the proxy.



## 🖥 How to Use Burp’s Browser
1. Open Burp Suite → go to the **Proxy tab**.  
2. Click the **Open Browser** button.  
3. A **Chromium window** will pop up → all requests automatically flow through the proxy.  

👉 No need to install FoxyProxy or manually configure proxy settings.



## ⚙️ Settings
- Burp’s Browser has its own settings under:
  - **Settings → Tools → Burp’s browser**
  - You can customize behavior (cookies, sandboxing, etc.).



## ⚠️ Problem on Linux (root user – like in AttackBox)
- Running as **root** sometimes prevents Burp’s browser from starting.  
- Reason: Chromium can’t create a **sandbox environment**.

### ✅ Two Solutions:
1. **Smart Option (Secure)**  
   - Create a **new low-privilege user account**.  
   - Run Burp Suite from that account → Browser works normally.

2. **Easy Option (Quick Fix in AttackBox)**  
   - Go to:  
     `Settings → Tools → Burp’s browser`  
   - Check ✅ **Allow Burp's browser to run without a sandbox**.  
   - This bypasses sandbox restrictions → Browser starts fine.  
   - ⚠️ **Warning**: Disabling sandbox weakens security. If browser is hacked, attacker could gain access to your machine.  
   - Safe enough inside **AttackBox training environment**, but not recommended on your main PC.



## 📌 Summary
- Burp Browser = built-in Chromium, **already configured** to use proxy.  
- Use it for faster setup, testing, and avoiding FoxyProxy hassle.  
- If running as root → either create a non-root user (safe) or disable sandbox (quick fix).  

---

# Burp Suite – Scoping (🎯 One of the Most Important Features)

When we use Burp Proxy without scoping, **every request from our browser** gets captured —  
including background requests (ads, analytics, updates, etc.). This quickly becomes **messy**.  

That’s why **Scoping** is so important.



## 📝 What is Scoping?
Scoping = Defining **which applications/domains/IPs** Burp should focus on.  
- Only traffic **in scope** will be **logged & analyzed**.  
- Helps us avoid **noise** and keep Burp focused on the actual target(s).

## 🔧 How to Set Scope
1. Go to **Target tab**.  
2. Find your target (in the left tree under **Site map**).  
3. **Right-click → Add to Scope**.  
4. Burp asks:  
   - *“Do you want to stop logging anything that’s not in scope?”*  
   - Choose **Yes** (usually the right choice).  



## ✅ Checking / Editing Scope
- Go to **Target tab → Scope settings (sub-tab)**.  
- Here you can:  
  - **Include** domains/IPs.  
  - **Exclude** domains/IPs.  
- Very powerful for managing exactly what Burp should care about.



## ⚠️ Important: Interception Still Captures Out-of-Scope
Even after disabling logging, Burp **still intercepts** all traffic by default.  

👉 To fix this:
1. Go to **Proxy tab → Options (settings)**.  
2. In **Intercept Client Requests**, select:  
   - **“And URL Is in target scope”**.  

Now, Burp will **ignore all out-of-scope requests**, making your Proxy view super clean.


## 📌 Summary
- **Add target → Add to Scope** (keeps focus only on chosen app).  
- Check scope in **Target → Scope settings**.  
- For clean Proxy view → enable **Intercept only in scope**.  

---
# Burp Suite – Fixing TLS/HTTPS Certificate Issues (⚡Important for HTTPS Interception)

When you try to intercept **HTTPS traffic** (e.g., `https://google.com/`) using Burp,  
your browser often throws a **certificate error** ❌ like:  
*“The certificate is not trusted / PortSwigger CA not authorized”*.  

👉 Why?  
Because Burp acts as a **man-in-the-middle proxy**, generating its **own certificates**.  
But your browser doesn’t trust Burp’s **PortSwigger CA** by default.  

To fix this, we must **install Burp’s CA certificate** into our browser.
<img width="904" height="512" alt="Screenshot 2025-08-26 at 12 52 26 PM" src="https://github.com/user-attachments/assets/a7e69c9e-bad5-4e64-bd59-35b73f3463c7" />



## 🛠 Steps to Install Burp’s CA Certificate in Firefox

1. **Download the Certificate**  
   - Make sure Burp Proxy is running.  
   - Go to: `http://burp/cert`  
   - This will download a file called **`cacert.der`**.  
   - Save it somewhere safe (e.g., Desktop).

2. **Open Firefox Certificate Settings**  
   - Type: `about:preferences` in the Firefox address bar.  
   - Search for **“certificates”**.  
   - Click **View Certificates**.
<img width="717" height="356" alt="Screenshot 2025-08-26 at 12 52 42 PM" src="https://github.com/user-attachments/assets/fee1b15d-481a-40c8-a65d-b5627b1f1df4" />

3. **Import Burp’s Certificate**  
   - In the **Certificate Manager** window → click **Import**.  
   - Select the file **`cacert.der`** you downloaded.

4. **Trust the Certificate**  
   - A window will pop up → check ✅  
     - *“Trust this CA to identify websites”*.  
   - Click **OK**.

<img width="683" height="311" alt="Screenshot 2025-08-26 at 12 52 57 PM" src="https://github.com/user-attachments/assets/6dac85a0-604b-4a49-905a-01e674847c68" />


## 🎉 Result
Now your browser **trusts Burp’s CA**.  
- You can intercept **HTTPS/TLS sites** without certificate errors.  
- All traffic (HTTP + HTTPS) can be decrypted and analyzed in Burp.



## ⚠️ Notes
- In the **AttackBox**, this is **already configured** → you can skip these steps.  
- On your personal machine, you **must do this once per browser**.  
- Be cautious: If you import Burp CA and someone else uses your browser,  
  they could run Burp and intercept your traffic too. So only keep it installed on testing setups.



📌 **In short**: Burp is like a fake "middle-man CA". Installing its certificate makes your browser *trust Burp as a secure authority*, so it stops complaining when Burp generates fake TLS certificates on the fly.
---
# 🧪 Real-World Example: Reflected XSS with Burp Proxy

## 1️⃣ Understand the Scenario
- Target page: http://10.201.118.188/ticket/  
- It has a support form (with email + query fields).  
- We’re testing for Reflected XSS.  

Normal test payload (in email field):  
```bash
<script>alert("Succ3ssful XSS")</script>  
```
But the client-side filter blocks special characters (since email shouldn’t contain <, >, etc.).



## 2️⃣ Bypass the Client-Side Filter with Burp
👉 Client-side filters only stop you in the browser.  
Burp lets us edit the raw HTTP request before sending it to the server.  

Steps:
- Open Burp → Turn Proxy Intercept **ON**.  
- Fill out the form normally:  
  - Email: pentester@example.thm  
  - Query: Test Attack  
- Submit → Burp intercepts the request.  



## 3️⃣ Inject the Payload
Inside Burp Proxy, find the email field in the captured request:  
```bash
email=pentester@example.thm&query=Test+Attack  
```
Replace the email value with your payload:  
```bash
email=<script>alert("Succ3ssful XSS")</script>&query=Test+Attack  
```


## 4️⃣ URL Encode the Payload
If we send raw <script> characters, the request might break.  
So we URL-encode it (Burp shortcut: Ctrl + U).  

Encoded version becomes:  
```bash
email=%3Cscript%3Ealert(%22Succ3ssful+XSS%22)%3C%2Fscript%3E&query=Test+Attack  

```
## 5️⃣ Forward the Request
- Click **Forward** in Burp.  
- Browser receives the server’s response.  

🎉 You should see an alert popup:  
```bash
Succ3ssful XSS  
```


# ✅ Key Takeaways
- Client-side filters are weak: bypassable by intercepting requests.  
- URL encoding is often required for safe payload delivery.  
- Burp Proxy allows full control over modifying traffic before the server sees it.  
- Reflected XSS only affects the current request/response (not stored).  



# ⚡ Pro Tip: Extra Payloads to Try
```bash
<img src=x onerror=alert(1)>  
"><svg/onload=alert(1337)>  
<script>confirm('XSS')</script>  
```
