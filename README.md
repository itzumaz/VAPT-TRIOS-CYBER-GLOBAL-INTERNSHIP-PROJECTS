# 30-Day Web Application Vulnerability Assessment & Penetration Testing (VAPT) Portfolio

## 📌 Intern & Assessment Overview
* **Lead Penetration Tester:** Azeez Umar Opeyemi
* **Academic Program:** Undergraduate Cybersecurity Science, LAUTECH
* **Host Organization:** TriosCyber Internship Program (in partnership with Ernith)
* **Target Security Lab:** Local Virtual Staging Range / DVWA / OWASP Broken Web Applications VM
* **Primary Core Toolkit:** Kali Linux, Nmap, FFUF, Nikto, Burp Suite Community Edition, Wireshark, Metasploit, Python

---

## 📝 Executive Summary
This repository contains the complete technical portfolio, daily milestone logs, command execution outputs, and final capstone deliverables compiled over an intensive **30-Day Web Application VAPT Internship**. 

The curriculum progressed systematically from low-level network protocol analysis and secure lab compilation to active infrastructure reconnaissance, directory fuzzing, automated vulnerability scanning, manual payload engineering, and formal executive threat reporting. The engagement culminated in a fully scoped security assessment against the **Damn Vulnerable Web Application (DVWA)**, modeling an industry-standard penetration testing lifecycle.

---

## 🗺️ 30-Day Internship Roadmap & Syllabus

```plaintext
+-----------------------------------------------------------------------------------+

|                           30-DAY VAPT INTERNSHIP ROADMAP                          |
+-----------------------------------------------------------------------------------+

| Days 01–05: Network Protocols, Traffic Analysis & Lab Environment Setup           |
| Days 06–10: Linux CLI Administration, Shell Scripting & Port Scanning (Nmap)      |
| Days 11–15: Web Fundamentals, HTTP/HTTPS Interception & Burp Suite Basics        |
| Days 16–20: Content Fuzzing (FFUF), Vulnerability Scanning & OWASP Top 10 Review  |
| Days 21–25: Exploitation (OS Command Injection, Reflected XSS, SQLi Basics)       |
| Days 26–28: Web Security Hardening, Remediation Logic & Defensive Controls       |
| Days 29–30: Scoped DVWA VAPT Execution & Executive Technical Reporting            |
+-----------------------------------------------------------------------------------+
```
*[Source: Internship Milestone Syllabus]*

### Phase 1: Fundamentals, Traffic Interception & Range Deployment (Days 01–05)
* **Day 01:** Introduction to Web Application Penetration Testing (VAPT) methodology and legal ethics.
* **Day 02:** Setting up Kali Linux and target VMs (DVWA / OWASP BWA) in VirtualBox/VMware environments.
* **Day 03:** Networking fundamentals: TCP/IP stack, OSI model, subnetting, and port mapping.
* **Day 04:** Traffic interception and packet analysis using Wireshark (capturing HTTP vs. HTTPS traffic).
* **Day 05:** Analyzing web protocol headers, status codes (200, 301, 403, 500), and session state management.

### Phase 2: Host Reconnaissance, Content Discovery & Scanning (Days 06–10)
* **Day 06:** Command-line administration in Kali Linux and bash scripting for automation.
* **Day 07:** Network service mapping with Nmap (-sS, -sT, -sV, -O, -sC).
* **Day 08:** Advanced Nmap scripting engine (NSE) usage for basic vulnerability detection.
* **Day 09:** Web application directory and endpoint fuzzing using FFUF and Gobuster.
* **Day 10:** Automated web application scanning with Nikto v2.6.1 to audit missing security headers and configuration flaws.

### Phase 3: Proxy Manipulation & OWASP Top 10 Threat Modeling (Days 11–20)
* **Day 11:** Configuring Burp Suite Community Edition proxy listener and browser certificates.
* **Day 12:** Intercepting and modifying HTTP GET/POST requests in Burp Proxy.
* **Day 13:** Manipulating request headers, user-agents, and session cookies via Burp Suite Repeater.
* **Day 14:** Overview of the OWASP Top 10 vulnerability classification framework.
* **Day 15:** Understanding injection flaws: SQL Injection (SQLi) mechanics and testing logic.
* **Day 16:** Understanding client-side security risks: Cross-Site Scripting (XSS) categories (Reflected, Stored, DOM).
* **Day 17:** Understanding broken access control, parameter tampering, and session fixation.
* **Day 18:** Exploring file upload vulnerabilities and web shell execution paths.
* **Day 19:** Examining OS Command Injection mechanics and command separator operators (|, ;, &&).
* **Day 20:** Mid-internship assessment: Combining FFUF discovery with Burp Repeater parameter analysis.

### Phase 4: Practical Exploitation, Hardening & Final Capstone (Days 21–30)
* **Day 21:** Manual testing for Reflected XSS payloads using unescaped JavaScript tags.
* **Day 22:** Exploiting OS Command Injection on target endpoints (POST /vulnerabilities/exec/).
* **Day 23:** Analyzing security headers (Content-Security-Policy, X-Frame-Options, X-Content-Type-Options).
* **Day 24:** Developing remediation strategies: Input validation allowlists vs. context-aware output encoding.
* **Day 25:** Reviewing vulnerability severity scoring systems (CVSS v3.1 calculation).
* **Day 26:** Formulating technical findings into structured bug reporting templates.
* **Day 27:** Pre-capstone lab setup: Re-evaluating target state (192.168.145.134) on OWASP BWA.
* **Day 28:** Executing complete scoped discovery across target endpoints (/exec/, /xss_r/, /upload/).
* **Day 29:** Scoped Application VAPT: Running full-suite tests using Nmap, FFUF, Nikto, and Burp Repeater.
* **Day 30:** Final Practical VAPT Deliverable: Consolidating findings into a professional technical report.

---

## 🚀 Capstone Final Engagement Execution Summary (Days 29–30)

### Phase 1: Active Reconnaissance & Port Fingerprinting (Nmap)
Executed targeted port scanning and service detection against web infrastructure ports 80 and 443:
```bash
nmap -sC -sV -p 80,443 192.168.145.134
```

* **Findings:** Fingerprinted Apache 2.2.14 (Ubuntu) with PHP 5.3.2 . Discovered that the HTTP TRACE method is enabled on web ports .

### Phase 2: Authenticated Endpoint Discovery (FFUF)
Enumerated directory structures under /dvwa/vulnerabilities/ using authenticated session cookies :
```bash
ffuf -w /usr/share/wordlists/dirb/common.txt -u http://192.168.145 -mc 200,301,302 -b "security=low; PHPSESSID=c8q05ruidd0b6otc507c15co0"
```

* **Findings:** Located multiple valid operational testing endpoints: `/exec/` (Command Execution), `/xss_r/` (Reflected XSS), `/upload/` (File Upload), and `/captcha/` .

### Phase 3: Automated Vulnerability Assessment (Nikto)
Executed baseline web configuration auditing :
```bash
nikto -h http://192.168.145 -o day29_nikto.txt
```

* **Findings:** Identified missing HTTP security headers (Content-Security-Policy, X-Frame-Options) and exposed server directories (/config/, /docs/) .

### Phase 4: Manual Exploitation & Technical Verification

#### 🔴 Vector 1: OS Command Injection (`POST /dvwa/vulnerabilities/exec/`) 
* **Payload Executed:** `172.0.0.1%3B+whoami%3B+uname+-a` 
* **Telemetry Finding:** Confirmed Remote Code Execution (RCE) . Backend executed injected shell operators, returning www-data and kernel release details in the response .
* **Severity Ranking:** **High (CVSS v3.1: 9.8)** 

#### 🟡 Vector 2: Reflected Cross-Site Scripting (`GET /dvwa/vulnerabilities/xss_r/`) 
* **Payload Executed:** `<script>alert(1)</script>` 
* **Telemetry Finding:** Confirmed Reflected XSS . Script tags reflected verbatim into the DOM without sanitization, triggering script execution .
* **Severity Ranking:** **Medium (CVSS v3.1: 6.1)** 

---

## 📊 Consolidated Internship Vulnerability Matrix

| Vulnerability Title | Affected Endpoint / Scope | Severity | Exploitation Status | Risk Impact |
| :--- | :--- | :---: | :---: | :--- |
| **OS Command Injection** | `POST /dvwa/vulnerabilities/exec/` `(ip)` | **High (9.8)** | Confirmed RCE | System takeover & arbitrary command execution. |
| **Reflected XSS** | `GET /dvwa/vulnerabilities/xss_r/` `(name)` | **Medium (6.1)** | Confirmed XSS | Session hijacking & client-side script execution. |
| **Verbose Banners & TRACE Method** | Server Headers / Web Ports 80/443 | **Info (3.7)** | Confirmed | Software profiling & potential XST attacks. |

---

## 🛡️ Strategic Actionable Remediation Roadmap

1. **OS Command Injection Mitigation:**
   * Replace raw shell calls (system(), exec()) with native function calls.
   * Enforce strict IP format allowlists using regex or `filter_var($ip, FILTER_VALIDATE_IP)`.
2. **Reflected XSS Mitigation:**
   * Enforce context-aware HTML entity encoding on dynamic user output (e.g., `htmlspecialchars()`).
   * Implement a strict Content-Security-Policy (CSP) header.
3. **Web Server Hardening:**
   * Disable HTTP TRACE in Apache configuration (`TraceEnable Off`).
   * Suppress version disclosure tokens (`ServerTokens Prod`, `ServerSignature Off`).

---

---

## 👤 Author
**Azeez Umar Opeyemi**
* 💼 **Role:** VAPT Intern at TriosCyber
* 📧 **Email:** umaropeyemiazeez@gmail.com
* 🐙 **GitHub:** [itzumaz](https://github.com/itzumaz)
* 🔗 **LinkedIn:** [Azeez Umar Opeyemi](https://www.linkedin.com/in/azeez-umar-opeyemi-201a433a4/)