# 🔐 Firewall Basics 

## 🧠 What is a Firewall?
A firewall is like a **security guard** of a network.

It checks every packet (data) that comes in or goes out.

👉 It decides:
- Allow (PASS)
- Block (DROP)

---

## 📦 What is inside a network packet?

Every packet has:

- Source IP → who is sending
- Destination IP → who is receiving
- Source Port → which app is sending
- Destination Port → which service is used

---

## 🏠 Easy Example

- IP Address = House address  
- Port = Room number  

👉 So:
- IP = building
- Port = specific room inside

---

## ⚙️ Firewall Rules

A firewall works using rules.

Example:

| Source IP   | Destination IP | Destination Port | Action |
|------------|----------------|------------------|--------|
| 172.16.4.1 | 10.10.10.41    | 80               | PASS   |
| 172.16.8.1 | 10.10.10.81    | 23               | DROP   |

---

## ✅ Rule 1 (PASS)

- Port 80 = Website (HTTP)
- This is normal traffic

👉 Firewall allows it

---

## ❌ Rule 2 (DROP)

- Port 23 = Telnet (unsafe)

👉 Firewall blocks it

---

## 🚨 Actions in Firewall

- PASS → allow traffic  
- DROP → block silently  
- REJECT → block and send response  

👉 SOC mostly uses DROP

---

## 🛡️ Firewall in SOC (Important)

SOC uses firewall to:

- Stop attackers quickly
- Block suspicious IPs
- Control network access

---

## 🔍 Real SOC Example

Problem:
- One IP is sending many requests (attack)

SOC Action:
1. Identify attacker IP
2. Add firewall rule:
   - Block that IP

👉 Attack stops immediately

---

## 💡 Key Points to Remember

- Firewall = network security guard
- It checks IP and port
- Uses rules to allow/block traffic
- Very useful to stop attacks fast

---

## 🎯 Final Line (Revision)

Firewall helps SOC **control traffic and stop attacks quickly**
