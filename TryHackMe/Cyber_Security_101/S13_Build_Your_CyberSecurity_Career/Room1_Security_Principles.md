# 🔐 Security Basics 

## 1. Why Security?
- Har company bolti hai ki unka product/service secure hai.
- Lekin "secure from whom?" ye samjhna jaruri hai.

### Example:
- Toddler (chhota bacha) se laptop bachana hai → simple lock kaafi hai.
- Industrial spy (jo million-dollar design churana chahta hai) se bachana hai → advanced security chahiye.
- Matlab adversary (attacker ka type) samjhna sabse pehla step hai.



## 2. Perfect Security?
- 100% perfect security possible **nahi hoti**.
- Goal: apne system ko itna secure karna ki attacker ko todna mushkil ho jaye.



## 3. CIA Triad (Confidentiality, Integrity, Availability)

| Element          | Meaning | Example |
|------------------|---------|---------|
| **Confidentiality** | Sirf authorized log hi data access kar saken | Online shopping me credit card number sirf bank ko dikhe, hacker ko nahi |
| **Integrity**       | Data change na ho ya agar change ho to detect ho | Shopping order ka shipping address koi attacker badal de → integrity broken |
| **Availability**    | System/data jab zarurat ho tab available rahe | Agar shopping website down hai → order nahi place hoga |



## 4. CIA in Healthcare Example

- **Confidentiality:** Patient records illegal leak nahi hone chahiye.
- **Integrity:** Record galat edit ho gaya → galat treatment → jaan ko khatra.
- **Availability:** Doctor ko record time par na mile → diagnosis galat ho sakta hai.

👉 Har situation me 3 ka importance alag ho sakta hai.  
E.g. University announcement → confidentiality important nahi hai, but integrity important hai.



## 5. Beyond CIA (Extra Security Properties)

| Element           | Meaning | Example |
|-------------------|---------|---------|
| **Authenticity**  | Data asli ho, fake/counterfeit na ho | Online order genuinely customer ne hi place kiya ho |
| **Non-repudiation** | Data bhejne wala baad me mana na kar sake | Customer ne car ka order diya aur baad me "maine order nahi kiya" na keh sake |



## 6. Parkerian Hexad (1998)

6 Security Elements = CIA + Extra 3

| Element | Meaning | Example |
|---------|---------|---------|
| **Confidentiality** | Data sirf authorized users ko mile | Credit card info |
| **Integrity** | Data correct aur unchanged ho | Medical record |
| **Availability** | Jab chahiye tab accessible ho | Website online ho |
| **Authenticity** | Source asli ho | Genuine shopping order |
| **Non-repudiation** | Source deny na kar sake | Customer deny na kare order ko |
| **Possession** | Data hamare control me rahe | Backup drive chori ho jaye → possession lost |
| **Utility** | Data useful form me ho | Encrypted laptop jiski key kho gayi → available hai but useless |



## 7. Important Security Principles

- **Defence-in-Depth:** Ek se zyada layers of security use karna.  
  (e.g. password + OTP + firewall + antivirus)
- **Zero Trust:** Kisi par blindly trust na karo. Har request verify karo.  
- **Trust but Verify:** Agar trust kar bhi rahe ho to verify karna zaruri hai.



## 8. Key Terms

| Term           | Meaning | Example |
|----------------|---------|---------|
| **Vulnerability** | Weakness ya flaw in system | Outdated software version |
| **Threat**        | Potential danger exploiting vulnerability | Hacker attack |
| **Risk**          | Probability × Impact of threat | Weak password + attacker try kare = high risk |

---
# 🔐 DAD Triad (Opposite of CIA)

## 1. CIA vs DAD

| CIA Triad (Good)       | DAD Triad (Attack/Opposite) |
|-------------------------|-----------------------------|
| **Confidentiality** → Only authorized log access kar saken | **Disclosure** → Secret data leak/disclose ho jaye |
| **Integrity** → Data unchanged & correct rahe | **Alteration** → Data modify/change kar diya jaye |
| **Availability** → System/data accessible rahe | **Destruction/Denial** → System/data destroy ya unavailable ho jaye |



## 2. DAD in Healthcare Example

- **Disclosure (Opposite of Confidentiality):**  
  - Patient medical records chura liye gaye aur online public dump kar diye.  
  - Result → Legal issues + reputation loss.

- **Alteration (Opposite of Integrity):**  
  - Attacker ne patient records edit kar diye (e.g., medicine dose badal di).  
  - Result → Wrong treatment → life-threatening situation.

- **Destruction/Denial (Opposite of Availability):**  
  - Hospital ka pura system down kar diya (DoS attack).  
  - Paperless system hone ki wajah se records access nahi ho paye.  
  - Result → Facility stall ho gayi, operations ruk gaye.



## 3. Balance Between CIA

- Sirf ek cheez ko extreme secure karna bhi dangerous hai.

### Example:
- **Too much Confidentiality & Integrity →** Availability suffer karegi.  
  (Data itna lock kar diya ki doctor timely access nahi kar pa raha.)
- **Too much Availability →** Confidentiality & Integrity lose ho sakte hain.  
  (Sabko open access diya → records leak aur alter ho sakte hain.)

👉 Best practice = **Balance maintain karna** between C, I, A.

---

# 🔐 Security Models (CIA Implementation)

Security functions (Confidentiality, Integrity, Availability) ko ensure karne ke liye alag-alag **security models** banaye gaye hain.  
Yahan 3 foundational models cover karenge:



## 1. Bell-LaPadula Model (Confidentiality Focus)

- **Goal:** Confidentiality ensure karna (data leak na ho).
- **Rules:**
  1. **Simple Security Property (No Read Up):**  
     - Lower level subject higher level data **read** nahi kar sakta.  
     - Example: Secret clearance wala file ek "Confidential" employee padh sakta hai, but "Top Secret" file nahi.
  2. **Star Security Property (No Write Down):**  
     - Higher level subject lower level data par **write** nahi kar sakta.  
     - Example: Top Secret officer apni info "Unclassified" file me nahi likh sakta.
  3. **Discretionary Security Property:**  
     - Access Matrix use hota hai (read/write permissions table).  

- **Summary:** **Write Up, Read Down**  
- **Limitation:** File sharing handle nahi karta.



## 2. Biba Model (Integrity Focus)

- **Goal:** Integrity ensure karna (data sahi & unchanged rahe).
- **Rules:**
  1. **Simple Integrity Property (No Read Down):**  
     - Higher integrity subject lower integrity data **read** nahi kar sakta.  
     - Example: High-quality financial report ek low-trust source se data nahi padhta.
  2. **Star Integrity Property (No Write Up):**  
     - Lower integrity subject higher integrity data par **write** nahi kar sakta.  
     - Example: Intern (low integrity) CEO ke final report ko edit nahi kar sakta.

- **Summary:** **Read Up, Write Down**  
- **Limitation:** Insider threats handle nahi karta.



## 3. Clark-Wilson Model (Practical Integrity)

- **Goal:** Integrity ensure karna using constraints.
- **Concepts:**
  - **Constrained Data Item (CDI):** Sensitive/critical data (e.g., bank balance).  
  - **Unconstrained Data Item (UDI):** General data (e.g., user input).  
  - **Transformation Procedures (TPs):** Valid operations on CDIs (e.g., debit/credit transaction).  
  - **Integrity Verification Procedures (IVPs):** Check data validity (e.g., balance ≠ negative).  

- **Focus:** Ensure only well-formed transactions allowed.  
- **Example:**  
  - Bank account (CDI) ko sirf authorized transaction (TP) hi modify kar sakti hai.  
  - Agar user ne galat data diya (UDI), system usko reject karega ya validate karega.



## 4. CIA Model Mapping

| Model            | Focus           | Rule Summary            |
|------------------|-----------------|-------------------------|
| **Bell-LaPadula** | Confidentiality | Write Up, Read Down     |
| **Biba**         | Integrity       | Read Up, Write Down     |
| **Clark-Wilson** | Integrity       | Well-formed Transactions |



## 5. Other Security Models (FYI)

- **Brewer-Nash Model:** "Chinese Wall" → Prevent conflict of interest.  
- **Goguen-Meseguer Model:** Based on non-interference (military).  
- **Sutherland Model:** Based on system states and information flow.  
- **Graham-Denning Model:** Secure rights transfer between subjects/objects.  
- **Harrison-Ruzzo-Ullman Model:** Access rights management.

---
# 🔐 Defence-in-Depth & ISO/IEC 19249 Principles



## 1. Defence-in-Depth (Multi-Level Security)

- Matlab ek single lock par bharosa na karke multiple layers of security lagana.  
- Goal = Attacker ko **slow karna + stop karna**.

### Analogy:
- Expensive items ek **locked drawer** me rakhe hain.  
- Sirf drawer lock par trust mat karo.  
- Extra layers:
  - Room lock  
  - Apartment main door lock  
  - Building gate lock  
  - Security cameras  
- Result: Har extra layer attacker ke liye aur time-consuming banata hai.


## 2. ISO/IEC 19249 (2017)  

International standard jo **security architecture & design principles** define karta hai.  
2 categories: **Architectural (5)** + **Design (5)** principles.



### A. 5 Architectural Principles

1. **Domain Separation**  
   - Har group of components apna alag "domain" rakhta hai with its own attributes.  
   - Example: x86 CPU privilege levels → Ring 0 (OS kernel), Ring 3 (apps).  

2. **Layering**  
   - System ko multiple abstract layers me todna.  
   - Example: OSI model in networking → har layer apna role play karti hai.  
   - Defence-in-Depth ka core idea.  

3. **Encapsulation**  
   - Internal details hide karo; sirf controlled methods allow karo.  
   - Example: OOP me `increment()` method instead of direct `seconds++`.  
   - Example: Database access via API, not direct raw queries.  

4. **Redundancy**  
   - Backup systems use karke availability & integrity ensure karna.  
   - Example:  
     - Dual power supplies in server  
     - RAID 5 disks → ek disk fail hone par bhi data safe  

5. **Virtualization**  
   - Single hardware par multiple isolated OS run karna.  
   - Benefits: Sandboxing, malware detonation, security isolation.  



### B. 5 Design Principles

1. **Least Privilege**  
   - "Need-to-know basis" → Minimum permissions hi do.  
   - Example: Agar sirf read chahiye to write permission mat do.  

2. **Attack Surface Minimisation**  
   - Extra services/features band karke vulnerabilities kam karo.  
   - Example: Linux hardening → unnecessary services disable.  

3. **Centralized Parameter Validation**  
   - User/system input ek centralized place par validate karo.  
   - Goal: Invalid/malicious inputs se bachav.  

4. **Centralized General Security Services**  
   - Common security services centralize karo.  
   - Example: Authentication ek centralized server se ho.  

5. **Preparing for Error & Exception Handling**  
   - Systems "fail safe" design karo.  
   - Example: Firewall crash → block all traffic (not allow all).  
   - Avoid info leakage in error messages.  



## 3. Quick Reference Table

| Category        | Principle                          | Example |
|-----------------|------------------------------------|---------|
| **Architecture** | Domain Separation | Kernel ring 0 vs User ring 3 |
|                 | Layering | OSI model layers |
|                 | Encapsulation | OOP increment() method |
|                 | Redundancy | RAID 5 disks |
|                 | Virtualization | Cloud VMs |
| **Design**      | Least Privilege | Only read access given |
|                 | Attack Surface Minimisation | Disable unused services |
|                 | Centralized Parameter Validation | Input validation library |
|                 | Centralized General Security Services | Central auth server |
|                 | Error & Exception Handling | Fail-safe firewall |



## 4. Questions Guide (Number Answers)

When a question asks → “refer to ISO/IEC 19249 design principles” →  
Answer with a **number 1–5** (based on design principles list above).

| Number | Principle                        |
|--------|----------------------------------|
| **1**  | Least Privilege                  |
| **2**  | Attack Surface Minimisation      |
| **3**  | Centralized Parameter Validation |
| **4**  | Centralized General Security Services |
| **5**  | Error & Exception Handling       |

---

# 🔐 Trust in Security Principles



## 1. Trust Overview
- Trust = Bahut complex topic hai.  
- Without trust, systems aur business chal hi nahi sakte.  
- Example:  
  - Agar laptop vendor par trust na ho → banda system ko rebuild karega.  
  - Agar hardware vendor par bhi trust na ho → phir device use hi nahi karega.  

👉 Business level par trust aur bhi complicated ho jata hai.  
👉 Isliye hume **guiding security principles** chahiye.



## 2. Two Security Principles for Trust

### A. Trust but Verify
- Matlab: **Trust karo, par verify bhi karo.**  
- Entity = User ya System dono ho sakte hain.  
- Verification = Logging + Monitoring.  

#### Example:
- User ke sab Internet pages manually review karna → not practical.  
- Solution: Automated tools ka use:  
  - Proxy  
  - Intrusion Detection System (IDS)  
  - Intrusion Prevention System (IPS)  



### B. Zero Trust
- Core idea: **Trust itself is a vulnerability.**  
- Teaching: **"Never trust, always verify."**  
- Har entity ko adversarial maana jata hai unless proven otherwise.  

#### Characteristics:
- Trust **nahi di jati** based on:
  - Device location (internal network hone ke bawajood)  
  - Device ownership (enterprise-owned hone ke bawajood)  

- Har access ke liye:
  - **Authentication + Authorization** zaroori hai.  

- Benefit:  
  - Agar breach ho bhi jaye, to damage limited & contained rahega.  



## 3. Microsegmentation (Zero Trust Implementation)
- Network ko chhote-chhote segments me todna (even **per host** level tak).  
- Har segment ke beech communication ke liye:
  - Authentication  
  - ACL (Access Control List) checks  
  - Extra security requirements  

👉 Result: Attack surface minimize + breach containment.



## 4. Limitations
- Zero Trust ko **100% apply karna possible nahi hota** → business impact ho sakta hai.  
- Lekin feasible areas me apply karna recommended hai.  



## 5. Quick Comparison Table

| Principle          | Core Idea                        | Tools/Implementation           | Limitation |
|--------------------|----------------------------------|--------------------------------|------------|
| Trust but Verify   | Trust karo, phir logs verify karo | Logs, Proxy, IDS, IPS          | Har action manually verify impossible |
| Zero Trust         | Trust = Vulnerability (Never trust) | Microsegmentation, AuthN/AuthZ | Business overhead, complexity |

---
# ⚠️ Vulnerability, Threat, and Risk


## 1. Key Definitions

### 🔹 Vulnerability
- Meaning: **Weakness** in a system.  
- "Susceptible to attack or damage."  
- Example:  
  - Glass doors & windows = Vulnerability (easily breakable).  
  - Software bug in database = Vulnerability.



### 🔹 Threat
- Meaning: **Potential danger** associated with a vulnerability.  
- Example:  
  - Glass doors can be **broken** by thief = Threat.  
  - Exploit code available for a database bug = Threat.


### 🔹 Risk
- Meaning: **Likelihood** that a threat actor will exploit a vulnerability **+ Impact** on business.  
- Example:  
  - Chances of thief actually breaking showroom glass + Business loss.  
  - Attacker using database exploit + Impact on hospital’s medical records.



## 2. Real-World Example (Showroom)

- **Vulnerability**: Glass doors/windows (weak).  
- **Threat**: Thief can break the glass.  
- **Risk**: Probability of break-in × Financial loss for showroom.



## 3. Information Systems Example (Hospital)

- **Vulnerability**: Database system has a bug.  
- **Threat**: Exploit code released (attack possible).  
- **Risk**:  
  - Attacker may use exploit → Steal/alter patient data.  
  - High business impact → Legal + operational consequences.  



## 4. Summary Table

| Term          | Meaning                                   | Example (Showroom) | Example (Hospital) |
|---------------|-------------------------------------------|---------------------|---------------------|
| Vulnerability | Weakness in system                        | Glass windows       | Bug in DB system    |
| Threat        | Potential danger using that weakness      | Thief breaking glass| Exploit code release|
| Risk          | Likelihood of exploitation + Business loss| Break-in loss       | Patient data loss   |


## 5. Formula (Simple)

Risk = Threat × Vulnerability × Impact
