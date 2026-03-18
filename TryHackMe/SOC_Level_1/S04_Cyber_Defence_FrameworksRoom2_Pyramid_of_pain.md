# Pyramid of Pain (for Threat Hunters / IR / SOC Analysts)

## What is the Pyramid of Pain?
The Pyramid of Pain shows which indicators (IOCs) cause the most trouble for an attacker when we detect or block them.
- Bottom = easy for defenders to detect but also easy for attackers to change (low pain).
- Top = hard for defenders to detect but very costly for attackers to change (high pain).

Higher = more attacker pain. Aim to move your detections up the pyramid.

## Pyramid levels (bottom → top)
1. Hash values (file hashes)
2. IP addresses
3. Domain names / URLs
4. Network / Host artifacts (registry keys, mutexes, specific file paths, YARA rules)
5. Tools (specific malware families, unique utilities)
6. TTPs (Tactics, Techniques, Procedures — attacker behavior)

## Short glossary of common hash algorithms
MD5
- 128-bit output (32 hex characters)
- Fast; NOT cryptographically secure; collisions known

SHA-1
- 160-bit output (40 hex characters)
- Deprecated for security uses; collisions known

SHA-2 (example: SHA-256)
- 256-bit output (64 hex characters)
- Stronger, widely used (recommended over MD5/SHA-1)

## Why file hashes are weak as the only IOC
- A hash identifies an exact set of bytes. Change 1 byte → new hash.
- Attackers can trivially change a file (append a string, recompile, pack, encrypt) and the hash changes.
- Hash hunting finds exact matches only; it misses polymorphic or modified samples.
- Use hashes as one signal among many (file behaviour, signatures, network patterns, YARA, sandbox results).

## Practical example — change file hash by appending text (Windows PowerShell style)
Get-FileHash .\OpenVPN_2.5.1_I601_amd64.msi -Algorithm MD5

echo "AppendTheHash" >> .\OpenVPN_2.5.1_I601_amd64.msi

Get-FileHash .\OpenVPN_2.5.1_I601_amd64.msi -Algorithm MD5

(You will see the MD5 value change after the echo step. That demonstrates how trivial changes break hash-based detection.)

## Comparison table — Ease for defender vs cost to attacker

| IOC type             | Easy for defender to use?   | Cost for attacker to change? |
|------|------|----|
| Hash                 | Very easy                   | Very low                     |
| IP address           | Easy                        | Low                          |
| Domain/URL           | Easy–medium                 | Low–medium                   |
| Network/Host artifact| Medium                      | Medium                       |
| Tools (malware)      | Medium–hard                 | High                         |
| TTPs                 | Hard                        | Very high                    |


## How to use hashes effectively (practical tips)
- Use hashes for quick lookup (VirusTotal, MetaDefender) and rapid triage.
- Combine with behavioral indicators (process trees, network connections, command lines).
- Create YARA rules or fuzzy signatures for families instead of relying on single hashes.
- Record hashes with context: filename, observed path, process parent, network destinations.
- Monitor changes: identical hashes across many systems may indicate reuse; differing hashes with similar behaviour may indicate polymorphism.

## Short hunting playbook (hash-aware)
1. If you get a hash alert:
   - Lookup hash on VirusTotal / MetaDefender / internal repo
   - Pull sandbox or EDR behavioural output (processes, network calls)
   - Check related IOCs (domains, IPs, mutexes, registry keys)
2. If behaviour matches known malware family:
   - Create detection rules based on behaviour (YARA, Sigma, EDR rules)
   - Search for related TTPs across telemetry
3. If only hash match with no behaviour:
   - Mark as low-confidence unless corroborated by other signals

## Quick examples of detection rule ideas (conceptual)
- If a file with hash X executes and connects to domain Y within 60 seconds → high priority
- If a process creates mutex named Z and spawns PowerShell with encoded command → investigate
- If many machines download same filename but different hashes and all contact same IP → hunt for C2 behavior

## TL;DR (one-liner)
Hashes are useful for quick lookups and triage, but they are brittle — rely on behaviour, artifacts, tools, and TTPs to cause real "pain" for attackers.

---

# Pyramid of Pain — IP Addresses (Green Level)

## Why IP addresses matter
- Every device on a network has an IP address (desktops, servers, CCTV cameras, etc.).
- Security teams use IP addresses as Indicators of Compromise (IOCs).
- A common defense = block or deny inbound/outbound traffic from malicious IPs using firewalls.

## Why the color green?
- In the Pyramid of Pain, IPs are marked green.
- Green = useful but not very painful for attackers if blocked.
- Reason: attackers can easily switch to another IP (new VPS, proxy, compromised machine).

## Value for defenders
- Knowing an adversary’s IPs helps with:
  - Quick blocking at perimeter firewall
  - Identifying infected hosts communicating with known bad IPs
  - Feeding IPs into threat intel platforms and SIEM rules
- BUT → attackers can bypass this fast.

## How attackers evade IP-based blocking
- **Simple IP change**: attacker just spins up a new server → new IP.
- **Botnets**: huge pool of infected devices with unique IPs.
- **Fast Flux technique**:
  - Domain name points to many IPs.
  - These IPs change rapidly (DNS round-robin + short TTL).
  - Makes it very hard to block by IP only.
  - Purpose: hide real Command & Control (C2) servers.
  - Used in phishing, malware delivery, and C2 comms.

## Example: Fast Flux (easy analogy)
Imagine:
- Normal website: `bank.com → 1 fixed IP`
- Fast Flux website: `evil.com → IP1, IP2, IP3, IP4...` (rotates constantly)
- Even if you block IP1, attacker still has IP2, IP3, etc.  
→ Defender headache!

## Tools to analyze malicious IPs
- **app.any.run** (interactive malware sandbox)
- **VirusTotal** (IP reputation lookups)
- **AbuseIPDB** (reports on abusive IPs)
- **Threat Intel Feeds** (Cisco Talos, AlienVault OTX)

## Pros and cons of IP IOCs
+ Easy to block (firewall, IPS, proxy)
+ Useful for quick containment
- Very easy for attackers to rotate/change
- False positives possible (shared hosting, NAT)

## Comparison Table


| IOC Type         | Defender Effort            | Attacker Effort to Evade  |
|------------------|----------------------------|---------------------------|
| IP Address       | Easy to detect & block     | Easy to change/rotate     |


## TL;DR
- IP addresses are a **green-level IOC** in the Pyramid of Pain.
- They give defenders quick wins but attackers can easily bypass.
- Fast Flux networks make IP blocking much less effective.
- Best practice: use IPs as one data point, combine with domains, artifacts, and behaviors for strong detections.
---

# Pyramid of Pain — Domain Names (Teal Level)

## Why Domain Names matter
- Domains map human-readable names → IP addresses.
- Attackers often rely on domains for Command & Control (C2), phishing, malware delivery.
- Defenders can detect/block malicious domains in **DNS logs, proxy logs, and web server logs**.

## Why teal?
- Domains are **harder for attackers to replace** compared to IPs.
- They must:
  - Buy/register the domain
  - Set up DNS records
  - Update infrastructure
- BUT → many DNS providers have weak policies, APIs make automation easy.

## Attacker tricks with domains
### 1. **Punycode attacks**
- Converts Unicode → ASCII for domain names.
- Example:
  - Legit: adidas.de
  - Malicious lookalike: adıdas.de → punycode = xn--addas-o4a.de
- Looks visually similar, tricks users into visiting fake websites.

### 2. **URL Shorteners**
- Attackers hide malicious destinations behind shortened links.
- Common services:
  - bit.ly
  - goo.gl
  - ow.ly
  - s.id
  - smarturl.it
  - tiny.pl
  - tinyurl.com
  - x.co
- **Tip**: Add “+” to the short URL in browser to preview final destination.

### 3. **Fast Flux with domains**
- Same as IP level: domains resolve to rapidly rotating IPs.
- Makes domain blocking less effective.

## Detection tips for defenders
- Monitor DNS logs → detect requests to suspicious domains.
- Use proxy/web server logs → detect unusual HTTP requests.
- Compare against threat intelligence feeds (e.g., domain blacklists).
- Watch for:
  - New domains with low reputation
  - Domains with strange character sets (IDN homographs)
  - Domains resolving to many changing IPs (Fast Flux)

## Using sandbox tools (example: any.run)
- **Networking tab** → shows HTTP requests, DNS lookups, and connections during malware execution.
- **HTTP Requests tab** → shows resources retrieved (e.g., droppers, callbacks).
- **Connections tab** → shows C2 communications (file uploads/downloads, FTP, etc.).
- **DNS Requests tab** → shows domains malware queries (helps spot beaconing or sandbox detection evasion).

## Pros and cons of Domain IOCs
+ Harder for attackers to swap than IPs
+ Easier for defenders to monitor in logs
- Still replaceable (cheap domains, APIs, Fast Flux)
- Attackers can hide behind URL shorteners or punycode

## Comparison table


| IOC Type         | Defender Effort            | Attacker Effort to Evade  |
|------------------|----------------------------|---------------------------|
| Hash             | Very easy                  | Very low                  |
| IP address       | Easy                       | Low                       |
| Domain name      | Medium                     | Medium                    |


## TL;DR
- Domain names are **teal-level IOCs** in the Pyramid of Pain.
- They are harder to swap than IPs, but attackers still have tricks (Punycode, shorteners, Fast Flux).
- Defenders should combine domain monitoring with DNS/proxy log analysis and TI feeds for better detection.
---
# Pyramid of Pain — Network & Host Artifacts (Yellow Level)

## What are Network & Host Artifacts?
Host artifacts = traces attackers leave on a system.  
These are more stable than IPs/domains and cause more “pain” for attackers because changing them usually means reworking tools or payloads.

Examples:
- Registry values created or modified by malware
- Suspicious process executions / unusual parent-child process trees
- Files dropped in unusual locations or with odd names
- Mutexes created by malware
- Specific command-line arguments used by a payload
- Services installed, scheduled tasks created
- File creation/modification patterns, suspicious Windows Event IDs, or Sysmon events

## Why this level hurts attackers more
- Artifacts are often tied to how the malware works (persist, communicate, escalate).
- To evade detections that look for these artifacts they must change code/behaviour — not just infrastructure — which is time-consuming and error-prone.

## Where to find these artifacts
- EDR/EDR sensor telemetry (process trees, file writes, registry writes)
- Sysmon logs (Process Create, File Create, Network Connect, Registry changes)
- Windows Security/Event logs (e.g., Process Creation events)
- File system monitoring and SIEM (search for file create/modify patterns)
- Process memory dumps and sandbox behavioral reports

## Practical examples (plain lines, GitHub-style raw text)
Process creation detection example:
- Parent: winword.exe
- Child: cmd.exe /c powershell -nop -w hidden -enc <base64>
- Suspicious because Office spawned a shell to run encoded PowerShell

File-dropped example:
- C:\Users\Public\temp\update32.exe created after document open

Registry persistence example:
- HKCU\Software\Microsoft\Windows\CurrentVersion\Run\Updater = "C:\Users\Public\temp\update32.exe"

Mutex example:
- Mutex named Global\{random-guid}-lock created by the payload

Event log checks (search these concepts in your logs):
- Process creation events showing unusual parent-child combos
- File creation events in uncommon directories
- New service installation or scheduled task creation
- Large number of failed logon events combined with unusual process activity

## Hunting & detection tips (practical)
1. **Process ancestry rules**
   - Flag when Office products spawn cmd/powershell/wscript/cscript.
2. **Command-line detection**
   - Look for base64-encoded PowerShell, suspicious flags (-enc, -nop, -w hidden).
3. **Path whitelist/blacklist**
   - Alert on executables running from %AppData%, %Temp%, or user profile folders.
4. **Registry monitoring**
   - Watch Run/RunOnce, Services, and common persistence keys for new entries.
5. **File creation patterns**
   - Alert when files with EXE/DLL extensions are created by Office / browsers.
6. **Correlate network + host artifacts**
   - If a new file is created and immediately initiates an outbound connection → high priority.
7. **Use YARA + Sigma**
   - YARA for file/artifact patterns; Sigma for translating host detection logic to EDR/SIEM rules.
8. **Baseline & anomaly**
   - Build a baseline of normal process-parent relationships, then flag deviations.

## Example Sigma-style rule idea (conceptual, not full syntax)
- Title: Word spawning PowerShell with encoded command
- Condition: process_name == "winword.exe" AND child_process_name == "powershell.exe" AND command_line contains "-enc" OR "-nop"
- Action: Alert high, isolate host

## Best practices
- Don’t rely solely on a single artifact — correlate multiple artifacts (process, file, registry, network).
- Record context: user, parent process, timestamps, hashes, and destination IPs/domains.
- Turn high-confidence artifact detections into containment playbooks (isolate host, collect memory, block C2).
- Convert repeated patterns into YARA rules or EDR detections to catch variants.

## Pros & cons (quick)
+ Harder for attacker to change than IP/domain  
+ Tends to reveal actual attacker technique or persistence method  
- Detection requires good telemetry (EDR, Sysmon, logging)  
- More effort to tune (avoid false positives from legit admin tools)

## TL;DR
Network & Host Artifacts are a yellow-level IOC — more painful for attackers because they reflect how malware *behaves* on hosts. Detecting them requires better telemetry and correlation, but when you catch them you force attackers to redesign their tools and workflows.

---
# Pyramid of Pain — Network Artifacts (Yellow Level)

## What are Network Artifacts?
Network artifacts are observable patterns in network traffic that attackers use for C2, data exfiltration, or delivery.  
Examples:
- User-Agent strings
- HTTP URI / path patterns
- POST request bodies or specific parameters
- Custom headers
- C2 protocols, ports, and beacon intervals
- TLS fingerprints (JA3), certificates

These are in the **yellow** zone — harder for attackers to change than IPs/domains and cause more disruption if detected.

## Why network artifacts matter
- They reflect *how* the attacker communicates.
- Changing them often requires modifying the malware/tool behavior (not just infrastructure).
- Detecting a consistent artifact (custom header, UA, URI pattern) lets you block or hunt for related activity across the network.

## Where to look
- Proxy logs / web server logs / WAF logs
- Packet captures (PCAPs) viewed with Wireshark/TShark
- IDS/IPS logs (Snort, Suricata)
- EDR network telemetry
- TLS inspection and JA3/JA3S fingerprinting systems

## Triage & capture example (TShark)
Use TShark to extract HTTP host and User-Agent from a PCAP:
tshark --Y http.request -T fields -e http.host -e http.user_agent -r analysis_file.pcap

(That command prints host + user-agent per HTTP request found in analysis_file.pcap.)

## Common suspicious indicators (examples)
- Unusual User-Agent strings that do not match known browser UAs or your baseline
- Repeated short-interval HTTP POSTs to the same URI with small payloads (beaconing)
- Long, encoded POST bodies, or POSTs containing predictable parameter names (e.g., `id=`, `tok=`, `data=`, base64 strings)
- Nonstandard headers (X-Custom, X-Auth) used consistently by malware
- JA3/JA3S TLS fingerprint observed across many hosts contacting odd domains or IPs

## Emotet-style User-Agent examples (illustrative)
- Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1)
- Mozilla/5.0 (Windows NT 10.0; Win64; x64)
- Opera/9.80 (Windows NT 6.0; U; en) Presto/2.12.388 Version/12.14
(Attackers often reuse older or uncommon UAs to blend or as an artefact of their downloader code.)

## Detection examples

### TShark extraction (repeat)
tshark --Y http.request -T fields -e http.host -e http.user_agent -r analysis_file.pcap

### Snort/Suricata example rule idea (conceptual)
alert http any any -> any any (msg:"Suspicious UA - custom bad UA"; http.user_agent; content:"BadUAStringHere"; sid:1000001; rev:1;)

### Sigma rule idea (conceptual)
- Title: Suspicious HTTP User-Agent seen in proxy logs
- Condition: http.user_agent IN (list_of_bad_uas) AND destination_not_in_corporate_allowlist
- Action: Alert + create investigation ticket

### Simple hunting steps
1. Extract user-agent strings from proxy or PCAPs.
2. Group by frequency and destination domain/IP.
3. Baseline normal / approved UAs for your environment (browsers, automated scanners, internal apps).
4. Flag UAs not in baseline, and cross-check for:
   - Repeated POSTs to same URI
   - Small consistent payloads (beacon)
   - Same JA3 fingerprint across multiple hosts
5. If suspicious, follow host → isolate if high-confidence: collect PCAP, host process tree, memory image.

## Quick examples of network artifact detections (plain text)
- HTTP POST /api/update.php with body "id=<hex>&st=<base64>" observed from 5 hosts to domain bad.example.com
- User-Agent: "curl/7.29.0" coming from multiple user workstations (no legitimate curl usage) → investigate
- TLS JA3 fingerprint abcdef12345 seen connecting to known-malicious domain → hunt

## Best practices
- Maintain a whitelist of known automated services (patch managers, monitoring, scanners).
- Keep an allowlist and baseline of common User-Agents used by your environment.
- Correlate network artifacts with host artifacts (process that initiated the connection, file written).
- Use JA3/JA3S fingerprints for TLS-encrypted C2 detection where possible.
- Create detections for both exact matches and fuzzy patterns (URI regex, header presence).

## Pros & cons
+ Harder for attacker to change than IPs/domains  
+ Often indicates actual attacker behavior (beaconing, C2)  
- Requires good telemetry (proxy, PCAP, IDS)  
- False positives if baseline not well-defined (legit automation may use odd UAs)

## TL;DR
Network artifacts (User-Agent, URIs, POST bodies, JA3) live in the yellow zone — detecting them forces attackers to change code/behavior, giving defenders time to act. Use TShark, IDS, proxy logs, and correlation with host telemetry to turn these artifacts into high-confidence detections.
---
# Pyramid of Pain — Tools (Challenging Level)

## What are Tools?
Tools = the actual utilities, custom EXEs/DLLs, maldocs, implant frameworks, password crackers, and toolsets attackers use.  
At this level the defender causes **real pain**: replacing or re-writing tools is costly for the attacker (time, money, skill).

## Why this is challenging for attackers
- Tools embed behaviour, APIs, libraries, artifacts and sometimes unique code patterns.
- To evade detections here, attackers must rebuild or significantly change their tool, which is expensive and slow.
- Good detections here often mean game-over for that campaign.

## Where to get samples & community detections
- MalwareBazaar (malware samples & meta)
- Malshare (malicious sample repository)
- VirusTotal (hashes, relationships, VT intelligence)
- SOC Prime Threat Detection Marketplace (shared detection rules)
- YARA rule repositories (community-contributed YARA)
- Your internal malware collection and EDR sample store

## Detection techniques that work well
- **YARA rules** — pattern-based file matching (strings, PE imports, magic bytes, code patterns)
- **Antivirus signatures / EDR signatures** — behavioural and static rules deployed to endpoints
- **Fuzzy hashing (similarity)** — SSDeep to detect variants with minor changes
- **Behavioral sandboxing** — dynamic behaviour detection (APIs used, dropped files, registry changes)
- **Static analysis** — import tables, resource strings, packer detection, section anomalies
- **Telemetry correlation** — file seen + unique persistence + C2 pattern → high confidence

## Practical YARA example (simple, real-style)
rule Stealer_Sample
{
    meta:
        author = "analyst"
        description = "Detects stealer-like binary by strings and PE imports"
        date = "2025-09-26"
    strings:
        $s1 = "Stealer" ascii
        $s2 = "GetProcAddress" ascii
        $s3 = "InternetOpenA" ascii
    condition:
        uint16(0) == 0x5A4D and any of ($s*) and pe.imports("ws2_32.dll", "connect")
}

## YARA tips
- Use the PE module to check imports, entrypoint, section names, and section entropy.
- Prefer multiple corroborating strings or structural checks — avoid one-line string-only rules to reduce false positives.
- Test rules on benign corpora to measure false positives before deployment.

## Fuzzy hashing (SSDeep) — concept and how to use it
- Fuzzy hashing produces a signature that allows similarity scoring between two files (score usually 0–100).
- Use fuzzy hashing to catch variants that differ by small changes (packer offsets, appended bytes, minor recompiles).
- Typical workflow:
  1. Generate fuzzy hash for known sample and store it in your malware repo.
  2. For a new sample, compute its fuzzy hash and compare against repo.
  3. If similarity > threshold (e.g., 60–80) → treat as likely variant and investigate.

## Example SSDeep workflow (conceptual lines, use your SSDeep tool)
- Generate fuzzy hash for a sample and save to database
- When new sample arrives, compute its fuzzy hash and compare to database to get similarity score
- If similarity_score >= 60 then escalate for manual review

## Detection rule ideas (EDR / SIEM / Sigma conceptual)
- If file with fuzzy similarity > 60 to known-malicious family executes AND it creates persistence registry keys → Alert and isolate host
- If new EXE named "Stealer.exe" is created in %TEMP% AND spawns network connections to known-bad domains → High priority incident
- Convert high-confidence YARA hits into EDR indicators and block on execution path or hash (with caution)

## Hardening & response playbook (Tools level)
1. Collect sample and compute: MD5/SHA256 + fuzzy hash + YARA match results + sandbox behaviour.
2. Correlate sample with network telemetry (domains, IPs, JA3).
3. Create EDR detection: block on behaviour (for example: creation of persistence + outbound to C2 domain), not just hash.
4. Publish YARA/EDR rules to detection platforms (SOC Prime, internal repos).
5. Hunt for related artifacts across telemetry using YARA, fuzzy-hash similarity, and behavioural signatures.
6. If confirmed, perform containment: isolate hosts, collect images, rotate secrets, patch vulnerable services.

## Examples of useful signatures / rule targets
- Unique resource string or error text embedded in the binary
- Unusual section names (e.g., .UPX1 but not actually packed)
- Import set that consistently appears in the family (wininet, ws2_32, advapi32)
- Specific sequence of API calls observed in sandbox traces
- Consistent persistence mechanism (same registry key or scheduled task name)

## Using community resources safely
- Download samples only in controlled environments (air-gapped lab, sandbox).
- Use nightly feeds (MalwareBazaar, Malshare) to enrich detection signatures.
- Vet community YARA rules on safe corpora to avoid false positives.

## Quick reference — pros & cons

+ Pros:
  - Very high attacker pain — forces tool rework
  - Detects families & variants using fuzzy or behavioural signatures
  - Good for proactive hunting and blocking

- Cons:
  - Requires skilled reverse engineering or good sandbox telemetry
  - Risk of false positives if rules are sloppy
  - Requires safe lab infrastructure for analysis

## TL;DR
At the Tools level you move from blocking infrastructure to breaking the attacker’s weaponry. Use YARA, fuzzy hashing (SSDeep), behavioural sandboxing, and community detection marketplaces to find and stop tool families — this forces attackers to rewrite or buy new tools, which is expensive and slow.

# End of Tools notes
---
# Pyramid of Pain — TTPs (Apex / Red Zone)

## What are TTPs?
TTPs = Tactics, Techniques, and Procedures.  
They describe *how* adversaries operate (the behavior, not just the things they use).  
This is where the MITRE ATT&CK matrix lives (reconnaissance → initial access → execution → persistence → privilege escalation → lateral movement → exfiltration → impact).

## Why TTPs are the most painful for attackers
- TTPs reflect attacker *behavior and intent*.  
- To evade TTP detections, attackers must change their operational methods, retrain operators, or redesign campaigns — expensive and slow.  
- Detecting TTPs early short-circuits campaigns (stop reconnaissance, kill lateral movement, block exfil).

## Examples of TTP-level detections (behavioral)
- Unusual authentication patterns: many small authentication attempts from a single host to many resources (credential stuffing / brute force).
- Pass-the-Hash / Pass-the-Ticket: authentication events showing NTLM hashes used across multiple hosts or suspicious Kerberos anomalies.
- Lateral movement pattern: host A created RDP sessions to host B, then host B executes PsExec to host C.
- Data staging + outbound bulk transfer after unusual privilege escalation — exfil pipeline behavior.
- Living-off-the-land sequencing: Office document → macro → powershell encoded → rundll32 loading a DLL → network beaconing → scheduled task creation.

## How to detect TTPs (high-level approach)
1. **Baseline & profiling**
   - Baseline normal user and service behavior (logon patterns, process families, data access volumes).
2. **Collect broad telemetry**
   - Endpoint process trees, command lines, file events, registry, network flows, DNS, proxy logs, auth logs, SIEM events.
3. **Correlate across domains**
   - Link host, network, and identity signals (example: same account performing odd access + host shows persistence artifacts + outbound beaconing).
4. **Map to ATT&CK**
   - Translate detections to ATT&CK techniques; this helps standardize detection, measurement, and response.
5. **Use sequence detection**
   - Detect sequences of events (not just single IOCs): e.g., suspicious download → new service installed → outbound to new domain within X minutes.
6. **Threat hunting**
   - Hunt proactively for technique patterns (e.g., lateral movement chains, specific privilege escalation footprints).
7. **Behavioral ML / analytics**
   - Use anomaly detection for uncommon sequences that humans may miss (with caution — tune for false positives).

## Practical detection ideas (conceptual)
- Create detection rules that trigger on **chains**: Process spawn patterns + persistence creation + network beaconing within a time window → high-confidence alert.
- Monitor for **credential anomalies**: logins from geographically impossible locations, rarely used accounts accessing sensitive systems, or rare admin tools being used by non-admin accounts.
- Watch for **tooling sequences**: e.g., use of `schtasks` + creation of suspicious scheduled task + execution of signed-but-weird binary.
- Enforce EDR behaviors: block execution of unsigned binaries in sensitive hosts; block common LOLBINs in contexts they are not normally used.
- Implement **Deception / Honeypots**: create fake credentials/endpoints and alert on any access — direct detection of attacker TTPs.

## Incident response playbook (TTP-focused)
1. Detect abnormal behavior and map to ATT&CK technique(s).
2. Prioritize based on impact (sensitive data, privilege use, lateral spread).
3. Contain: isolate infected hosts, revoke involved credentials (rotate service accounts), block known C2 on network devices.
4. Collect forensics: memory dumps, process trees, logs, timeline.
5. Hunt for earlier stages: pivot back through logs to find initial access vector (phishing, exposed service, supply chain).
6. Remediate: remove persistence, patch exploited vulnerabilities, harden configs, update detection rules.
7. Lessons & gating: tune detections, apply preventative controls, share IOCs and TTP mappings with teams.

## Measuring detector effectiveness (apply to TTPs)
- Use **detection coverage matrix**: map ATT&CK techniques → which telemetry you have → detection quality (none / heuristic / high-confidence).  
- Track **time-to-detect** (TTD) and **time-to-contain** (TTC) for TTP events.
- Red-team / purple-team: test detection quality by emulating TTPs (calibrated and controlled).

## Example mapping (simple)
- Technique: T1078 Valid Accounts
  - Data sources: authentication logs, privilege escalation events, atypical access patterns.
  - Detection: anomalous logon patterns + new administrative operations + host artifacts.

- Technique: T1059.001 PowerShell
  - Data sources: process creation, command lines, PowerShell transcription logs.
  - Detection: Office spawns PowerShell with -EncodedCommand + network connection to uncommon domain → escalate.

## Hunting exercise (do this)
1. Pick an APT or threat actor (Rapid7, Mandiant, or vendor intel write-up).
2. Extract the TTPs they use (phishing, credential stuffing, specific privilege escalation).
3. Map each TTP to:
   - telemetry needed,
   - a detection approach (rule, ML, hunting query),
   - where it sits on the Pyramid of Pain (TTP = apex).
4. Create a chain-detection Sigma rule or EDR analytic to catch the sequence described by the actor.

## Best practices & hard truths
- TTP detection requires investment in telemetry (EDR, identity logs, network metadata) and analytic correlation.
- Focus on high-impact TTPs first (lateral movement, credential abuse, exfil).
- Avoid siloed detection; cross-domain correlation is essential.
- Test detections regularly with red/purple team exercises.

## TL;DR
TTPs are the apex of the Pyramid of Pain — detecting them hits the attacker where it hurts most because it forces changes to how they operate. Build baselines, collect rich telemetry, map to ATT&CK, detect sequences of behavior (not just individual artifacts), and run regular purple-team tests to keep your defenses honest.




