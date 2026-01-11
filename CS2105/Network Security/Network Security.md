## 1. Threat Model
### **Passive attack**s
- **Eavesdropping**
- **Reading unencrypted data**

### **Active attack**s
- **Modify / Forge / Delete packets**
- **Man-in-the-middle:** hijack ongoing connection by removing sender / receiver
- **Impersonation:** spoof source address in packet / any fields
- **Denial-of-service:** prevent service from being used by others

---

# ## 2. Security Goals (CIA + A)

- **Confidentiality:** prevent **unauthorized access** to **sensitive information** → encryption
- **Integrity:** verify **trustworthiness** of data from **unauthorized modification**
- **Authenticity:** verify sender of data is **genuine**
- **Availability:** data is **readily accessible to authorized parties**→ firewall / DoS protection
- **Non-repudiation:** sender cannot later deny (repudiate) sending a message

### MAC vs Digital Signature 

| Feature                  | MAC           | Digital Signature |
| ------------------------ | ------------- | ----------------- |
| Key                      | Shared secret | Private/Public    |
| Integrity                | ✔             | ✔                 |
| Authentication           | ✔             | ✔                 |
| Non-repudiation          | ❌             | ✔                 |
| Third-party verification | ❌             | ✔                 |
| Performance              | Fast          | Slower            |
