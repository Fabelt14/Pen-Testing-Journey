# 🔎 OSINT Reconnaissance with SpiderFoot (Target: h4cker.org)

![Alt text](path/to/your/image.png)


## 📌 Introduction
As part of my cybersecurity learning journey, I conducted an **OSINT reconnaissance project** using [SpiderFoot](https://www.spiderfoot.net/). The goal was to simulate real-world footprinting activities carried out during the reconnaissance phase of penetration testing.  
Target: **h4cker.org** (training/demo environment).  
Approach: **Passive footprinting only** to avoid unauthorized scanning.

---

## 🛠️ Tools & Methodology
- **Tool Used:** SpiderFoot (GUI)
- **Scan Type:** Footprint (Passive Recon)
- **Process:**
  1. Configured a new scan on `h4cker.org`.
  2. Selected the *Footprint* use case.
  3. Ran scan and monitored results across DNS, WHOIS, SSL/TLS, and OSINT modules.
  4. Categorized findings by reconnaissance value.

---

## 📊 Key Findings

### 1. Email Gateway (MX Records)
- **Finding:** Email routed through Mailgun.
- **Risk:** Phishing/spoofing if SPF/DKIM/DMARC not strict.
- **Recommendation:** Enforce SPF/DKIM/DMARC policies.

### 2. WHOIS Data
- **Finding:** Domain registrar: Squarespace, expiry 2028, privacy enabled.
- **Risk:** Minimal – good hygiene.
- **Recommendation:** Continue domain monitoring and renewals.

### 3. IPv6 Addresses
- **Finding:** IPv6 addresses assigned.
- **Risk:** Often overlooked in firewall rules.
- **Recommendation:** Audit IPv6 configs and firewall coverage.

### 4. SSL/TLS Certificates
- **Finding:** Let’s Encrypt certs for `h4cker.org`, `web.h4cker.org`, `websploit.h4cker.org`.
- **Risk:** Exposed dev/test subdomains increase attack surface.
- **Recommendation:** Restrict staging environments, audit subdomains.

### 5. OSINT Correlation
- **Finding:** SSL transparency data linked to **Omar Santos**.
- **Risk:** Could enable targeted social engineering.
- **Recommendation:** Minimize personal identifiers in certificates.

---

## 🚀 Conclusion
This project demonstrates how **passive OSINT tools** like SpiderFoot reveal critical details: third-party services, subdomains, domain ownership, and certificate history. Even without active probing, attackers could leverage this data to plan intrusions.  

I learned how to:  
- Conduct structured OSINT scans.  
- Translate raw technical data into actionable insights.  
- Present findings in a professional cybersecurity report.  

---

## 📂 Portfolio
- 🔗 [LinkedIn Post](#)  
- 📝 [Hashnode Blog](#)  


