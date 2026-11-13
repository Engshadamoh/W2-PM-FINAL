# PENETRATION TESTING REPORT
## FOOTPRINTING & NETWORK SCANNING PHASES
**W2-PM-FINAL | CYBERSECURITY | NETWORKWALKS**

| Field | Detail |
| :--- | :--- |
| **Pentester Name (Cybersecurity Professional)** | Shada Mohammed |
| **Program/Batch** | B082-Networkwalks |
| **Date** | 13 November 2026 |
| **Modules completed** | W2-PM1: Footprinting with multiple Kali tools<br>W2-PM2: GHDB based Footprinting Attacks<br>W2-PM3: Maltego based Footprinting Attacks<br>W2-PM5: Zenmap based Network Scanning |
| **Client/Target** | 1. Networkwalks (secured written permission already)<br>2. My own virtual local LAN Network |
| **Permission secured from client?** | Yes |
| **Phases covered** | Phase 1: Reconnaissance & Footprinting<br>Phase 2: Scanning & Network Discovery<br>Phase 3-5: In Progress |

---

## 1. Liability Disclaimer
I have performed these activities only on the systems & devices where I had secured written permission or the devices/systems that I own myself. All these materials are for education and research purpose only. Do not use anything from here to break the law. The instructor, the authors and Networkwalks are not responsible for what you do with this knowledge. Every action you take is your own responsibility. Misuse can lead to criminal charges, heavy fines, loss of your job and a permanent record. In most countries unauthorised access is a crime even when nothing is damaged.

## 2. Introduction
This report covers footprinting the networkwalks.com domain using multiple Kali Linux tools (W2-PM1), performing Google Hacking Database searches (W2-PM2), utilizing Maltego for visual link analysis and OSINT data gathering (W2-PM3), and scanning my own virtual local network with Zenmap (W2-PM5). Together they show how an attacker moves from gathering public information to mapping live hosts on a network. It is the Week 2 part of my ongoing internship program at Networkwalks.

All commands were run in Kali Linux (footprinting) and on a Windows/Linux PC with Zenmap and Maltego installed (scanning and OSINT). Every step below includes the result I observed and a short note on why the finding matters from an attacker's point of view.

## 3. Tools Used
The table below lists each tool used in this report and its purpose.

| Tool | Purpose |
| :--- | :--- |
| **Kali Linux** | Operating systems used for reconnaissance activities |
| **WHOIS** | Find domain registration details (owner, dates, name servers). |
| **Whatweb** | Fingerprint web technologies (server, CMS, plugins, IP). |
| **Nslookup** | Resolve the domain name to its IP address using DNS. |
| **curl -I** | Read the HTTP response headers of the website. |
| **wafw00f** | Detect whether a Web Application Firewall protects the site. |
| **GHDB (Google)** | Use advanced search operators to find exposed devices and files. |
| **Maltego** | Perform OSINT, harvest email addresses, and visualize data relationships. |
| **Zenmap (Nmap GUI)** | Scan the local subnet to find live hosts, IPs and network topology. |

## 4. Activities Performed

### 4.1 Footprinting & Reconnaissance (W2-PM1)
I performed reconnaissance against the networkwalks.com domain using five Kali Linux tools: WHOIS, WhatWeb, Nslookup, Curl, and Wafw00f.

* **WHOIS:** The results provided information about the domain registration. The domain is registered with GoDaddy.com, LLC, was created on 2015-04-16, and will expire on 2027-04-16. The name servers identified were NS35.DOMAINCONTROL.COM and NS36.DOMAINCONTROL.COM.
* **WhatWeb:** The results identified the web server as Apache and the CMS as WordPress 7.1.4 with WP Download Manager 3.3.76. It also identified frontend frameworks like Bootstrap 7.1.4 and JQuery 3.7.1.
* **Nslookup:** I resolved the domain name to its IP address. The provided result identified the server IP as 192.232.216.135.
* **Curl -I:** Inspecting the HTTP response headers showed an HTTP/2 200 response. It also exposed security-related headers like permissions-policy, referrer-policy, and the backend technology content type.
* **Wafw00f:** I used this to determine if a Web Application Firewall was protecting the website. The result successfully identified ModSecurity (SpiderLabs) WAF.

### 4.2 GHDB based Footprinting Attacks (W2-PM2)
Using Google Hacking Database (GHDB) dorks, I performed advanced search queries to footprint exposed assets:

* **Exposed Cameras:** I found 10 live vulnerable security cameras accessible from the internet using dorks like `intitle:"webcamXP" inurl:8080` and `inurl:indexFrame.shtml "Axis Video Server"`.
* **Exposed Documents:** I found 10 listings containing downloadable mathematics ebooks in PDF format by utilizing directory traversal dorks such as `intitle:index.of "parent directory" mathematics pdf`.

### 4.3 Maltego based Footprinting Attacks (W2-PM3)
I successfully installed Maltego and created an account to perform Open-Source Intelligence (OSINT) gathering.
* I started a new graph and added the "Domain" entity, setting the target to `networkwalks.com`.
* I ran the specific transform `To Email addresses [Search Engine]` against the target domain.
* The transform successfully harvested email addresses associated with the domain, displaying `info@networkwalks.com` as a resulting node linked to the main domain.

### 4.4 Network Scanning with Zenmap (W2-PM5)
For the second activity, I used Zenmap to perform network discovery on my local network.

* Using the command `nmap -sn 10.0.0.2/24`, I performed a Ping scan on my local subnet to identify active hosts.
* The scan successfully identified two live hosts: `10.0.0.1` (s.net) and `10.0.0.2` (localhost).
* After completing the scan, I used the Topology tab to generate a visual network map showing the relationship and connection between my localhost and the router/gateway (s.net). *(See attached screenshot).*

## 5. Risk Analysis / Impact
Based on the information collected during the footprinting and network scanning activities, I identified the following potential risks.

| # | Risk / Finding | Evidence / Observation | Potential Impact | Risk Level |
| :-: | :--- | :--- | :--- | :--- |
| **1** | Web technology information exposed | WhatWeb identified WordPress 7.1.4 and WP Download Manager 3.3.76 | Attackers may use exposed technology/version information to identify software requiring further security review | 🟡 Medium |
| **2** | Server IP address identifiable | Nslookup resolved the domain to 192.232.216.135 | Provides information about the network location of the web service | 🟢 Low |
| **3** | WAF technology identifiable | Wafw00f identified ModSecurity (SpiderLabs) | Reveals information about the web application’s security architecture | 🟢 Low |
| **4** | Exposed IoT Devices and Directories | GHDB searches revealed live webcams and open PDF directories | Unauthorized surveillance, data leakage, and access to internal network entry points | 🔴 High (External) |
| **5** | Email Addresses Harvested | Maltego transform extracted `info@networkwalks.com` from public sources | Attackers can use harvested emails for targeted phishing campaigns, social engineering, or brute-force login attempts. | 🟡 Medium |
| **6** | Live hosts visible on local network | Zenmap identified 10.0.0.1 and 10.0.0.2 | Unknown or unauthorized devices may potentially be present on a network | 🟢 Low |

*Risk level key: 🔴 Critical | 🟡 Medium | 🟢 Low*

The risks above are observations from the footprinting and scanning exercises, not confirmed vulnerabilities. The practical exercises primarily involved information gathering and host discovery. No exploitation or vulnerability validation was performed.

## 6. Recommendations
Based on the observations from these activities, I recommend the following security improvements:

* **Review publicly exposed technology information:** Organizations should regularly review what information about their web technologies, CMS and plugins is publicly visible.
* **Keep software updated:** CMS platforms, plugins and other web technologies should be regularly updated.
* **Restrict Search Engine Indexing:** Use `robots.txt` and proper directory access controls to prevent search engines from indexing sensitive webcams and open file directories.
* **Properly configure and monitor the WAF:** Keep the WAF (ModSecurity) enabled and tuned, since it already blocks naive attacks.
* **Implement Email Security & Awareness:** Educate employees about phishing risks since corporate emails are publicly discoverable, and deploy anti-phishing/spam filters.
* **Perform regular internal network discovery:** Organizations should periodically scan their own networks to identify active devices and maintain network documentation.

## 7. Conclusion
During Week 2 of my Cybersecurity & Ethical Hacking internship, I completed practical activities covering footprinting, reconnaissance and network scanning.

In the footprinting activity, I used Kali Linux tools to collect information about the target domain, learning how WhatWeb can identify web technologies, Nslookup can resolve domain names, and Wafw00f can identify a WAF. I also learned how powerful Google Dorks (GHDB) are in uncovering exposed IoT devices and open directories across the internet. Using Maltego, I learned how to visually map relationships and harvest organizational data (like emails) from open sources. In the network scanning activity, I used Zenmap to identify my local network configuration, discover active hosts, and create a network topology.

The exercises showed me that information gathering is an important part of cybersecurity. I learned that reconnaissance and scanning must always be performed within an authorized scope, and these activities were completed as part of the assigned educational cybersecurity lab.

## 8. Evidences Collected
*(Attach screenshots of Zenmap Topology, GHDB Tables, and Maltego Graph here).*
```eof
