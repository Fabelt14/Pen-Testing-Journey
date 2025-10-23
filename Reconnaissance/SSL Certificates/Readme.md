# SSL Certificate Recon on netacad.com


## 1. Introduction  
This lab focused on collecting public SSL/TLS certificate information for **netacad.com**.  
I recorded certificate details, supported TLS versions and cipher suites, certificate validity dates, and performed a basic Heartbleed check.  
All activity was **passive** and did not involve any intrusion or exploitation.

![Quick Demo GIF](https://github.com/Fabelt14/Pen-Testing-Journey/blob/main/Reconnaissance/Images/sslscan%20on%20netacad.gif)

---

## Tools used

* Kali Linux (lab VM)
* Browser certificate viewer (Chrome)
* `crt.sh` (Certificate Transparency lookup)
* `sslscan` (certificate and TLS analysis)
* `aha` (capture colorized terminal output to HTML)
---

## Commands I ran

```bash
# update and install helper
sudo apt update
sudo apt install -y aha

# capture colorized html and raw output
sslscan netacad.com | aha > sfa_cert.html

```


---

## What I did (concise steps)

1. Confirmed lab scope and used only passive collection.
2. Opened `https://netacad.com` in browser and inspected the certificate via the padlock UI. Saved screenshot.
3. Queried Certificate Transparency logs on `crt.sh` for historical certificate issuances and saved screenshots of relevant entries.
4. Ran `sslscan netacad.com` to collect supported protocols, cipher suites, server key groups, certificate signature and key strength, validity dates, and Heartbleed checks. Exported raw text and a colorized HTML with `aha`.
5. Compiled findings and practical remediation recommendations.

![image for crt.sh](https://github.com/Fabelt14/Pen-Testing-Journey/blob/main/Reconnaissance/Images/crt.shII.jpg)

![image for crt.sh](https://github.com/Fabelt14/Pen-Testing-Journey/blob/main/Reconnaissance/Images/crt.sh.jpg)

---

## Key technical findings

1. Certificate subject and SANs

   * Subject: `netacad.com`
   * SANs: `DNS:netacad.com, DNS:*.netacad.com`
   * Issuer: `Amazon RSA 2048 M02`

2. Certificate validity window

   * Not valid before: `7/24/25, 1:00:00 AM GMT+1`
   * Not valid after:  `8/23/26, 12:59:59 AM GMT+1`

3. Key and signature

   * Signature Encryption Algorithm: `SHA-256 With RSA Encryption`
   * RSA key strength: `2048` bits

4. Protocols and ciphers (summary)

   * TLS versions observed: TLSv1.0 enabled, TLSv1.1 enabled, TLSv1.2 enabled, TLSv1.3 disabled.
   * Preferred strong cipher for TLSv1.2: `ECDHE-RSA-AES128-GCM-SHA256` (curve P-256).
   * Legacy protocols TLSv1.0 and TLSv1.1 are enabled and should be disabled.

5. Heartbleed check

   * TLSv1.0 / TLSv1.1 / TLSv1.2: not vulnerable to Heartbleed.
  
![image for certviewer](https://github.com/Fabelt14/Pen-Testing-Journey/blob/main/Reconnaissance/Images/cert%20viewer%20.jpg)


---

## Immediate impact / severity (short)

* **High:** Legacy TLS versions (TLSv1.0 and TLSv1.1) are enabled. These protocols are deprecated and increase attack surface for downgrade and legacy attacks.
* **Medium:** TLSv1.3 is disabled. Enabling TLSv1.3 improves security and performance.
* **Low:** Certificate uses RSA-2048 and SHA-256. Acceptable today but ECDSA could be considered for performance and stronger elliptic curve usage depending on server ecosystem.

---


## 5. Analysis  
- The certificate metadata and key strength (RSA 2048) meet modern security standards.  
- **Main issue:** TLSv1.0 and TLSv1.1 are still enabled — these are outdated and weaken overall security.  
- Enabling **TLSv1.3** would enhance both performance and encryption strength.  
- No certificate chain or Heartbleed vulnerabilities were identified.

---

## 6. Conclusion  
This lab reinforced my understanding of:  
- How to collect and analyze SSL/TLS certificate data using multiple tools.  
- Identifying supported TLS versions, cipher suites, and certificate validity details.  
- Recognizing configuration weaknesses such as deprecated protocol support.  

**📌 Key Takeaway:**  
Disable old TLS versions, enable TLSv1.3, and prefer modern AEAD ECDHE cipher suites to improve encryption strength and minimize the attack surface.
