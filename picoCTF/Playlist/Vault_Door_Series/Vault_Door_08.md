# vault-door-8

## PROBLEM STATEMENT 
Apparently Dr. Evil's minions knew that our agency was making copies of their source code, because they intentionally sabotaged this source code in order to make it harder for our agents to analyze and crack into! The result is a quite mess, but I trust that my best special agent will find a way to solve it. The source code for this vault is here: VaultDoor8.java


## Step-by-Step Solution:

# VaultDoor8 Password Scramble Explained (Simple Version)

## 1️⃣ What is happening?

- The program asks for a vault password in the format `picoCTF{...}`.
- Inside, there is a **scramble function** that mixes up the bits of each character.
- This is done so that even if someone sees the password in memory, it looks scrambled.

---

## 2️⃣ What is a Bit Swap?

- Computers store letters as **8 bits** (0 or 1).  
  Example: `'A'` → binary `01000001`
- **Swap** means exchanging the positions of 2 bits.  
  Example: Swap bit 0 and bit 1:

Original: 01000001 ('A')
After swap: 10000001

- Only 2 bits changed, rest stay the same.  

---

## 3️⃣ How Scramble Works

- The scramble function applies **multiple swaps** per character:

(1,2), (0,3), (5,6), (4,7), (0,1), (3,4), (2,5), (6,7)

- Each pair means: take the bit at first position and swap it with the second position.
- After all swaps, the character looks completely different.

---

## 4️⃣ How to Reverse Scramble

- To get the **original password** from scrambled bytes:
  1. Use the **same swaps** in **reverse order**:

Reverse swaps:
(6,7), (2,5), (3,4), (0,1), (4,7), (5,6), (0,3), (1,2)

- Reverse swapping restores the original character.

---

## 5️⃣ Step-by-step Example (First Character)

- Scrambled byte: `0xF4` → binary `11110100`
- Apply reverse swaps in order:

(6,7) -> 11110100
(2,5) -> 11110100
(3,4) -> 11101100
(0,1) -> 11101100
(4,7) -> 01111100
(5,6) -> 01111100
(0,3) -> 01110101
(1,2) -> 01110011

- Final binary: `01110011` → ASCII `'s'`
- ✅ Original character restored!

---

## 6️⃣ Full Password Table (Scrambled → Original)

| Index | Scrambled (hex) | Original (hex) | ASCII |
|-------|----------------|----------------|-------|
| 0     | 0xF4           | 0x73           | s     |
| 1     | 0xC0           | 0x30           | 0     |
| 2     | 0x97           | 0x6d           | m     |
| 3     | 0xF0           | 0x33           | 3     |
| 4     | 0x77           | 0x5f           | _     |
| 5     | 0x97           | 0x6d           | m     |
| 6     | 0xC0           | 0x30           | 0     |
| 7     | 0xE4           | 0x72           | r     |
| 8     | 0xF0           | 0x33           | 3     |
| 9     | 0x77           | 0x5f           | _     |
| 10    | 0xA4           | 0x62           | b     |
| 11    | 0xD0           | 0x31           | 1     |
| 12    | 0xC5           | 0x74           | t     |
| 13    | 0x77           | 0x5f           | _     |
| 14    | 0xF4           | 0x73           | s     |
| 15    | 0x86           | 0x68           | h     |
| 16    | 0xD0           | 0x31           | 1     |
| 17    | 0xA5           | 0x66           | f     |
| 18    | 0x45           | 0x54           | T     |
| 19    | 0x96           | 0x69           | i     |
| 20    | 0x27           | 0x4E           | N     |
| 21    | 0xB5           | 0x67           | g     |
| 22    | 0x77           | 0x5f           | _     |
| 23    | 0xE0           | 0x32           | 2     |
| 24    | 0x95           | 0x65           | e     |
| 25    | 0xF1           | 0x37           | 7     |
| 26    | 0xE1           | 0x36           | 6     |
| 27    | 0xE0           | 0x32           | 2     |
| 28    | 0xA4           | 0x62           | b     |
| 29    | 0xC0           | 0x30           | 0     |
| 30    | 0x94           | 0x61           | a     |
| 31    | 0xA4           | 0x62           | b     |

- All characters together → `s0m3_m0r3_b1t_sh1fTiNg_2e762b0ab`

---

## 7️⃣ Final Flag

```text
picoCTF{s0m3_m0r3_b1t_sh1fTiNg_2e762b0ab}
```
