# Warzone 2 - Investigation Report

- **Platform:**  TryHackMe
- **Category:** Network Analysis
- **Difficulty:** Medium 
- **Author:** Sebastian Strychała
- **Date:** 2026-10-29

---

## 1. Executive Summary
An alert triggered: Misc activity, A Network Trojan Was Detected, and Potential Corporate Privacy Violation. True Positive

---

## 2. Investigation & Analysis Process

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

## 3. Indicators of Compromise (IoCs)

| Indicator | Type | Description / Notes |
| :--- | :--- | :--- |
| `185.118.164[.]8` | IP Address |
| `malicious-domain[.]com` | Domain | Phishing Landing Page |
| `e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855` | SHA256 | Malicious Attachment |

---

## 4. Key Takeaways & Mitigation
- **Findings:** Summary of key evidence discovered.
- **Recommendations:** Steps to mitigate the threat or improve detection in the future.

---


