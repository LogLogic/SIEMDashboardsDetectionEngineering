# SIEM Dashboards & Detection Engineering

This repository showcases real-world SIEM detection engineering projects built using Splunk Enterprise. Each project simulates common SOC analyst workflows—from log ingestion and detection logic to alerting, triage, and dashboard visualizations.

These use cases demonstrate how Splunk can be leveraged for both proactive threat detection and efficient incident response.

---

## 🔍 Projects

### 🛡️ Brute Force Login Detection (Splunk)
📝 [Read the Report](https://github.com/LogLogic/SIEMDashboardsDetectionEngineering/blob/main/BruteForceDetectionSplunk/investigations/alert-report_bruteforce-192.168.1.103.md)

Detects repeated failed login attempts from the same IP address, indicating potential brute force activity. Includes:

- Custom log ingestion (Windows authentication logs)
- Scheduled alert for 5+ failures in 10 minutes
- Dashboard panels for top usernames, IPs, and failures over time
- Investigation report with simulated SOC triage

---

### 🎣 Phishing Email Detection (Splunk)
📝 [Read the Report](https://github.com/LogLogic/SIEMDashboardsDetectionEngineering/blob/main/PhishingEmailAnalysisSplunk/investigations/alert-investigation_phishing_failed-auth_20250705.md)

Detects phishing emails failing SPF, DKIM, and DMARC checks, combined with risk indicators such as suspicious URLs and malware attachments. Includes:

- Custom phishing email logs in CSV format
- Scheduled alert for spoofed emails
- Bar chart dashboards for high-risk senders and attachments
- Investigation report including full email IOC breakdown

---

### 🌍 Unusual Login Geolocation Analysis (Splunk)  
📝 [Read the Report](https://github.com/LogLogic/SIEMDashboardsDetectionEngineering/blob/main/UnusualLoginGeoAnalysis/investigations/unusual_logins_report.md)

Identifies suspicious user logins based on geolocation anomalies such as impossible travel, new IPs, and logins at odd hours. Designed for proactive review of historical data. Includes:

- Manual log ingestion of simulated authentication logs
- SPL queries to detect impossible travel and logins from new countries
- Dashboard visualizations for IP mapping, login heatmaps, and per-user trends
- Investigation report and triage summary
