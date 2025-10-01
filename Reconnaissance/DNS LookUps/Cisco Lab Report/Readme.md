# Lab Report – DNS Lookups on Cisco.com

![image for dnsrecon](https://github.com/Fabelt14/Pen-Testing-Journey/blob/main/Reconnaissance/Images/dnsrecon%201.jpg)

## 🔹 Introduction
This lab focused on performing **DNS reconnaissance** to gather information about domain names, their associated IP addresses, mail servers, and registration details.  
DNS lookups are an essential step in **footprinting and enumeration** during ethical hacking. By analyzing DNS records, attackers or defenders can understand the infrastructure of a target domain and identify potential attack vectors.

---

## 🔹 Setup
- **Operating System**: Kali Linux (root environment)  
- **Tools Used**:  
  - `dnsrecon` – Automated DNS enumeration  
  - `dig` – Manual DNS querying  
  - `whois` – Domain registration lookup  
  - `nslookup` – Interactive DNS resolver queries  
- **Target Domains Tested**:  
  - `h4cker.org`  
  - `tesla.com`  
  - `cisco.com`  
  - `netacad.com`  

---

## 🔹 Steps Performed
1. **General DNS Enumeration**  
   - Ran `dnsrecon` on `h4cker.org` to gather DNSSEC, SOA, NS, MX, A, and TXT records.  

2. **Manual A Record Lookup**  
   - Used `dig h4cker.org` to verify IP addresses returned by the domain.  

3. **WHOIS Lookup**  
   - Queried `h4cker.org`, `tesla.com`, and `cisco.com` for registrar, creation/expiry dates, DNSSEC status, and domain ownership details.  

4. **Interactive Queries with nslookup**  
   - Retrieved `A`, `NS`, and `ANY` records for `cisco.com` and `netacad.com`.  
   - Switched to **Google DNS (8.8.8.8)** for external lookups.  

---

## 🔹 Results

![image for nslookup](https://github.com/Fabelt14/Pen-Testing-Journey/blob/main/Reconnaissance/Images/nslookup.jpg)

### `h4cker.org`
- **A Records**:  
  - 185.199.108.153  
  - 185.199.109.153  
  - 185.199.110.153  
  - 185.199.111.153  
- **NS Records**:  
  - ns-cloud-c1.googledomains.com  
  - ns-cloud-c2.googledomains.com  
  - ns-cloud-c3.googledomains.com  
  - ns-cloud-c4.googledomains.com  
- **MX Records**:  
  - mxa.mailgun.org  
  - mxb.mailgun.org  
- **TXT Record**:  
  - `v=spf1 include:mailgun.org ~all`  
- **WHOIS**: Registered with Squarespace, expires May 2028, DNSSEC enabled.  

---

### `tesla.com`
- Registrar: MarkMonitor Inc.  
- Creation Date: 1992-11-04  
- Expiry Date: 2026-11-03  
- Status: Multiple protections (client/server update/transfer/delete prohibited)  
- Name Servers: Akamai & UltraDNS infrastructure  
- DNSSEC: **Unsigned**  

---

### `cisco.com`
- Registrar: MarkMonitor Inc.  
- Creation Date: 1987-05-14  
- Expiry Date: 2026-05-15  
- Name Servers: Akamai + Cisco’s own (`ns1.cisco.com`, `ns2.cisco.com`, `ns3.cisco.com`)  
- WHOIS Contact: `infosec@cisco.com`  
- DNSSEC: **Unsigned**  

---

### `netacad.com`
- **A Records**: 3.231.78.22, 54.196.0.42  
- **NS Records**: Amazon AWS (`awsdns` nameservers)  
- **MX Records**: Google Mail (aspmx.l.google.com + alternates)  
- **TXT Records**: Multiple entries for SPF, DKIM, Google verification, and Facebook verification  
- SOA Record: Hosted on Amazon AWS  

---

## 🔹 Analysis
- **h4cker.org** uses **Google DNS infrastructure** with **Mailgun for email services** and enforces DNSSEC, making it more resistant to DNS spoofing.  
- **tesla.com** and **cisco.com** use **enterprise-grade registrars (MarkMonitor)** with strong domain protection, but both **lack DNSSEC**, which could be a minor security concern.  
- **netacad.com** relies heavily on **AWS + Google services**, indicating a cloud-based infrastructure. Multiple TXT records confirm **SPF/DKIM validation** for secure email handling.  

This demonstrates how DNS lookups can reveal **mail servers, hosting providers, security practices (DNSSEC, SPF, DKIM), and third-party dependencies**.

---

## 🔹 Conclusion
This lab reinforced my understanding of:
- How to perform **DNS reconnaissance** using multiple tools (`dnsrecon`, `dig`, `whois`, `nslookup`).  
- Identifying **domain ownership, expiration, and registrar details** through WHOIS.  
- Recognizing **security configurations** like DNSSEC, SPF, and MX setups.  
- Mapping **infrastructure providers** (Google Domains, Mailgun, Akamai, AWS, etc.).  

📌 **Key Takeaway**: DNS lookups are a critical early step in ethical hacking and penetration testing. They provide valuable insights into the target’s infrastructure, which can later inform **attack surfaces or defensive measures**.  
