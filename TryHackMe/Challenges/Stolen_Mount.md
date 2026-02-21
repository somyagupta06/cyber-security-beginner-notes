# 🕵️ NFS PCAP Investigation — Super Simple Notes (Beginner Level)

Imagine this like a **school story** 👇

---

## 📖 The Story (Scenario)

A thief entered a school computer network.

There is one **special computer (server)** where backup files are stored.

The thief:
- connected to that server
- opened a secret file
- stole the data
- left

Now ❗  
The **only thing left** is a recording of network activity.

That recording file = **PCAP file**

Your job =  
👉 Watch the recording  
👉 Find which file was stolen  
👉 Recover the secret

---

## 🧠 What is NFS?

**NFS = Network File System**

Very simple meaning:

> NFS allows one computer to open files from another computer over a network.

### Example

Think like this:

| Real Life | Computer World |
|------------|---------------|
| School library | NFS Server |
| Student | Client |
| Opening a book | Reading a file |
| Library network | NFS protocol |

The attacker is like a student opening books from the library remotely.

---

## 🌐 What Happened in This Attack?

Step by step:

1. Attacker connected to NFS server
2. Started communication
3. Looked for files
4. Opened a file
5. Read file data
6. Stole secret

All these actions are recorded inside the PCAP.

---

## 📦 What is a PCAP File?

PCAP = Network Recording

It records:
- who talked
- which server was accessed
- which file was opened
- what data moved

Like CCTV footage of network traffic.

---

## 🔎 Important NFS Actions You Saw

Inside Wireshark you saw words like:

| Name | Meaning (Simple) |
|------|------------------|
| EXCHANGE_ID | Client says hello |
| CREATE_SESSION | Connection created |
| GETATTR | Checking file info |
| LOOKUP | Searching file |
| READ | Reading file data ⭐ |

✅ **READ is most important**

Because data theft happens here.

---

## 🎯 Your Main Goal

Find packets where:
READ

exists.

Because:

> READ = File content transfer

That is where the secret lives.

---

## 🧭 Investigation Steps (Very Easy)

### Step 1 — Show Only NFS Traffic
Type in Wireshark filter bar:

nfs

---

### Step 2 — Find File Reading
Scroll and look for:

READ

or

COMPOUND → READ

---

### Step 3 — Open Data Stream

Right click the READ packet:

Follow → TCP Stream

Now you may see:
- text
- file content
- secret message

---

### Step 4 — Try File Extraction

Go to:

File → Export Objects → NFS

Sometimes Wireshark rebuilds the stolen file.

---

## 🧩 How to Think Like Investigator

Ask these questions:

- Who connected?
- Which server was accessed?
- Which file was searched?
- Which file was READ?
- What data moved?

Answer = stolen secret.

---

## ⭐ Golden Rule
- Connection shows access
- READ shows theft


Always remember this.

---

## ✅ Final Understanding

You are not hacking.

You are:
- watching network recording
- tracking attacker activity
- recovering stolen data

You are acting like a **Digital Detective**.

---

## 🧠 One-Line Revision

NFS lets remote file access.
PCAP records network traffic.
READ packets contain stolen data.
Find READ → Follow Stream → Get Secret
