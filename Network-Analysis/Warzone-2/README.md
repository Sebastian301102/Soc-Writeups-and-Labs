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

### Step 1: Initial Triage
Describe the first steps, initial artifacts, or context provided in the alert.

### Step 2: Technical Deep Dive
Detail your step-by-step analysis using tools (e.g., Wireshark, CyberChef, Volatility). Include screenshots where helpful.

---

## 3. Indicators of Compromise (IoCs)
*Note: Always defang URLs and IPs (e.g., using `[.]` instead of `.`).*

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


