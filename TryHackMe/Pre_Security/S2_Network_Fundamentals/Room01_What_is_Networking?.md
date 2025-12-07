# Networking Notes

---



## **Networking**

Network → simply devices connected.

## Internet 

One global network that consists of many, many small networks within itself.

- First Iteration of Internet → ARPANET (1960’s)

  i. Funded by US Defense Department.

  ii. First decentralized network.

- 1989 → Tim Berners-Lee → created World Wide Web.

- Small networks called Pvt Networks.

- Networks connecting small Pvt Networks → Public Net.

- Network
  1. Private
  2. Public
 

## **Identifying Devices on a Network**  
To communicate and maintain order, devices must be both identifiying and identifiable on a network.

**Devices → analogous to humans**
- Human
  -> our name
  -> our fingerprints

- Devices
  -> IP Address (must access control
  -> [MAC] address)

---



## **IP Address**

1. Used as a way of identifying a host in a network for a period of time.

**Example:**

| Octet1 | Octet2 | Octet3 | Octet4 |
|-------:|-------:|-------:|-------:|
| 192    | 168    | 1      | 1      |

- Total = 32 bits.

- Octet values: 0–255.
  
- **Note** :
   - 0 And 255 are reserved for the networking and broadcast addresses.
   - For ex. : 192.168.1.0 -> net address

- IP Address = 4 Octets = 32 bits

- This no. is calculated through IP addressing/subnetting.

2. IP address can change from one device to another but cannot be active simultaneously more than one within the same network.

3. IP Address allows protocols & forms medium of communication.

4. Depending on where the device is will determine whether public or private IP address.

- Public IP Address → used to identify device on internet.  
- Private IP Address → used to identify device among set of other devices.

5. RFC 1918 defines 3 ranges of Pvt IP addresses:

- 10.0.0.0 – 10.255.255.255 (10/8)  
- 172.16.0.0 – 172.31.255.255 (172.16/12)  
- 192.168.0.0 – 192.168.255.255 (192.168/16)

6. Public IP addresses are given by your ISP at monthly fee.

**Command:**  
```bash
ip a s
```
Used to compare how network card ip address is presented. 
## IPv4 & IPv6

- IPv4 → 2³² IP addresses (~4.29 billion).  
- IPv6 → 2¹²⁸ IP addresses (~340 trillion-trillion).



## MAC Address

Devices on Network → all have physical network interface (a microchip board found on the device’s motherboard).

MAC Address → Network Interface is assigned a unique address at the factory it has built, called a MAC.

- 12 character **hexadecimal number** (a base system numbering system used in computing to represent numbers)  
- Split into two and separated by **colon**   
- Colon are considered separators.

**Example:**  
`00:1A:2B:3C:4D:5E`

- (00:1A:2B) -> Vendor who built the network interface   
- (3C:4D:5E) ->  Unique address of the network interface.  
- They can be faked or spoofed → **K/A Spoofing**.  
- **Spoofing** → network device pretends to identify as another using its MAC address.



## Difference Between IP Address & MAC Address

| IP Address | MAC Address |
|------------|-------------|
| Logical Address | Physical Address |
| Internet / Network-wide | LAN only |
| Easily changeable | No |
| Network/ISP assigned | Manufacturer |
| Send data on basis of location | Send data to specific device |

---

##  Ping 

- **Ping**  → network tool 
- **ICMP** → Internet Control Message Protocol.

Ping uses ICMP packets to determine the performance of a connection between devices and if connection exists & is reliable.

**Example:** if the connection exists & is reliable.

Time taken for ICMP packets travelling b/w devices is measured by **PING**.

Measuring is done using ICMP’s **echo packet** & then ICMP’s **echo reply** from the target device.

**Syntax:**  
```bash
ping IP_Address or Website_URL
```
