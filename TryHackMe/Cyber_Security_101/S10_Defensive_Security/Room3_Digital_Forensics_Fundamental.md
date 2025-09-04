# Digital Forensics Notes

## 1. What is Forensics?
- Forensics = Application of methods + procedures to investigate and solve crimes.
- Digital Forensics = Branch of forensics that deals with **cyber crimes** (crimes involving digital devices).

### Example:
- Normal Forensics → Fingerprints, Blood sample, DNA test.
- Digital Forensics → Analyzing Laptop, Mobile, Hard Drive, USB.



## 2. What is Cyber Crime?
- Any criminal activity conducted **on or using a digital device**.
- Due to high usage of digital devices, **cyber crimes are increasing**.

### Examples of Cyber Crimes:
- Hacking bank systems
- Data theft
- Online fraud
- Illegal chat groups
- Planning robbery using digital devices



## 3. Case Example (Bank Robbery)

### Scenario:
- Police raid robber’s house with search warrant.
- Devices found: **Laptop, Mobile, Hard Drive, USB**.
- Digital Forensics team investigates in their lab.

### Evidence Found:
| Device       | Evidence Found                                                                 |
|--------------|--------------------------------------------------------------------------------|
| Laptop       | Digital map of bank, media files (photos/videos of previous robberies)         |
| Hard Drive   | Document with bank’s entrance + escape routes, security controls bypass plans  |
| Mobile Phone | Illegal chat groups, call records related to robbery                           |



## 4. Digital Forensics Process

Digital Forensics follows **4 major steps**:

1. **Collection**
   - Securely collect devices (Laptop, Phone, USB).
   - Ensure no tampering.
   - Example: Police bagging laptop with seal.

2. **Preservation (Storage)**
   - Store devices safely.
   - Maintain **chain of custody** (record who handled the evidence and when).
   - Example: Evidence locker with ID tags.

3. **Analysis**
   - Investigate devices using forensic tools.
   - Recover deleted files, chats, call logs, images, documents.
   - Example: Finding hidden bank maps from hard drive.

4. **Reporting**
   - Create final report for legal use in court.
   - Report should be clear, detailed, and explain evidence.
   - Example: “Suspect had digital map of bank in laptop, linked to robbery plan.”



## 5. Importance of Digital Forensics
- Helps law enforcement **prove crimes in court**.
- Collects solid digital evidence.
- Tracks criminals through devices.
- Protects society from digital threats.



## 6. Comparison: Traditional Forensics vs Digital Forensics

| Feature                 | Traditional Forensics                 | Digital Forensics                          |
|--------------------------|---------------------------------------|--------------------------------------------|
| Crime Scene Evidence     | Fingerprints, blood, weapons          | Laptop, Mobile, USB, Hard Drive             |
| Tools Used               | DNA kits, fingerprint scanner         | Forensic software, data recovery tools      |
| Nature of Evidence       | Physical                              | Digital (files, chats, media, logs)         |
| Example Case             | Murder, physical robbery              | Cyber fraud, planning robbery on computer   |



# Learning Objectives of This Room
1. Understand how digital evidence is collected.
2. Learn how evidence is preserved safely.
3. Study analysis methods and tools.
4. Know how final forensic reports are prepared.
---
# Digital Forensics Process (NIST Model)

The **National Institute of Standards and Technology (NIST)** defines a general process for digital forensics in **4 phases**:

➡️ Collection → Examination → Analysis → Reporting



## 1. Collection
- First phase = Data collection.
- Identify all devices at crime scene (PC, laptop, mobile, USB, camera).
- Ensure:
  - Data is not tampered.
  - Proper documentation (who collected, when, what device).

### Example:
Police finds:
- Laptop, USB, Mobile → All are sealed, tagged, and documented before sending to forensic lab.



## 2. Examination
- Collected data can be **huge**.
- Investigator filters the data to extract only **useful information**.

### Example:
- From camera: Only select photos taken on robbery date.
- From system: Extract data of specific user (ignore others).
- Purpose = Reduce irrelevant data → Keep only important data.



## 3. Analysis
- **Most critical phase**.
- Correlate filtered data with other evidence.
- Extract activities relevant to the case, in chronological order.

### Example:
- Timeline of suspect’s activity (calls, files, GPS locations).
- Compare laptop maps + hard drive documents + mobile chats.



## 4. Reporting
- Prepare **detailed report**:
  - Investigation methodology
  - Findings from evidence
  - Recommendations (if needed)
  - Executive summary (for non-technical readers like judges, managers)

### Example:
Report shows:
- Suspect used laptop to plan robbery
- Mobile chats prove coordination
- Evidence timeline matches robbery day



# Types of Digital Forensics

Different categories of forensics focus on different evidence sources.

| Type of Forensics   | Focus Area | Evidence Examples |
|---------------------|------------|------------------|
| **Computer Forensics** | Investigating computers (most common) | Documents, hidden files, browsing history |
| **Mobile Forensics**   | Investigating smartphones | Call records, SMS, GPS data, WhatsApp chats |
| **Network Forensics**  | Investigating network traffic | Logs, IP traces, suspicious packets |
| **Database Forensics** | Investigating databases | Unauthorized data changes, data theft |
| **Cloud Forensics**    | Investigating cloud storage/services | Cloud logs, files stored on servers |
| **Email Forensics**    | Investigating emails | Phishing mails, fraud communication |



# Quick Visual Flow 

**Phases (NIST):**
Collection → Examination → Analysis → Reporting

**Types (Where applied):**
Computer | Mobile | Network | Database | Cloud | Email

---
# Evidence Acquisition in Digital Forensics

Acquiring evidence = **very critical job**.  
Evidence must be collected securely, without tampering the original data.

Different devices (laptop, phone, USB) may need different methods, but some **general practices** are always followed.


## 1. Proper Authorization
- Forensics team must get **approval from relevant authority** before collecting evidence.
- Without approval → Evidence may be **inadmissible in court**.
- Evidence often contains **private and sensitive data** → Must be collected legally.

### Example:
- Police want to collect suspect’s laptop → They first get **search warrant** or official authorization.
- Otherwise, laptop data cannot be shown as valid evidence in trial.


## 2. Chain of Custody
- Formal document that tracks **who handled evidence, when, and where it is stored**.
- Prevents issues like evidence loss, tampering, or mismanagement.

### Key Details in Chain of Custody:
1. Description of evidence (name, type, serial no.)
2. Collector’s name (who collected it)
3. Date & time of collection
4. Storage location
5. Record of access (who accessed it, when)

### Example:
- Laptop seized → Document shows:
  - Collected by Officer A at 10:30 AM, stored in Locker 2.
  - Accessed later by Investigator B at 3:00 PM.
- This ensures **evidence reliability** in court.



## 3. Use of Write Blockers
- Write blocker = Special device/tool that prevents **any changes** on original storage media (like HDD, SSD, USB).
- Needed because connecting suspect’s hard drive directly to forensic workstation may alter file timestamps.

### Example:
- Without Write Blocker:
  - Investigators connect hard drive → System background process changes file timestamps.
  - This alters original evidence → Invalid in court.
- With Write Blocker:
  - Hard drive stays in **original state**.
  - Data can only be **read**, not modified.



# Why These Practices Matter
- Court only accepts evidence if it is **authentic, untampered, and legally collected**.
- Proper authorization + chain of custody + write blockers = Ensures **integrity** of digital evidence.




---

# Windows Forensics: Evidence Acquisition & Analysis

## 1. Why Focus on Windows?
- Most criminal activities involve **personal computers/laptops**.
- Windows OS is **very common**, so forensic teams often analyze it.



## 2. Forensic Images in Windows
Forensics teams create **forensic images** = **bit-by-bit copies** of system data.

### Two Main Categories:

1. **Disk Image**
   - Copy of data on storage device (**HDD / SSD**).
   - **Non-volatile** → survives restart/shutdown.
   - Includes: Media files, Documents, Browsing history, Installed software.
   - Example: Investigator recovers deleted bank robbery plans from disk.

2. **Memory Image**
   - Copy of data inside **RAM** (Random Access Memory).
   - **Volatile** → data lost after power off/restart.
   - Includes: Running processes, Open files, Current network connections.
   - Must be **captured first** before system shuts down.
   - Example: Investigators capture suspect’s active chat application from RAM.



## 3. Tools for Acquisition & Analysis

| Tool        | Type of Image | Purpose | Features |
|-------------|---------------|---------|----------|
| **FTK Imager** | Disk Image | Acquisition + Analysis | GUI tool, create disk images, preview contents |
| **Autopsy**    | Disk Image | Analysis | Open-source, keyword search, recover deleted files, metadata check |
| **DumpIt**     | Memory Image | Acquisition | CLI tool, creates memory images in different formats |
| **Volatility** | Memory Image | Analysis | Open-source, plugin-based, supports Windows/Linux/macOS/Android |



## 4. Example Workflow (Windows Forensics Case)
1. **Step 1:** Capture **memory image** first using `DumpIt` (volatile evidence).  
2. **Step 2:** Capture **disk image** using `FTK Imager`.  
3. **Step 3:** Analyze disk image with **Autopsy** (find hidden files, recover deleted data).  
4. **Step 4:** Analyze memory image with **Volatility** (running processes, network connections).  
5. **Step 5:** Correlate results → build complete timeline of suspect’s activities.  


## 5. Summary
- **Disk Image = Non-volatile (HDD/SSD data)**  
- **Memory Image = Volatile (RAM data)** → always capture first  
- **Tools:**  
  - Disk Acquisition/Analysis → FTK Imager, Autopsy  
  - Memory Acquisition/Analysis → DumpIt, Volatility  

---
# Digital Forensics: Metadata & EXIF Data

## 1. What is Metadata?
- Metadata = "Data about data".
- Every file (document, image, video, etc.) has **hidden details** saved by system/software.
- Useful for investigations because it can reveal:
  - Who created the file
  - When it was created/modified
  - Which software/device was used



## 2. Metadata in Documents (Example: PDF from MS Word)

### Tool: `pdfinfo`
- Shows PDF metadata like title, author, creation date, etc.
- Installed by default in AttackBox.
- Install manually (Kali):  
  `sudo apt install poppler-utils`

### Example Command:
```
pdfinfo DOCUMENT.pdf
```
### Sample Output:
```
Creator: Microsoft® Word for Office 365
Producer: Microsoft® Word for Office 365
CreationDate: Wed Oct 10 21:47:53 2018
ModDate: Wed Oct 10 21:47:53 2018
Pages: 20
File size: 560362 bytes
PDF version: 1.7
```

✅ From this output we learn:
- File was created in **MS Word Office 365**.
- Created on **Oct 10, 2018**.
- File has **20 pages**.



## 3. Metadata in Images (EXIF Data)

### What is EXIF?
- **EXIF = Exchangeable Image File Format**.
- Metadata stored inside image files (e.g., JPEG, PNG).
- Common EXIF information:
  - Camera/phone model
  - Date & time photo was taken
  - Photo settings (ISO, shutter speed, etc.)
  - GPS coordinates (if taken on smartphone/camera with GPS)



### Tool: `exiftool`
- Reads and writes metadata of image files.
- Installed by default in AttackBox.
- Install manually (Kali):  
  `sudo apt install libimage-exiftool-perl`

### Example Command:
```
exiftool IMAGE.jpg
```
### Sample Output:
```
Camera Model Name : iPhone 12 Pro
Date/Time Original: 2021:08:15 16:42:12
GPS Position : 51 deg 31' 4.00" N, 0 deg 5' 48.30" W
```

✅ From this output we learn:
- Photo taken with **iPhone 12 Pro**.
- Captured on **15 Aug 2021 at 16:42**.
- GPS location available.



## 4. Using GPS Coordinates
- Metadata can contain **latitude + longitude**.
- Example from EXIF:
GPS Position: 51 deg 30' 51.90" N, 0 deg 5' 38.73" W
- To use in Google Maps/Bing Maps:
- Replace `deg` with `°`
- Remove extra spaces
- Final search string:  
`51°30'51.9"N 0°05'38.7"W`

✅ This reveals **exact street/location** where photo was taken.



# Quick Summary

- **Document Metadata (pdfinfo)** → Software used, creation date, author.  
- **Image Metadata (exiftool)** → Device model, capture time, GPS location.  
- Metadata = powerful forensic clue to connect suspect → device → location → timeline.

---
