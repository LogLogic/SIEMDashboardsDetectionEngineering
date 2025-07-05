# SOC Alert Investigation Report – Phishing Email Detection

**Analyst:** Olga Zaytseva  
**Date:** 07.05.2025 
**Platform:** Splunk Enterprise  
**Alert:** Phishing - Failed SPF DKIM DMARC  
**Severity:** High  
**Description:** Detects phishing emails failing SPF, DKIM, and DMARC authentication checks, indicating potential spoofing or malicious intent.

---

## 1. Alert Summary

- **Triggered At:** 2025-07-05 09:30 AM  
- **Search Logic:**

index=phishing sourcetype=phishing_email_logs
| where spf_result="fail" AND dkim_result="fail" AND dmarc_result="fail"

## Condition Met:
Email(s) failed all three authentication checks (SPF, DKIM, DMARC).

Example Alerted Email:
From support@microsft.com with subject “Important security update”.

![Triggered Alerts View](https://github.com/LogLogic/SIEMDashboardsDetectionEngineering/blob/main/PhishingEmailAnalysisSplunk/screenshots/alert_triggered_phishing_fail_auth.png)
Example: Screenshot from Triggered Alerts showing phishing alert fired due to failed SPF/DKIM/DMARC checks.

## 2. Log Sample – Suspicious Email

2025-07-05T14:30:00Z,phish-2398475,support@microsft.com,40.76.54.120,support@microsft.com,user@company.com,"Important security update","from microsft.com (40.76.54.120) by mx.company.com",fail,fail,fail,https://microsoft.com/update,http://malicious-xyz.com/update,TRUE,update.exe,malicious,malicious,positive,5,"security;update;urgent",90,high

From Address: support@microsft.com

Source IP: 40.76.54.120

To Address: user@company.com

Subject: Important security update

SPF / DKIM / DMARC: All failed

Displayed URL: https://microsoft.com/update

Actual URL: http://malicious-xyz.com/update

Attachment: update.exe

Attachment/Link Reputation: Positive (malware)

Tags: security, update, urgent

Risk Score: 90

Priority: High

Log sample taken from Search view.

## 3. Triage Analysis

| Indicator               | Value                                               | Notes                            |
| ----------------------- | --------------------------------------------------- | -------------------------------- |
| Source IP               | 40.76.54.120                                        | Potentially spoofed Microsoft IP |
| From Address            | [support@microsft.com](mailto:support@microsft.com) | Misspelled domain (typo-squat)   |
| SPF / DKIM / DMARC      | fail / fail / fail                                  | Strong spoofing indicators       |
| Displayed vs Actual URL | Mismatch                                            | Malicious redirect               |
| Attachment Name         | update.exe                                          | Detected as malware              |
| Threat Reputation       | Positive                                            | Confirmed via VirusTotal         |

## 4. Verdict
Is this phishing activity? Yes
The email uses a spoofed sender, URL mismatch, and malware-laced attachment.
High confidence phishing attempt targeting users under the guise of a security update.

## 5. Recommended Actions
Quarantine this and similar emails in the organization

Block sender domain and source IP on perimeter defenses

Notify affected user(s) and initiate phishing awareness

Submit email artifacts to threat intel platform

Consider automated rules for mail gateway to flag similar patterns

## 6. Lessons Learned
Email authentication checks (SPF/DKIM/DMARC) were essential in detection

Domain typos and mismatched URLs are strong red flags

Automated alerting helps rapidly identify and contain threats

Future enhancement: add automatic reputation lookups to enrich detections
