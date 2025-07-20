# SOC Alert Investigation Report – Detecting Unusual User Logins

**Analyst:** Olga Zaytseva  
**Date:** 2025-07-20  
**Platform:** Splunk Enterprise  
**Alert:** No alert was configured  
**Severity:** Medium  (based on observed indicators) 
**Purpose:** Manual log review to detect potentially suspicious login activity using historical data and geolocation logic.
---

## 1. Summary

This report presents the results of a manual investigation into user login patterns using historical log data from 2021. Although no automated alert was configured, the review simulated "impossible travel" detection and anomalous geolocation patterns to uncover signs of potential account compromise.

**Sample SPL Logic**

```spl
index=login_data
| where isnotnull(lat) AND isnotnull(lon)
| geostats latfield=lat longfield=lon count
```
Example alert condition: logins from locations not seen before or impossible travel between login events.

## 2. Log Sample – Suspicious Logins

| User | Login Time (UTC)     | City     | Lat, Lon               | Country | Notes                            |
|------|----------------------|----------|------------------------|---------|----------------------------------|
| emma | 2021-07-16 03:00:00  | Toronto  | 43.65107, -79.347015   | Canada  | Unusual login (outside baseline) |
| emma | 2021-07-16 04:00:00  | Seattle  | 47.6062, -122.3321     | USA     | Rapid return to usual location   |
| bob  | 2021-07-16 11:00:00  | Beijing  | 39.9042, 116.4074      | China   | Sudden login from new country    |
| bob  | 2021-07-16 12:00:00  | Chicago  | 41.8781, -87.6298      | USA     | Impossible return time           |

![06_impossible_travel](https://github.com/LogLogic/SIEMDashboardsDetectionEngineering/blob/main/DetectingUnusualUserLoginsSplunk/screenshots/06_impossible_travel.png)

  
## 3. Triage Analysis

Upon reviewing the alert, two user accounts—**emma** and **bob**—show clear signs of suspicious behavior.

- **emma** logged in from **Toronto, Canada**, then an hour later from **Seattle, USA**. The geographic distance (~3,300 km) makes this travel physically impossible in that timeframe. This pattern triggered an "impossible travel" detection rule.  
- **bob** logged in from **Beijing, China**, followed by a return login from **Chicago, USA** within an hour. No previous activity was logged from China, marking it as a **new, unusual location**.

The following table summarizes the key triage indicators:

| Indicator          | Value                     | Notes                                            |
| ------------------ | ------------------------- | ------------------------------------------------ |
| User               | emma, bob                 | Both accounts triggered location-based anomalies |
| Time Difference    | 1 hour                    | Impossibly fast travel between logins            |
| Locations          | Canada → USA, China → USA | High geographic distance in short time window    |
| Baseline Deviation | Yes                       | Login cities not seen before for either user     |
| Login Spike        | emma: 5 logins/day        | Increased activity suggests possible compromise  |

Initial triage confirms both accounts exhibited high-risk behavior patterns: login anomalies across continents within an hour, unrecognized locations, and activity spikes. These match indicators commonly seen in credential theft or VPN/geolocation spoofing attacks.

## 4. Verdict
**Is this suspicious login activity? Yes**

Multiple rapid logins from geographically distant locations strongly indicate possible credential compromise.

## 5. Recommended Actions
Investigate flagged user accounts for compromise

Enforce multi-factor authentication for affected users

Monitor for additional suspicious login patterns

Notify security operations and affected users

## 6. Lessons Learned
Geolocation analysis is essential for login anomaly detection

Impossible travel logic is effective for early compromise alerts

Visual dashboards accelerate triage and investigations
