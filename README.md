# SIEM Dashboards & Detection Engineering

This repository showcases real-world SIEM detection engineering projects built using Splunk Enterprise. Each project simulates common SOC analyst workflows—from log ingestion and detection logic to alerting, triage, and dashboard visualizations.

These use cases demonstrate how Splunk can be leveraged for both proactive threat detection and efficient incident response.

---

## 🔍 Projects

### 🛡️ Brute Force Login Detection (Splunk)
📘 [Read the Report](./investigations/alert-report_bruteforce-192.168.1.103.md)

Detects repeated failed login attempts from the same IP address, indicating potential brute force activity. Includes:

- Custom log ingestion (Windows authentication logs)
- Scheduled alert for 5+ failures in 10 minutes
- Dashboard panels for top usernames, IPs, and failures over time
- Investigation report with simulated SOC triage

---

### 🎣 Phishing Email Detection (Splunk)
📘 [Read the Report](./investigations/alert-report_phishing-email-spoof.md)

Detects phishing emails failing SPF, DKIM, and DMARC checks, combined with risk indicators such as suspicious URLs and malware attachments. Includes:

- Custom phishing email logs in CSV format
- Scheduled alert for spoofed emails
- Bar chart dashboards for high-risk senders and attachments
- Investigation report including full email IOC breakdown
