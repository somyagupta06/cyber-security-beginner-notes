# Simple guide to MITRE & ATT&CK 

> Short goal: Make these notes super simple so you can *easily* understand and remember.  
> Everything below is plain Markdown (put this straight into a GitHub file). Commands or one-line examples are written as normal lines (no `bash` highlighting).



## 1) What is MITRE? 
MITRE is a U.S. non-profit that builds shared knowledge and tools for safety and security.  
They create open resources that help security teams (blue team, red team, analysts) understand and fight threats.



## 2) Key MITRE cybersecurity projects 

- **ATT&CK** — A catalog of real attacker behaviors: *what* attackers try to do and *how* they do it.  
- **CAR (Cyber Analytics Repository)** — Reusable detection logic and analytics recipes (how to detect attacks).  
- **ENGAGE** — Guidance and playbooks for running adversary emulation and purple-team exercises.  
- **D3FEND** — A mapping of defensive techniques to attack techniques (how defenses map to attacks).  
- **AEP (ATT&CK Emulation Plans)** — Step-by-step plans to emulate specific adversary behavior in your environment.



## 3) Important terms 

- **APT (Advanced Persistent Threat)**  
  - Meaning: a long-term, targeted attacker group (sometimes a nation-state or well-funded group).  
  - Misleading part: *"Advanced"* doesn't always mean custom zero-day — often they use common techniques in smart combos.

- **TTP (Tactics, Techniques, Procedures)**  
  - **Tactic** = The attacker's goal (e.g., *Initial Access*, *Privilege Escalation*).  
  - **Technique** = The method used to achieve that goal (e.g., *Phishing*, *Token Impersonation*).  
  - **Procedure** = The exact steps or tools the attacker used (a recipe or script).



## 4) What is the ATT&CK framework? 
ATT&CK is a big matrix that lists **tactics** (the goals) across the top and many **techniques** under each. It is based on *real* observed attacks. Use it to:

- Map what attackers do,
- Check where your defenses cover those techniques,
- Plan red-team or blue-team work,
- Build detections and tests.



## 5) ATT&CK quick visual idea 

| Tactic (goal)        | Example Techniques (how)                           |
|----------------------|----------------------------------------------------|
| Initial Access       | Phishing, Exploit Public-Facing App                |
| Execution            | PowerShell, Scripting                              |
| Persistence          | Create Service, Scheduled Task                     |
| Privilege Escalation | Exploiting Vulnerability, Token Impersonation      |
| Lateral Movement     | SMB/Pass-the-Hash, Remote Services                 |
| Exfiltration         | Data Staged, Exfil over HTTP                       |



## 6) Example: Phishing 
- Tactic: **Initial Access**  
- Technique: **Phishing**  
- Sub-techniques: spearphishing attachment, spearphishing link, spearphishing via service  
- Procedure example: attacker sends an email with a malicious attachment that runs a downloader.  
- Simple mitigations: user training, email filtering, attachment sandboxing, MFA.



## 7) ATT&CK Navigator — how to use it (step-by-step, simple)
1. Open ATT&CK Navigator in browser.  
2. Pick a matrix (Enterprise, Mobile, etc.).  
3. Load a layer (for example, your detections or a threat-group mapping).  
4. Color-code techniques you detect vs not-detected (Navigator supports layers).  
5. Use this to see coverage gaps and plan detections or red-team tests.



## 8) CAR (Cyber Analytics Repository) — what to do with it
- CAR contains detection ideas or rules (signatures, queries, pseudo-SQL or pseudo-SIEM rules).  
- Use CAR items to jump-start your detections: take a CAR analytic, adapt to your logs (Windows, Linux, network), test it, tune it.

Example workflow:
1. Find a CAR analytic for "PowerShell download cradle".  
2. Map fields to your logging (Sysmon, Windows Event IDs).  
3. Implement in SIEM / EDR.  
4. Test with emulation (AEP or red-team).


## 9) ENGAGE — what it helps with
- ENGAGE provides playbooks and structure for running **purple-team** tests and adversary emulation.  
- Use ENGAGE when you want to practice detecting a known threat in your own environment in a safe planned way.



## 10) D3FEND — short & useful
- D3FEND lists defensive techniques and maps them to ATT&CK techniques.  
- It helps answer: *If attackers do X, what defenses can I apply?*

Example: ATT&CK technique *Credential Dumping* → D3FEND suggests defenses like Credential Protection, Memory Protection, Segmentation.



## 11) AEP (ATT&CK Emulation Plans) — quick use
- AEPs are scripts/steps to emulate a threat group’s sequence of techniques.  
- Good for testing: run an AEP in a lab to verify detections and controls.



## 12) How a blue teamer uses these resources (short checklist)
- Use **ATT&CK** to map threats and find gaps.  
- Use **CAR** to get detection logic.  
- Use **D3FEND** to pick defenses for specific techniques.  
- Use **AEP** and **ENGAGE** to test and validate.  
- Track improvements in the ATT&CK Navigator.



## 13) How a red teamer uses these resources (short checklist)
- Use **ATT&CK** to plan realistic TTPs for scenarios.  
- Use **AEP** to create emulation plans.  
- Use **Navigator** to ensure coverage of techniques you want to test.  
- After tests, share mapped results so blue can tune detections (collaboration).



## 14) Small comparison table (ATT&CK vs CAR vs D3FEND vs AEP)

| Resource   | Purpose                         | Who benefits most           |
|------------|----------------------------------|-----------------------------|
| ATT&CK     | Catalog of attacker behaviors    | Both blue & red teams       |
| CAR        | Detection recipes / analytics    | Blue team / SOC engineers   |
| D3FEND     | Defensive techniques mapping     | Blue team / architects      |
| AEP        | Emulation plans to run attacks   | Red teams / purple teams    |
| ENGAGE     | Playbooks for exercises          | Purple teams / program leads|



## 15) Quick example: mapping a phishing incident (small plan)
1. Find Phishing in ATT&CK → note sub-technique (spearphishing-link).  
2. Check CAR for an analytic that detects suspicious link clicks or redirectors.  
3. Use D3FEND to pick defenses: email filtering, link rewriting, browser isolation.  
4. Create AEP to emulate the phishing link in a safe lab.  
5. Run test, collect logs, load into Navigator, color the techniques discovered vs missed.



## 16) Study tips (how to remember)
- Start with ATT&CK tactics (learn the high-level 12–14 tactics).  
- Then learn common techniques in **Initial Access**, **Execution**, **Lateral Movement**, **Exfiltration**.  
- Practice: map a real CVE report to ATT&CK techniques.  
- Use Navigator to color your detections — visual memory helps.


## 17) Small memory aid (mnemonic for stages)
Initial → Execution → Persistence → Privilege → Move → Exfiltrate  
(Make a short phrase in your head and attach a small example to each.)

---

# MITRE CAR (Cyber Analytics Repository)

---

## 1) What is CAR?
- Full name: **Cyber Analytics Repository (CAR)**  
- Created by MITRE, connected to the **ATT&CK framework**  
- **Purpose:** A **knowledge base of analytics** (ready-made detection logic).  
- Provides:
  - Pseudocode (human-readable logic)  
  - Queries for specific tools (like Splunk, EQL)  
  - References to ATT&CK (tactics, techniques, sub-techniques)  

Think of **ATT&CK** as: *"What attackers do"*  
And **CAR** as: *"How we can detect it in logs."*



## 2) Example Analytic: CAR-2020-09-001 → Scheduled Task - File Access
- **Description:** Detects when scheduled tasks are used to access files (possible persistence or execution).  
- **References:** ATT&CK tactic + technique (e.g., *Persistence → Scheduled Task/Job*).  
- **Details Provided:**
  - Pseudocode → readable detection logic.  
  - Splunk Query → ready to run in SIEM.  
  - Mentions **Sysmon** → Windows tool for logging events.  



## 3) Analytic Implementations
CAR analytics may include multiple implementations:
- **Pseudocode** (easy to read steps)  
- **Splunk queries** (search directly in Splunk logs)  
- **EQL (Event Query Language)** — used for parsing Sysmon event data.  

👉 Example: **CAR-2014-11-004 → Remote PowerShell Sessions**  
- Provides both **pseudocode** and **EQL version** for detection.



## 4) Two useful views in CAR
1. **Full Analytic List**  
   - Shows all analytics, their available implementations (Splunk, EQL, etc.), and supported OS platform.  
2. **CAR ATT&CK Navigator Layer**  
   - Visual ATT&CK Matrix with techniques covered by CAR (highlighted in purple).  



## 5) Quick Comparison: ATT&CK vs CAR

| Aspect              | ATT&CK                               | CAR                                             |
|---------------------|---------------------------------------|-------------------------------------------------|
| Focus              | What attackers do (TTPs)              | How to detect attacker behaviors in logs        |
| Content            | Tactics, techniques, procedures       | Analytics, pseudocode, tool queries             |
| Output             | Knowledge base of attack behaviors    | Knowledge base of detections                    |
| Who uses it        | Red + Blue teams (general framework)  | Mainly Blue teams (detection engineers, SOC)    |



## 6) Why CAR is useful
- Goes **beyond** ATT&CK’s short “Mitigation” and “Detection” notes.  
- Provides **detailed, validated, and explained analytics**.  
- Directly usable for SIEM/EDR queries.  
- Helps **SOC analysts, threat hunters, detection engineers**.  
- **Not a replacement** for ATT&CK → it is an *add-on resource*.


## 7) Simple Workflow Example
1. Look up an ATT&CK technique (e.g., *Remote PowerShell Execution*).  
2. Go to CAR → find analytic **CAR-2014-11-004**.  
3. Copy pseudocode or EQL query → implement in SIEM.  
4. Collect Sysmon logs → test detection.  
5. If working → map coverage in ATT&CK Navigator.



✅ **To remember**:  
- ATT&CK = "Attacker’s playbook"  
- CAR = "Defender’s recipe book"  
---
# MITRE ENGAGE 



## 1) What is MITRE ENGAGE?
- **MITRE Engage** = A **framework** for planning and running *adversary engagement operations*.  
- Goal: Don’t just block attackers → **engage them** to **learn more** and **waste their resources**.  
- Based on two main concepts:  
  1. **Cyber Denial** → stop attackers from achieving their goals.  
  2. **Cyber Deception** → mislead attackers with fake artifacts, systems, or data.



## 2) Starter Kit
- Engage provides a **starter kit** = whitepapers, PDFs, checklists.  
- Helps security teams design **adversary engagement strategies**.  
- Useful for **blue teams**, **threat hunters**, and **deception engineers**.



## 3) The Engage Matrix
Like ATT&CK has a matrix of TTPs, **Engage has its own matrix**.  
It has **5 categories**:

| Category   | Meaning (easy) |
|------------|----------------|
| **Prepare**   | Input → actions you set up before engagement (planning, deploying deception). |
| **Expose**    | Catch adversaries when they trigger deception (alerts, detection). |
| **Affect**    | Do things that hurt adversary operations (slow them down, block them, waste time). |
| **Elicit**    | Collect intel by observing adversary behavior (learn their tools, TTPs). |
| **Understand**| Output → analyze what happened, measure success, update defenses. |



## 4) Operate vs Prepare/Understand
- **Default focus = Operate** (Expose, Affect, Elicit).  
  → Because these are the *active engagement* actions against adversaries.  
- You can also explore **Prepare** (before ops) and **Understand** (after ops) if needed.



## 5) Example Use Case (short story)
- You create a **fake database server** (deception artifact).  
- Attacker connects → **Expose** (you detect them).  
- While they waste time exploring fake data → **Affect** (slow them down).  
- You record their commands and malware → **Elicit** (learn TTPs).  
- Later, your team reviews logs and updates defenses → **Understand**.  



## 6) Difference from ATT&CK
| ATT&CK                        | ENGAGE                                      |
|-------------------------------|----------------------------------------------|
| Focus: catalog of attacker TTPs | Focus: how defenders can actively engage adversaries |
| Used for: detection, mapping, red/blue team | Used for: deception, denial, adversary engagement |
| Passive knowledge base         | Active defensive framework |



## 7) Why ENGAGE is useful
- Moves defenders from **passive blocking** → **active engagement**.  
- Lets defenders **study attackers safely**.  
- Helps **buy time**, **reduce attacker success**, and **increase defender knowledge**.  
- Complements ATT&CK (not a replacement).



✅ **Remember:**  
- ATT&CK = *What attackers do*  
- CAR = *How to detect it*  
- D3FEND = *How to defend against it*  
- ENGAGE = *How to actively interact with attackers (deny + deceive)*  
---
# MITRE D3FEND 



## 1) What is D3FEND?
- **Full name:** Detection, Denial, and Disruption Framework Empowering Network Defense.  
- Funded by the **NSA Cybersecurity Directorate**.  
- Official description: **"A knowledge graph of cybersecurity countermeasures."**  
- Status: Still **in beta** (so expect big changes in the future).  

Think of **ATT&CK** = attacker behaviors,  
and **D3FEND** = defender countermeasures.



## 2) D3FEND Matrix
- At the time of writing: **408 defensive artifacts** (techniques).  
- Each artifact is like a defensive building block.  
- Example artifact: **Decoy File**  
  - **Definition** → what this defense is.  
  - **How it works** → description of mechanism.  
  - **Considerations** → warnings / things to keep in mind when using.  
  - **Example** → real-world usage.  



## 3) Integration with ATT&CK
- You can **filter D3FEND based on ATT&CK techniques**.  
- Example: If ATT&CK shows "Credential Dumping" → D3FEND suggests "Memory Protection" or "Credential Protection" as countermeasures.  



## 4) Why D3FEND is important
- Gives **structured defensive knowledge** (not just random tips).  
- Links **attack techniques** ↔ **defense techniques**.  
- Useful for **blue teams, SOC analysts, architects**.  
- Still early stage → but will become stronger over time.  



## 5) Comparison with other MITRE tools

| Resource   | Focus Area                     | Example Usage                          |
|------------|--------------------------------|-----------------------------------------|
| ATT&CK     | Attacker TTPs (what they do)   | Map adversary behavior                  |
| CAR        | Detection logic                | Find Splunk/EQL queries for ATT&CK TTPs |
| ENGAGE     | Adversary engagement (deny/deceive) | Plan deception operations           |
| D3FEND     | Defensive countermeasures      | Pick defenses against ATT&CK techniques |



## 6) Example workflow with ATT&CK + D3FEND
1. ATT&CK → Attack technique: **Phishing (Initial Access)**  
2. D3FEND → Defensive countermeasures: **Email Filtering, Link Protection, Browser Isolation**  
3. Team picks → Email gateway rules + sandbox attachments.  
4. Defender maps: Attack (red) → Defense (blue).  



✅ **Remember:**  
- **ATT&CK** = “What attackers do”  
- **D3FEND** = “What defenders can do”  
---

# MITRE ENGENUITY, CTID, and ATT&CK® Emulation Plans

> Short goal: Make this easy to read and drop straight into a GitHub file.  
> Commands or one-line examples written on normal lines (no bash fenced blocks).



## 1) What is MITRE Engenuity?
- **MITRE Engenuity** is the innovation arm of MITRE that runs projects and programs building advanced tools and community resources for threat-informed defense.  
- It produces additional collections and labs that extend ATT&CK and related MITRE work (for example — Adversary Emulation Library and ATT&CK Emulation Plans).



## 2) What is CTID (Center for Threat-Informed Defense)?
- **CTID** = Center for Threat-Informed Defense.  
- It is a consortium of companies and vendors collaborating with MITRE to research adversary tradecraft and publish practical resources.  
- Members include security vendors and big tech (examples: AttackIQ, Microsoft, Red Canary, Verizon, Splunk — participants vary over time).  
- Goal: Share datasets, emulation plans, tools, and methods publicly so defenders everywhere can improve.



## 3) Adversary Emulation Library (what it is)
- A public library of **adversary emulation plans** (step-by-step guides to mimic real threat groups).  
- Created and maintained as part of CTID / Engenuity contributions.  
- Purpose: Let red teams and purple teams run realistic emulations of known APTs or threat groups against their environment.



## 4) ATT&CK® Emulation Plans (how to use them)
- Each emulation plan maps a threat group’s behaviors to ATT&CK techniques and gives practical steps to emulate those TTPs safely.  
- Example emulation plans commonly available: **APT3**, **APT29**, **FIN6** (these are example names — library contains multiple plans).  
- Use case: If leadership asks “How would we fare against APT29?”, run the APT29 emulation plan in a controlled lab to measure detection and response.



## 5) Why emulation plans matter (quick reasons)
- They are **realistic** — based on observed adversary behavior.  
- They are **repeatable** — teams can rerun the same plan to measure improvements.  
- They are **collaborative** — blue and red teams use the same plan to communicate findings.  
- They provide **measurable results** for leadership and security program decisions.


## 6) Simple workflow to use an emulation plan
1. Pick an emulation plan (for example: APT29).  
2. Review the mapped ATT&CK techniques and required tools/abilities.  
3. Prepare a safe test environment (segmented lab or approved production-sim environment).  
4. Run the plan step-by-step (ideally using automation where safe).  
5. Collect logs and telemetry (Sysmon, EDR, network sensors, SIEM).  
6. Map detections to ATT&CK + CAR analytics; identify gaps.  
7. Tune detection rules, defenses, and repeat to verify improvements.  
8. Produce a short results summary for stakeholders (what detected, what missed, recommended fixes).



## 7) Quick comparison — where these pieces fit

| Resource                     | Main focus                                | Who uses it                     |
|------------------------------|-------------------------------------------|----------------------------------|
| ATT&CK                       | Catalog of attacker TTPs                  | Red & Blue teams                 |
| CAR                          | Detection analytics / queries             | Detection engineers, SOC         |
| D3FEND                       | Defensive countermeasures                 | Architects, blue teams           |
| ENGAGE / D3FEND / Emulation  | Active engagement & deception / defenses  | Purple teams, deception engineers|
| Adversary Emulation Library  | Step-by-step emulation plans               | Red teams, purple teams, testing |


## 8) Practical tips before running an emulation
- Always get **approval** from leadership (CISO / relevant owner).  
- Run emulations in a **segmented lab** or with explicit production exceptions.  
- Use **AEPs (ATT&CK Emulation Plans)** when available — they are structured and mapped.  
- Collect telemetry first — if you have no logs, you cannot measure results.  
- Automate replay where safe (reduces manual error and improves repeatability).



## 9) Short example — “How to test if we detect APT29”
1. Choose the APT29 emulation plan from the Adversary Emulation Library.  
2. Read the mapped ATT&CK techniques and required artifacts/tools.  
3. Prepare a test VM with Sysmon + EDR agent + SIEM ingestion.  
4. Execute the emulation plan steps that are safe for your environment.  
5. Review SIEM alerts and map to ATT&CK; mark what was detected and what was missed.  
6. Use CAR to find detection analytics you can adapt and implement.  
7. Re-run the plan after tuning to verify improvement.



## 10) Filename suggestion for GitHub
Save this as `MITRE-Engenuity-CTID-Emulation-Plans.md`.
---

# Simple Guide — Using ATT&CK for Threat Intelligence 

> Goal: Make threat intelligence (TI / CTI) useful and *actionable*.  
> This file is GitHub-ready Markdown. Put it in a repo as `Threat-Intel-ATTACK-QuickGuide.md`.



## What is Threat Intelligence (TI / CTI)? — Very simple
- **TI = information about attackers** — who they are, what they do, and how they work (their TTPs).  
- **Why it matters:** With TI, defenders can make better choices — where to add protections, what to monitor, and what to test.


## Two types of organizations and TI
1. **Large orgs** — may have a full TI team that collects, analyses, and shares intel.  
2. **Small/mid orgs / multitask defenders** — people wear many hats. They need quick, usable TI that they can apply in hours or days.


## Short definition of key terms (easy)
- **TTPs** = Tactics, Techniques, Procedures (how attackers behave).  
- **ATT&CK** = a catalog of attacker behaviors (what they do).  
- **CAR** = detection recipes (how to search for those behaviors in logs).  
- **D3FEND** = defensive techniques / countermeasures (what to do to stop or slow them).  
- **Emulation Plans / AEP** = step-by-step scripts to mimic real attackers in your environment.



## Quick one-line mental model
- ATT&CK = *What attackers do*  
- CAR = *How to detect it*  
- D3FEND = *How to defend against it*  
- Emulation = *Test it in a safe way*



## Step-by-step: Turn TI into action (easy steps)
1. **Pick your threat focus** (example: aviation org moving to cloud).  
2. **Choose priority groups** that target your sector (e.g., APTs known to hit aviation or cloud).  
3. **Open the ATT&CK group pages** for those groups and list their techniques.  
4. **For each technique, pick required telemetry** (what logs/events you need).  
5. **Search CAR** for analytics that match those techniques (copy pseudocode / queries).  
6. **Map defenses** using D3FEND (what countermeasures apply).  
7. **Run an emulation** (AEP) in lab to validate detections.  
8. **Fix gaps** (add logs, tune detections, strengthen controls), then repeat.



## Example scenario (aviation → cloud) — short and concrete
1. **Threat focus:** attackers that target airlines and cloud systems.  
2. **Pick groups to check:** APT33 (aviation history), APT10 (MSP/cloud), Scattered Spider (criminals targeting airlines).  
3. **Common techniques to look for:** phishing, valid accounts, service principal misuse, remote PowerShell, scheduled tasks, large cloud data downloads.  
4. **What telemetry you need:** Cloud audit logs (CloudTrail/AzureActivity), identity logs (Azure AD/Okta), EDR / Sysmon, SIEM network logs.  
5. **Actions:** add missing logs, import CAR analytics, run an APT emulation in lab, update rules.



## Small table: ATT&CK → CAR → D3FEND (example mapping)

| ATT&CK Technique (example)     | CAR Analytics (detection idea)        | D3FEND Countermeasure (defense)         |
|--------------------------------|---------------------------------------|-----------------------------------------|
| Phishing (spearphishing-link)  | Detect suspicious link clicks         | Email filtering, link rewriting, sandbox|
| Remote PowerShell              | EQL/Splunk rule for PowerShell download | Execution prevention, PowerShell logging|
| Scheduled Task persistence     | CAR-2020-09-001: Scheduled Task File Access | Restrict task creation, monitoring     |
| Compromised service principal  | Alert on new service principal usage  | Least-privilege, rotate keys, logging   |



## Cheat-sheet checklist (copy + use)

Identity & Cloud:
- [ ] CloudTrail / AzureActivity / GCP audit logs forwarded to SIEM.  
- [ ] Alert for new service principal or new API key creation.  
- [ ] MFA & conditional access for admin/service accounts.

Endpoint & Host:
- [ ] Sysmon + EDR on critical hosts for process creation and network events.  
- [ ] Rules for PowerShell, WMI, scheduled tasks.

Network & Data:
- [ ] Monitor large downloads from cloud storage.  
- [ ] Inspect unusual cross-account cloud activity.

People & Process:
- [ ] Phishing simulation focused on IT/help-desk.  
- [ ] MSP/third-party access monitoring & contracts requiring logs.

Testing:
- [ ] Pick an AEP (emulation plan) and run it in a lab.  
- [ ] Map results to ATT&CK + CAR + D3FEND → then patch the gaps.



## Short example: How to evaluate a group (quick)
1. Go to the ATT&CK group page (example: APT10).  
2. Export or copy the list of techniques.  
3. For each technique write two columns: *Telemetry needed* and *Possible CAR analytic*.  
4. Mark YES/NO if you have that telemetry today.  
5. Prioritize fixes for techniques that lead to cloud persistence or data exfiltration.



## Mini-template: Group → Gap matrix (copy into Excel or Markdown table)

| Technique | Telemetry needed | We have telemetry? (Y/N) | CAR analytic available? (Y/N) | Mitigation (D3FEND) | Priority (High/Med/Low) |
|-----------|------------------|--------------------------|-------------------------------|----------------------|-------------------------|
| Phishing  | Email gateway logs, web proxy | Y / N | Y / N | Email filtering, sandbox | High |



## Easy memory tips
- Start with high-level **tactics**: Initial Access, Execution, Persistence, Privilege Escalation, Lateral Movement, Exfiltration, Impact.  
- Learn 3–5 common techniques for each tactic (e.g., phishing, PowerShell, scheduled tasks, credential dumping, SMB lateral move).  
- Use visuals (ATT&CK Navigator) to color detected vs not detected — visuals stick.



## Final short checklist before you report to leadership
- [ ] I chose the threat groups relevant to our sector.  
- [ ] I mapped their ATT&CK techniques.  
- [ ] I checked telemetry and filled gaps (Cloud + Identity + EDR).  
- [ ] I implemented or adapted CAR analytics for top 5 techniques.  
- [ ] I planned or ran an emulation to validate.  
- [ ] I have an action list (what to fix first).



## Want this as files?
You can save this as:
- `Threat-Intel-ATTACK-QuickGuide.md` (this file)  
- `Threat-Intel-Checklist.md` (the checklist only)  
- `Group-Gap-Matrix-template.csv` (CSV from the mini-template)

---

