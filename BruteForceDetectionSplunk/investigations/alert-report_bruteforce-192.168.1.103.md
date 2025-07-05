# SOC Alert Investigation Report – Brute Force Login Detection

**Analyst:** Olga Zaytseva  
**Date:** 07.01.2025  
**Platform:** Splunk Enterprise  
**Alert:** Brute Force Detection Alert  
**Severity:** Medium  
**Description:** Detects 5+ failed login attempts from the same IP address within a 10-minute window.

---

## 1. Alert Summary

**Triggered At:** 07.01.2025 09:00 AM  
**Search Logic:**

index=bruteforce sourcetype=custom_windows_auth
| where like(_raw, "%FAILURE%")
| rex field=_raw "(?<timestamp>\d{4}-\d{2}-\d{2} \d{2}:\d{2}:\d{2}),(?<host>[^,]+),(?<user>[^,]+),(?<status>[^,]+),(?<src_ip>\d+\.\d+\.\d+\.\d+)"
| bin _time span=10m
| stats count by _time, src_ip
| where count >= 5

## Condition Met:

5 failed login attempts from IP 192.168.1.103 within 10 minutes.
![Triggered alert showing source IP and event count.](https://github.com/LogLogic/SIEMDashboardsDetectionEngineering/blob/main/BruteForceDetectionSplunk/screenshots/alert_triggered.png)

## 2. Log Sample – Failed Logins

2025-07-01 09:00:12,HOST2,admin,FAILURE,192.168.1.103
2025-07-01 09:00:15,HOST2,admin,FAILURE,192.168.1.103
2025-07-01 09:00:20,HOST2,admin,FAILURE,192.168.1.103
2025-07-01 09:00:25,HOST2,admin,FAILURE,192.168.1.103
2025-07-01 09:00:30,HOST2,admin,FAILURE,192.168.1.103

Username Targeted: admin
Host: HOST2
Status: All login attempts failed
Outcome: No successful login from this IP
![Log sample from Search view.](https://github.com/LogLogic/SIEMDashboardsDetectionEngineering/blob/main/BruteForceDetectionSplunk/screenshots/failed_logins_raw.png)

## 3. 🔎 Triage Analysis

| Indicator        | Value           | Notes                          |
|------------------|------------------|--------------------------------|
| Source IP        | 192.168.1.103    | Internal IP                    |
| Target Username  | admin            | High-value target              |
| Host             | HOST2            | Windows system                 |
| Repetition       | 5 failures in 18s | High-speed brute force attempt |
| Location         | Internal range    | No geolocation available       |


## 4. Verdict

### Is this brute force activity? Yes
Pattern matches automated brute force: rapid failures, repeated user, high-value account.

## 5. Recommended Actions
Block or monitor IP 192.168.1.103

Check for corresponding successful login

Review logs for further attempts from similar IPs

Notify system owner of HOST2

Consider adding MFA to admin accounts

## 6. Lessons Learned

The alert effectively detected a suspicious burst of failed logins

Time-based threshold worked well

Next iteration: enrich alerts with username context (admin/root)
