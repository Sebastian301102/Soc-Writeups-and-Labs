# [Lab / Room Name] - Investigation Report

- **Platform:** [e.g., TryHackMe / Blue Team Labs Online]
- **Category:** [e.g., Phishing / DFIR / Network Traffic]
- **Difficulty:** [Easy / Medium / Hard]
- **Author:** Sebastian Strychała
- **Date:** YYYY-MM-DD

---

## 1. Executive Summary
Briefly explain what this investigation was about, what triggered the analysis, and the final verdict (e.g., *False Positive*, *True Positive / Malicious Email*).

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
| `192.168.1[.]100` | IP Address | C2 Server IP |
| `malicious-domain[.]com` | Domain | Phishing Landing Page |
| `e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855` | SHA256 | Malicious Attachment |

---

## 4. Key Takeaways & Mitigation
- **Findings:** Summary of key evidence discovered.
- **Recommendations:** Steps to mitigate the threat or improve detection in the future.

---

```bash
# Example command or query used during analysis
