z## Notation
- **Plaintext (m)**
- **Ciphertext (C)**
- **Key** $K_a$ , $K_b$
- **Encryption:** `m → C`
- **Decryption:** `C → m`

---
# Symmetric Key Cryptography
**Sender and receiver uses the same key: $K_a = K_b$**

| Caesar's cypher                     | Monoalphabetic cypher                                            | Polyalphabetic Cipher                                                                           |
| ----------------------------------- | ---------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| **Shift alphabet** by fixed number  | Arbitrary **1-to-1 substitution** of 26 letters                  | Use **multiple substitution alphabets**: C1, C2, …, Cn<br>Cycle patterns e.g. `C1, C3, C3, C2…` |
| Key space: **25**                   | Key space: **26! (~$10^{26}$)**                                  | Key space: **$(26!)^n$**                                                                        |
| Trivial to break by **brute force** | **Frequency analysis** (letter frequency patterns are preserved) | Stronger than monoalphabetic but still breakable with analysis                                  |
### Modern Block Ciphers
- Break message into **K-bit blocks**
- Each block encrypted independently

| DES (Data Encryption Standard)                                       | **AES (Advanced Encryption Standard)**                                                   |
| -------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| **56-bit key**, **64-bit block**<br>**3DES:** Encrypt 3× with 3 keys | **128/192/256-bit key, 128-bit blocks**                                                  |
| **Key space: $2^{56}$**<br>Broken in < 1 day (thus now deprecated)   | **Key space: $2^{128}$ / $2^{192}$ / $2^{256}$**<br>Would take ~$10^{14}$ years to break |
### Weakness
1. Requires an **a priori** mode of exchange of keys

---
# Asymmetric Key Cryptography
**Sender and receiver uses different key: $K_a \ne K_b$
### Public Key Encryption
Requirements:
1. Needs both **private and public key** for decryption: $m=K_{a}^-{K_{a}^+(m)}$
2. Impossible to compute **private key** from **public key**
### Rivest, Shamir, Adleman (RSA) algorithm
#### Keygen algorithm
1. Choose two large primes: `p, q`
2. Compute $n=p \times q$
3. Compute $z=(p-1)(q-1)$
4. Choose public exponent $e \gt n$ that has no common factors with `n`
5. Compute private exponent `d` such that $(ed-1)\text{mod}(z)=0$ 
6. $K^+=(n,e)$ , $K^-=(n,d)$

**Encrypt:** $c=m^e\text{mod}(n)$
**Decrypt:** $m=c^d\text{mod}(n)$
## Public Key Distribution
Attackers can impersonate and send their own public key → need to verify ownership
1. Public announcement
2. Publicly available directory
3. Public Key Infrastructure (PKI)
### Public Key Infrastructure (PKI)
Involves
1. **Certificate**
   Digital document that contains
	- Identity of an owner e.g. google.com
	- Public key of owner
	- Time window that the certificate is valid
	- Signature of CA
2. **Certification Authority (CA)**
   Issues and signs digital certificates to entities
   Maintain a directory of public keys