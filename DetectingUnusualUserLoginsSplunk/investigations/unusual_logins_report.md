# SOC Investigation Report – Manual Review of Unusual User Logins

**Analyst:** Olga Zaytseva  
**Date:** 2025-07-20  
**Platform:** Splunk Enterprise  
**Alert:** No alert was configured  
**Severity:** Medium (based on observed indicators)  
**Purpose:** Manual log review to detect potentially suspicious login activity using historical data and geolocation logic.

---

## 1. Summary

This investigation analyzes user login activity to detect unusual or suspicious behavior that may indicate credential compromise or unauthorized access. Using Splunk and custom SPL queries, multiple indicators were examined, including logins from multiple countries, logins during unusual hours, new IP addresses, and impossible travel scenarios.

Several user accounts, including `emma`, `bob`, `charlie`, `grace`, `harry`, and `irene`, demonstrated potentially risky behaviors such as rapid logins from geographically distant locations within short timeframes and logins from unusual countries or at unusual hours. These findings support the suspicion of possible account compromise or unauthorized access.

---

## 2. Triage Walkthrough

### Step 1: Data Ingestion and Field Validation  
- Sample login data ingested into Splunk (`login_data` index).  
- Confirmed fields such as `_time`, `user`, `src_ip`, `city`, `country`, `lat`, and `lon` were correctly extracted.

### Step 2: Initial Search for Multi-Country Logins  
- Ran SPL query to identify users with logins from multiple countries, indicating potential anomalies.

### Step 3: Detection of Logins Outside Normal Hours  
- Applied filter for logins occurring before 6 AM or after 10 PM UTC to flag unusual timing.

### Step 4: Identification of New IP Addresses  
- Queried for first-seen IP addresses per user within the last 24 hours to detect unfamiliar access points.

### Step 5: Impossible Travel Detection  
- Executed streamstats-based query to identify rapid logins from geographically distant countries occurring within 12 hours, an unlikely physical travel scenario.

### Step 6: Dashboard Analysis and Correlation  
- Reviewed visual dashboards (login maps, top locations, timecharts) for patterns and spikes supporting suspicious activity.

### Step 7: Risk Prioritization  
- Prioritized investigation on users exhibiting multiple suspicious indicators, focusing on *emma* and *bob* accounts.

---

## 3. Data Overview

- **Data Source:** Sample login data ingested into a custom Splunk index (`login_data`) from `sample_login_data.csv`.  
- **Fields:** Timestamp (`_time`), user, source IP, city, country, latitude, and longitude.  
- **Tooling:** SPL queries leveraging stats, eval, streamstats, and geolocation commands were used to detect anomalies.  

---

## 4. Detection Methodology

The following detection criteria were applied:

- **Multi-country Logins:** Identified users logging in from more than one country.  
- **Unusual Hours:** Flagged logins occurring outside typical business hours (before 6 AM or after 10 PM UTC).  
- **New IP Addresses:** Detected logins from IP addresses not seen in the prior 24 hours for a user.  
- **Impossible Travel:** Detected sequential logins from different countries occurring within 12 hours, which is implausible given geographic distances.

These criteria were operationalized using SPL queries and visualized on dashboards featuring login maps, top locations, timecharts, and impossible travel tables.

---

## 5. Key Findings

| User    | Event Timestamp (UTC) | City          | Country   | Notes                           |
| ------- | --------------------- | ------------- | --------- | ------------------------------- |
| emma    | 2021-07-16 03:20:00   | Toronto       | Canada    | Unusual login outside baseline  |
| emma    | 2021-07-16 04:00:00   | Seattle       | USA       | Rapid return login              |
| bob     | 2021-07-16 10:40:00   | Beijing       | China     | New country login               |
| bob     | 2021-07-16 12:00:00   | Chicago       | USA       | Impossibly rapid return         |
| charlie | 2021-07-17 10:00:00\* | Moscow        | Russia    | Unusual foreign login           |
| charlie | 2021-07-16 08:00:00   | Los Angeles   | USA       | Normal login location           |
| grace   | 2021-07-16 22:00:00\* | London        | UK        | New country login late at night |
| grace   | 2021-07-16 07:00:00   | Denver        | USA       | Normal login location           |
| harry   | 2021-07-16 18:00:00\* | Sydney        | Australia | New foreign login               |
| harry   | 2021-07-16 06:00:00   | San Francisco | USA       | Normal login location           |
| irene   | 2021-07-17 02:00:00\* | New Delhi     | India     | Foreign login late at night     |
| irene   | 2021-07-16 07:00:00   | Dallas        | USA       | Normal login location           |

`*`  *Timestamp approximated based on epoch times in dataset for clarity*

![06_impossible_travel](https://github.com/LogLogic/SIEMDashboardsDetectionEngineering/blob/main/DetectingUnusualUserLoginsSplunk/screenshots/06_impossible_travel.png)

### Observations

- Several users (`charlie`, `grace`, `harry`, `irene`) exhibited logins from foreign countries (Russia, UK, Australia, India) interspersed with normal US logins. These events represent possible impossible travel scenarios or use of VPN/proxy IPs.
- Some foreign logins occurred outside typical business hours, increasing suspicion.
- Geographic distances between consecutive logins within a few hours for these users suggest account compromise or unauthorized access.
- These anomalies align with detection criteria from the SPL queries and reinforce the need to investigate a wider set of accounts.

---

## 6. Verdict
**Is this suspicious login activity? Yes**

The combination of unusual geolocations, impossible travel patterns, and login spikes constitutes strong evidence of suspicious activity consistent with potential account compromise. Immediate follow-up is warranted.

---

## 7. Recommendations

- **User Investigation:** Conduct in-depth review of user *emma* and *bob* accounts for signs of compromise.  
- **Access Controls:** Enforce or verify multi-factor authentication (MFA) for all users.  
- **Monitoring:** Set up automated alerts for impossible travel and multi-country login patterns.  
- **User Notification:** Inform affected users of suspicious activity and recommend password resets.  
- **Incident Response:** Prepare for possible escalation if further suspicious activity is detected.

---

## 8. Lessons Learned
- Geolocation analysis is essential for login anomaly detection
- Impossible travel logic is effective for early compromise alerts
- Visual dashboards accelerate triage and investigations
