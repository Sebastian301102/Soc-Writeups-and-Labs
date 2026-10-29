# Warzone 2 - Investigation Report

- **Platform:**  TryHackMe
- **Category:** Network Analysis
- **Difficulty:** Medium 
- **Author:** Sebastian Strychała
- **Date:** 2026-10-29

---

# 1. Executive Summary
Investigation was conducted following network IDS alerts indicating a Trojan infection, Command & Control (C2) activity, and potential corporate privacy violations targeting host `10.6.3.102`. Traffic analysis in Brim (Zeek) and NetworkMiner confirmed active HTTP payload retrievals (including `.cab` file extraction) and established connections to external adversary infrastructure. Cross-referencing extracted IP addresses, malicious domains, and file hashes with VirusTotal confirmed the activity as a **True Positive** security incident.

---

# 2. Investigation & Analysis Process

### 1. Initial Detection and Alert Analysis (Brim)
* **Action:** Performed a PCAP file analysis using Brim. Queried for generated IDS/Suricata alerts (`alert`), then identified and filtered anomalies marked as critical.
* **Result:** Identified two alert signatures associated with Trojan network traffic. Secured defanged source IP addresses and detection signatures.

### 2. HTTP Session Reconstruction and C2 Communication Identification (Brim)
* **Action:** Analyzed network traffic associated with the identified IP address in Brim by filtering HTTP protocol queries (`ip == [DEFANGED_IP] http`).
* **Result:** Extracted full URLs and target host/domain names, indicating a C2 (Command and Control) infrastructure address or a payload download point.

### 3. File Artifact Analysis and Indicator Extraction (NetworkMiner)
* **Action:** Loaded the PCAP file into NetworkMiner to analyze files transferred during the session.
* **Result:** Identified traces of a file download with a `.cab` extension onto the victim's workstation. Extracted the file hashes.

### 4. Data Enrichment and Threat Identification (VirusTotal)
* **Action:** Checked the acquired file hashes against the VirusTotal database.
* **Result:** Confirmed the malicious nature of the file (obtained payload classification) and identified related Indicators of Compromise (IoCs – file hashes, IP addresses, and domains).

### 5. Vector Identification and Client Metadata (User-Agent)
* **Action:** Returned to Brim to extract HTTP header details for the suspicious connection.
* **Result:** Identified the User-Agent string of the browser/application through which the interaction with the malicious resource or the payload download occurred, helping to determine the context of the breach or the profile of the tool used.

### 6. Expanded Alert Analysis and Identification of Associated IP Addresses (Brim / VirusTotal)**
  * **Action:** Performed a deeper correlation of events for the compromised workstation (`10.6.3.102`) by entering a query in Brim combining the host IP with the alert filter (`10.6.3.102 alert`)[cite: 2].
  * **Result:** Identified additional external IP addresses that communicated with the workstation and generated network traffic. Key IP addresses associated with the incident were detected and subsequently verified in the VirusTotal database to evaluate their detection rates and associated malicious domains.

---

# 3. Indicators of Compromise (IoCs)

---

### 1. Victim Information
* **Victim IP Address:** `10.6.3.102`[cite: 1]

### 2. Primary Trojan Infrastructure (C2 / Payload)
* **Attacker IP Address:** `185[.]118[.]164[.]8`[cite: 1]
* **Malicious Domain:** `awh93dhkylps5ulnq-be[.]com`[cite: 1]
* **Malicious File Hash (SHA-256):** `3769a84dbe7ba74ad7b0b355a864483d3562888a67806082ff094a56ce73bf7e`[cite: 1]

### 3. Associated Infrastructure (VirusTotal / Not Suspicious Traffic)
* **Associated IP Addresses:** `64[.]225[.]65[.]166`, `142[.]93[.]211[.]176`[cite: 1]
* **Associated / Malicious Domains:**[cite: 1]
  * `2partscow[.]top`[cite: 1]
  * `safebanktest[.]top`[cite: 1]
  * `tocsicambar[.]xyz`[cite: 1]
  * `ulcertification[.]xyz`[cite: 1]
  
---

# 4. Key Takeaways & Mitigation

### Key Takeaways
* **Successful Payload Acquisition:** The victim workstation (`10.6.3.102`) established inbound/outbound HTTP communication with malicious external infrastructure, resulting in the download of a Trojan payload archive (`.cab` file)[cite: 1, 4].
* **Multi-Stage Domain Infrastructure:** The adversary leveraged a cluster of malicious domains (such as `safebanktest[.]top`, `2partscow[.]top`, and `tocsicambar[.]xyz`) hosted across multiple external IP addresses to deliver payloads and maintain persistence[cite: 1, 4].
* **Evasion & Low-Detection Endpoints:** Some external IP nodes involved in the session exhibited low or zero detection scores on VirusTotal (e.g., `64[.]225[.]65[.]166`), highlighting the reliance on reputation-neutral endpoints to bypass basic perimeter checks[cite: 1, 4].
* **Privacy & Endpoint Exposure:** Client User-Agent strings and network telemetry were exposed during unencrypted HTTP sessions, providing adversaries with specific environment details[cite: 1, 4].

### Mitigation & Recommendations
1. **Host Isolation & Forensics:** Immediately isolate workstation `10.6.3.102` from the network to prevent potential lateral movement and perform a deep forensic disk scan for active registry persistence[cite: 1, 4].
2. **Network Level Blocking (IoC Blacklisting):** Implement perimeter firewall and DNS sinkhole rules to block all communication with the identified malicious domains (`*.top`, `*.xyz`) and IP addresses (`185[.]118[.]164[.]8`, `64[.]225[.]65[.]166`, `142[.]93[.]211[.]176`)[cite: 1, 4].
3. **Payload Signature Deployment:** Import the extracted SHA-256 hash (`3769a84dbe7ba74ad7b0b355a864483d3562888a67806082ff094a56ce73bf7e`) into the Endpoint Detection and Response (EDR) system to block any execution across the enterprise[cite: 1, 4].
4. **Enhanced Suricata/Zeek Detection:** Refine network detection rules to trigger high-priority alerts on unencrypted HTTP archive downloads (`.cab`, `.zip`, `.iso`) originating from top-level domains with low reputation scores[cite: 1, 4].

---


