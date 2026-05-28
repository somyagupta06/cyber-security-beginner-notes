# MBR, BIOS, UEFI and Boot Process Notes (SOC Point of View)

## 1. What is a Disk?

A disk is the place where the computer stores data.

Example:

* Windows files
* Games
* Photos
* Documents

Think of a disk like a big school building.

---

## 2. What is a Partition?

A partition is a small section inside the disk.

Example:

| Partition | Work                     |
| --------- | ------------------------ |
| C:        | Windows Operating System |
| D:        | Personal Files           |
| E:        | Backup Files             |

Partitions help organize data properly.

Without partitions, all data would become messy.

---

## 3. What is MBR?

MBR = Master Boot Record

It is the first part of the disk that helps the computer start.

Location:

* First sector of disk
* First 512 bytes

MBR contains:

1. Bootloader Code
2. Partition Table
3. MBR Signature

---

## 4. Simple Boot Process

When we press the power button:

Step 1:
Computer gets electricity.

Step 2:
CPU starts working.

Step 3:
BIOS or UEFI wakes up.

Step 4:
BIOS/UEFI checks hardware.
This is called POST.

POST checks:

* RAM
* Keyboard
* Hard Disk
* CPU

Step 5:
BIOS/UEFI searches for a bootable disk.

Step 6:
It reads the MBR from the disk.

Step 7:
MBR finds the bootable partition.

Step 8:
Operating System loads.

Step 9:
Windows starts.

---

## 5. BIOS vs UEFI

| BIOS                 | UEFI                      |
| -------------------- | ------------------------- |
| Old technology       | Modern technology         |
| Uses MBR             | Uses GPT                  |
| Supports small disks | Supports very large disks |
| Less secure          | More secure               |

---

## 6. What is GPT?

GPT = GUID Partition Table

Modern systems use GPT instead of MBR.

Advantages:

* More secure
* Supports more partitions
* Better recovery features

UEFI mostly works with GPT.

---

# MBR Structure

MBR size = 512 bytes

| Bytes   | Purpose         |
| ------- | --------------- |
| 0-445   | Bootloader Code |
| 446-509 | Partition Table |
| 510-511 | Signature       |

---

## 7. Bootloader Code

This code starts first.

Its job:

* Find the bootable partition
* Start the operating system

SOC Point of View:
Attackers sometimes place malware here.

Why?
Because this code runs before Windows starts.

---

## 8. Partition Table

This part stores information about partitions.

It tells:

* Partition location
* Partition size
* Filesystem type
* Bootable partition

MBR supports only 4 primary partitions.

---

## 9. Important Partition Fields

### Boot Indicator

| Value | Meaning      |
| ----- | ------------ |
| 80    | Bootable     |
| 00    | Not Bootable |

If a partition has value 80, it means the OS can boot from it.

---

### Partition Type

This tells the filesystem type.

Example:

| Hex Value | Filesystem |
| --------- | ---------- |
| 07        | NTFS       |

NTFS is commonly used in Windows.

---

### Starting LBA

LBA = Logical Block Address

It tells where the partition starts on the disk.

SOC analysts use this field to:

* Find hidden partitions
* Recover deleted data
* Investigate malware activity

---

### Number of Sectors

This tells the size of the partition.

Formula:

Number of Sectors × 512

This gives the partition size in bytes.

---

## 10. MBR Signature

Last 2 bytes of MBR:

55 AA

This is called the Magic Number.

Purpose:
It tells the system that the MBR is valid.

If these bytes are damaged:

* System may not boot

---

# SOC Analyst Point of View

SOC analysts check MBR because attackers can hide malware there.

---

## 11. What is a Bootkit?

A bootkit is malware that infects the boot process.

It can:

* Start before Windows
* Hide from antivirus
* Stay persistent after reboot

Very dangerous malware.

---

## 12. Why Attackers Target MBR?

Because:

* MBR runs before Windows
* Security tools are not fully active yet
* Malware becomes harder to detect

---

## 13. Signs of MBR Attack

SOC analysts look for:

* Modified MBR bytes
* Broken MBR signature
* Unknown boot code
* Hidden partitions
* Suspicious bootable partitions

---

## 14. Important Tools

| Tool       | Purpose            |
| ---------- | ------------------ |
| HxD        | Hex analysis       |
| FTK Imager | Disk imaging       |
| Autopsy    | Digital forensics  |
| TestDisk   | Partition recovery |

---

## 15. HxD Hex Editor Basics

HxD shows:

* Offsets
* Hex values
* ASCII text

SOC analysts mainly focus on:

* Hex values
* Disk offsets
* Partition entries

---

# Quick Revision

## MBR

* First 512 bytes of disk
* Starts boot process

## BIOS

* Old firmware
* Uses MBR

## UEFI

* Modern firmware
* Uses GPT

## Bootkit

* Malware in boot process

## Important MBR Parts

1. Bootloader Code
2. Partition Table
3. Signature

## MBR Signature

55 AA

---

# Very Important SOC Concept

Attackers love places that run before security tools.

MBR is one of those places.

That is why boot process analysis is important in SOC and Digital Forensics.

---
# MBR Attacks and Recovery Notes (SOC Point of View)

# 1. Why MBR is Important

MBR = Master Boot Record

Location:

* First 512 bytes of the disk

Purpose:

* Starts the boot process
* Finds the bootable partition
* Helps load the Operating System

If MBR is damaged:

* System may not boot
* Disk may become unreadable

---

# 2. Why Attackers Target MBR

MBR runs before Windows starts.

This makes it a powerful target for attackers.

Attackers use MBR to:

* Hide malware
* Break boot process
* Show ransom messages
* Make systems unusable

---

# 3. Bootkits

## What is a Bootkit?

A bootkit is malware that infects the boot process.

It hides inside the MBR or bootloader.

---

## Why Bootkits are Dangerous

Bootkits run:

* Before Windows
* Before antivirus
* Before security tools

This helps malware stay hidden.

---

## Important Point

Even reinstalling Windows may not remove the bootkit because it lives inside the MBR.

---

# 4. Ransomware Attacks on MBR

Some ransomware attacks the MBR instead of encrypting files.

---

## What Happens?

The malware:

* Replaces boot code
* Corrupts the MBR
* Shows ransom message after reboot

---

## Result

Instead of Windows loading:

* ransom screen appears
* system becomes unusable

---

## Examples

| Malware    | Activity                |
| ---------- | ----------------------- |
| Petya      | Encrypted MBR           |
| Bad Rabbit | Replaced MBR bootloader |

---

# 5. Wiper Malware

## What is Wiper Malware?

Wiper malware destroys data instead of asking for money.

---

## MBR Attack by Wipers

The malware overwrites or corrupts the MBR.

Result:

* system becomes unbootable
* disk cannot load properly

---

## Example

| Malware | Activity                       |
| ------- | ------------------------------ |
| Shamoon | Overwrote MBR with random data |

---

# 6. Scenario Summary

An employee:

* opened a malicious email attachment
* rebooted the machine

After reboot:

* server became unbootable

Investigation found:

* MBR corruption

---

# 7. Corrupted MBR Fields

Two important things were damaged.

---

## Corruption 1: Starting LBA

Original value:

```text
00 08 00 00
```

Purpose:

* tells where the partition starts

If corrupted:

* system cannot find the partition correctly

---

## Corruption 2: MBR Signature

Correct value:

```text
55 AA
```

Purpose:

* confirms the MBR is valid

If corrupted:

* system may fail to boot

---

# 8. HxD Tool

HxD is a hex editor.

Used for:

* viewing raw disk bytes
* editing corrupted MBR values

---

## HxD Work in This Task

Steps:

1. Open corrupted disk image
2. Find damaged bytes
3. Replace with correct bytes
4. Save the image

---

# 9. FTK Imager

FTK Imager is a forensic tool.

Used for:

* opening disk images
* checking partitions
* viewing filesystems

---

## Before Repair

FTK shows:

```text
Unrecognized file system
```

Reason:

* MBR is corrupted

---

## After Repair

FTK can:

* read the disk properly
* display partitions
* show files and folders

---

# 10. SOC Analyst Learning

SOC analysts should understand:

* boot process attacks
* MBR corruption
* bootkits
* ransomware behavior
* disk recovery basics

---

# 11. Simple Attack Flow

```text
Malicious attachment opened
        ↓
Malware executes
        ↓
MBR gets corrupted
        ↓
System reboots
        ↓
Boot process fails
```

---

# 12. Simple Recovery Flow

```text
Open disk image in HxD
        ↓
Fix corrupted bytes
        ↓
Save disk image
        ↓
Verify in FTK Imager
        ↓
Disk becomes readable again
```

---

# 13. Important Tools

| Tool       | Purpose             |
| ---------- | ------------------- |
| HxD        | Hex editing         |
| FTK Imager | Disk image analysis |
| TestDisk   | Partition recovery  |
| Autopsy    | Digital forensics   |

---

# 14. Quick Revision

## Bootkit

Malware inside boot process

## Ransomware on MBR

Locks system during boot

## Wiper Malware

Destroys MBR and breaks system

## HxD

Used to edit raw disk bytes

## FTK Imager

Used to verify repaired disk

## Important MBR Signature

```text
55 AA
```

## Important LBA Value

```text
00 08 00 00
```

---

# Final SOC Point

The boot process is a high-value target.

If attackers control the MBR:

* they can hide malware
* break systems
* bypass security tools

That is why SOC analysts and forensic investigators study MBR attacks carefully.

---

 # GPT, UEFI and ESP Notes (SOC Point of View)

# 1. What is GPT?

GPT = GUID Partition Table

GPT is a modern partitioning scheme.

It is used with:

* UEFI firmware

GPT replaced:

* MBR

---

# 2. Why GPT is Better than MBR

| MBR                 | GPT                       |
| ------------------- | ------------------------- |
| Old system          | Modern system             |
| Supports 2 TB disks | Supports very large disks |
| Only 4 partitions   | Up to 128 partitions      |
| No proper backup    | Backup available          |

---

# 3. BIOS vs UEFI

| BIOS         | UEFI            |
| ------------ | --------------- |
| Old firmware | Modern firmware |
| Uses MBR     | Uses GPT        |
| Less secure  | More secure     |

---

# 4. GPT Structure

GPT has 5 important components:

1. Protective MBR
2. Primary GPT Header
3. Partition Entry Array
4. Backup GPT Header
5. Backup Partition Entry Array

---

# 5. Protective MBR

Location:

* First sector of disk

Purpose:

* Protect GPT disks from old BIOS systems

Important byte:

```text id="z75v2x"
EE
```

Meaning:

* Disk is GPT formatted

MBR Signature:

```text id="zbjlwm"
55 AA
```

---

# 6. Primary GPT Header

The GPT Header stores important disk information.

It acts like:

* disk blueprint
* partition map

---

# 7. Important GPT Header Fields

| Field                     | Meaning                      |
| ------------------------- | ---------------------------- |
| Signature                 | Identifies GPT               |
| Revision                  | GPT version                  |
| Header Size               | Size of GPT header           |
| CRC32                     | Integrity check              |
| Current LBA               | Current GPT location         |
| Backup LBA                | Backup GPT location          |
| First Usable LBA          | Partition start area         |
| Last Usable LBA           | Partition end area           |
| Disk GUID                 | Unique disk ID               |
| Partition Entry Array LBA | Partition list location      |
| Number of Entries         | Number of partitions         |
| Size of Entry             | Size of each partition entry |

---

# 8. GPT Signature

GPT header starts with:

```text id="p6p6yu"
45 46 49 20 50 41 52 54
```

This identifies the disk as GPT.

---

# 9. What is LBA?

LBA = Logical Block Address

Purpose:

* tells the exact disk location

SOC analysts use LBA to:

* locate partitions
* recover hidden data
* investigate disks

---

# 10. Partition Entry Array

This is the partition table of GPT.

GPT supports:

* 128 partition entries

Each partition entry size:

* 128 bytes

---

# 11. Important Partition Entry Fields

| Field                 | Meaning               |
| --------------------- | --------------------- |
| Partition Type GUID   | Type of partition     |
| Unique Partition GUID | Unique partition ID   |
| Starting LBA          | Partition start       |
| Ending LBA            | Partition end         |
| Attributes            | Hidden/bootable flags |
| Partition Name        | Partition name        |

---

# 12. GUID

GUID = Globally Unique Identifier

Purpose:

* uniquely identify disks and partitions

Example format:

```text id="0dclcj"
1DF1B0D6-43BE-374E-B1E6-3866ECB17389
```

---

# 13. EFI System Partition (ESP)

ESP = EFI System Partition

Very important partition in GPT disks.

Purpose:

* stores bootloader files

Important file extension:

```text id="fq8rza"
.efi
```

Examples:

```text id="2g69h9"
bootmgr.efi
bootx64.efi
```

---

# 14. UEFI Boot Process

```text id="7pw1qy"
Power ON
↓
UEFI starts
↓
POST check
↓
UEFI reads GPT disk
↓
UEFI loads .efi files from ESP
↓
Bootloader loads OS kernel
↓
Windows starts
```

---

# 15. Backup GPT Header

GPT stores a backup GPT header.

Location:

* end of disk

Purpose:

* recovery if primary GPT is damaged

---

# 16. Backup Partition Entry Array

GPT also stores a backup partition table.

Purpose:

* recover partitions if original gets corrupted

---

# 17. Bootkits in GPT

In GPT systems:

* attackers target .efi files inside ESP

Purpose:

* run malware before Windows starts

Danger:

* bypass OS security tools

---

# 18. Secure Boot

Secure Boot is a UEFI security feature.

Purpose:

* verify digital signatures of .efi files

If bootloader is modified:

* boot may be blocked

---

# 19. Ransomware in GPT

Ransomware may:

* damage ESP
* encrypt bootloader files
* disrupt boot process

Result:

* system may not boot

---

# 20. Wiper Malware in GPT

Wiper malware may:

* destroy primary GPT
* destroy backup GPT
* damage ESP

Result:

* severe boot failure
* difficult recovery

---

# 21. Scenario Summary

A bootkit modified:

```text id="0vh73m"
bootmgr.efi
```

Possible activity:

* hidden payload
* encoded string
* modified bootloader

SOC analysts must:

* inspect the file in HxD
* look for suspicious bytes
* identify hidden data

---

# 22. HxD in GPT Investigation

HxD is used for:

* viewing raw hex data
* finding suspicious strings
* analyzing tampered boot files

SOC analysts check:

* unusual bytes
* encoded strings
* hidden data
* modified .efi files

---

# 23. Important SOC Concepts

## ESP

Stores critical bootloader files

## Secure Boot

Stops tampered boot files

## Bootkit

Malware hidden in boot process

## GPT Redundancy

Backup headers improve recovery

---

# 24. Quick Revision

## GPT

Modern partition scheme

## UEFI

Modern firmware

## ESP

Stores .efi bootloader files

## Protective MBR

Protects GPT disks from old BIOS systems

## Secure Boot

Verifies bootloader integrity

## Bootkit

Boot malware before OS loads

## Backup GPT

Recovery mechanism

---

# Final SOC Point

Attackers target the boot process because it runs before Windows and security tools.

In GPT systems, attackers mainly target:

* ESP
* .efi bootloader files
* GPT structures

That is why SOC analysts must understand:

* GPT structure
* UEFI boot process
* bootkit detection
* hex analysis of boot files

