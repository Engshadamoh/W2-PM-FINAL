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
This report covers footprinting the networkwalks.com domain using multiple Kali Linux tools (W2-PM1), performing Google Hacking Database searches (W2-PM2), performing footprinting using Maltego (W2-PM3), and scanning my own virtual local network with Zenmap (W2-PM5). Together they show how an attacker moves from gathering public information to mapping live hosts on a network. It is the Week 2 part of my ongoing internship program at Networkwalks.

All commands were run in Kali Linux (footprinting), Windows (Maltego), and on a Windows/Linux PC with Zenmap installed (scanning). Every step below includes the result I observed and a short note on why the finding matters from an attacker's point of view.

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
| **Maltego** | Open Source Intelligence (OSINT) tool used to visualize relationships and extract data like email addresses from a domain. |
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

### 4.3 Footprinting with Maltego (W2-PM3)
I used Maltego to perform footprinting against the target domain networkwalks.com.

* **Target Domain:** I started by searching for the "Domain" entity in the Entity Palette and dragged it to the main graph area. I then renamed the domain to `networkwalks.com`.
* **Email Harvesting:** I right-clicked the domain entity, searched for email-related transforms, and ran the `[Utilities] To Emails @domain [Search Engine]` transform.
* **Result:** The transform successfully harvested email addresses associated with the domain, displaying `info@networkwalks.com` connected to the domain entity in the graph.

### 4.4 Network Scanning with Zenmap (W2-PM5)
For the second activity, I used Zenmap to perform network discovery on my local network.

* Using the command `nmap -sn 10.0.0.2/24`, I performed a Ping scan on my local subnet to identify active hosts.
* The scan successfully identified two live hosts: `10.0.0.1` (s.net) and `10.0.0.2` (localhost).
* After completing the scan, I used the Topology tab to generate a visual network map showing the relationship and connection between my localhost and the router/gateway (s.net).

## 5. Risk Analysis / Impact
Based on the information collected during the footprinting and network scanning activities, I identified the following potential risks.

| # | Risk / Finding | Evidence /
