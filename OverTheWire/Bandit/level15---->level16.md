# 🔐 Bandit Level: Bandit15 ➝ Bandit16



### 📂 Command(s) used: 
👉 
- openssl s_client -connect localhost:30001
- 8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo

<img width="460" height="128" alt="Screenshot 2025-08-20 at 5 19 46 PM" src="https://github.com/user-attachments/assets/17d18c40-1ea9-41bc-94f0-074c4353f4bf" />
<img width="462" height="121" alt="Screenshot 2025-08-20 at 5 20 05 PM" src="https://github.com/user-attachments/assets/b84d08f2-d928-4ebb-89ff-bceb26dec82a" />



---
   
### 📄 Password found:
👉
kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx






---

## 🧠 Notes / What I learned:
👉 
### Command Used (explain)
**openssl s_client -connect localhost:30001**
- **openssl s_client**: This establishes a TLS handshake and gives you an interactive prompt.
- **Port 30001** → Requires SSL/TLS.

---
# 📝 OpenSSL – Full Explanation



## 🔑 What is OpenSSL?
- **OpenSSL = Open + Secure Sockets Layer**
- Open-source toolkit that implements **SSL (Secure Sockets Layer)** and **TLS (Transport Layer Security)** protocols.  
- Two main purposes:  
  - **Cryptography** → encryption, decryption, hashing, key generation  
  - **Network Security** → TLS/SSL handshake, secure client/server communication  

👉 In short: if you want to secure communication or manage certificates, **OpenSSL is the go-to tool**.



## ⚙️ Important Uses of OpenSSL

### 🔹 Testing TLS/SSL connections
``` bash
openssl s_client -connect google.com:443
```
👉 Shows certificates, handshake, and lets you send raw HTTPS requests.  


### 🔹 Encrypt / Decrypt messages (AES-256)
``` bash
echo "hello bro" | openssl enc -aes-256-cbc -a -salt -pass pass:secret123
```
👉 Output = Encrypted text.  

To decrypt:
``` bash
echo "<encrypted_text>" | openssl enc -aes-256-cbc -a -d -pass pass:secret123
```


### 🔹 Generate a private key
``` bash
openssl genrsa -out private.pem 2048
```
👉 Creates a 2048-bit RSA private key.  



### 🔹 Generate a public key from private key
``` bash
openssl rsa -in private.pem -pubout -out public.pem
```
👉 Extracts the public key.  



### 🔹 Generate a Certificate Signing Request (CSR)
``` bash
openssl req -new -key private.pem -out request.csr
```
👉 CSR is what you send to a Certificate Authority (CA) to get an SSL certificate.  



### 🔹 Generate a Self-Signed Certificate
``` bash
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -nodes
```
👉 Creates **cert.pem** (certificate) and **key.pem** (private key).  



### 🔹 Check certificate details
``` bash
openssl x509 -in cert.pem -text -noout
```


## 🖥 Practical Examples

### Example 1: Test HTTPS manually
``` bash
openssl s_client -connect example.com:443
```
Inside the connection, type:
``` bash
GET / HTTP/1.1
Host: example.com
```
👉 You’ll see the server’s HTTP response.  



### Example 2: Send a password to a secure server 
``` bash
cat /etc/bandit_pass/bandit16 | openssl s_client -connect localhost:30001 -quiet
```


### Example 3: Hashing (SHA256)
``` bash
echo -n "mypassword" | openssl dgst -sha256
```
👉 Outputs the SHA256 hash.  



## 🧠 Key Points to Remember
- `s_client` → works like a secure netcat for SSL/TLS  
- `enc` → used for encryption/decryption  
- `genrsa` / `rsa` → used for key generation  
- `req` → for creating CSR & certificates  
- `x509` → for viewing and managing certificates  
---
# 📝 TLS / SSL – Full Explanation



## 🔑 What is SSL/TLS?

- **SSL (Secure Sockets Layer)** → Old protocol (now deprecated).  
- **TLS (Transport Layer Security)** → Modern, secure version of SSL.  

👉 Both are cryptographic protocols used to provide **secure communication** over the internet.  



## ⚙️ Main Goals of TLS/SSL

1. **Encryption** → Protects data so outsiders can’t read it.  
2. **Authentication** → Confirms the server (and sometimes client) is real.  
3. **Integrity** → Makes sure data isn’t changed in transit.  



## 📡 How TLS/SSL Works (Handshake)

1. **Client Hello** → Client says “Hello” + sends supported ciphers + random number.  
2. **Server Hello** → Server chooses cipher + sends its certificate (with public key).  
3. **Certificate Validation** → Client checks server certificate via CA.  
4. **Key Exchange** → Client & Server agree on a shared session key (using RSA or Diffie-Hellman).  
5. **Session Key Established** → All further communication is encrypted using symmetric encryption (faster).  

👉 In short: Public key (asymmetric) used only at start, then symmetric encryption used for speed.  



## 🛠 TLS/SSL Versions

- **SSL 2.0 & 3.0** → Insecure, deprecated.  
- **TLS 1.0 & 1.1** → Weak, also deprecated.  
- **TLS 1.2** → Very common, secure.  
- **TLS 1.3** → Latest, fastest, more secure.  



## 🔐 Cipher Suites

A **cipher suite** = combination of algorithms used in TLS.  
Example:  
``` bash
TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
```
Breakdown:  
- `ECDHE` → Key exchange (Elliptic Curve Diffie-Hellman Ephemeral)  
- `RSA` → Authentication method  
- `AES_256_GCM` → Encryption algorithm  
- `SHA384` → Hashing for integrity  



## 🖥 Practical Examples with OpenSSL

### 🔹 Test SSL/TLS connection
``` bash
openssl s_client -connect example.com:443
```
### 🔹 Check TLS version support
``` bash
openssl s_client -connect example.com:443 -tls1_2
openssl s_client -connect example.com:443 -tls1_3
```
### 🔹 View certificate chain
``` bash
openssl s_client -connect example.com:443 -showcerts
```


## 🧠 Key Points to Remember

- SSL = old, insecure → **don’t use it**.  
- TLS = modern replacement.  
- TLS uses both:
  - **Asymmetric encryption** (RSA, ECDHE) → for key exchange.  
  - **Symmetric encryption** (AES, ChaCha20) → for fast secure communication.  
- Always prefer **TLS 1.2 or 1.3**.  
- Certificates prove server identity (issued by Certificate Authorities).  



