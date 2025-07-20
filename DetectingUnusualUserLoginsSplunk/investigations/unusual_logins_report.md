# SOC Alert Investigation Report – Detecting Unusual User Logins

**Analyst:** Olga Zaytseva  
**Date:** 2025-07-20  
**Platform:** Splunk Enterprise  
**Alert:** Unusual User Login Detection  
**Severity:** Medium  
**Description:** Detects logins from unusual locations or rapid logins from geographically distant places indicating potential account compromise.

---

## 1. Alert Summary

**Triggered At:** 2025-07-20
**Note:** The data analyzed is from 2021 logs, used here as sample/historical data for demonstration.
**Search Logic:**

```spl
index=login_data
| where isnotnull(lat) AND isnotnull(lon)
| geostats latfield=lat longfield=lon count
```
Example alert condition: logins from locations not seen before or impossible travel between login events.

## 2. Log Sample – Suspicious Logins

| User | Login Time          | Location (Lat, Lon) | Country | Notes                        |
| ---- | ------------------- | ------------------- | ------- | ---------------------------- |
| emma | 2025-07-18 03:20:00 | 43.6532, -79.3832   | Canada  | Usual location               |
| emma | 2025-07-18 04:20:00 | 40.7128, -74.0060   | USA     | Rapid login from new country |
| bob  | 2025-07-18 12:30:00 | 31.2304, 121.4737   | China   | Suspicious login location    |

## 3. Triage Analysis

| Indicator         | Value                           | Notes                               |
| ----------------- | ------------------------------- | ----------------------------------- |
| User              | emma                            | Multiple logins from Canada and USA |
| Time Difference   | 1 hour                          | Impossibly short travel time        |
| Locations         | Toronto, Canada & New York, USA | Geographically distant              |
| Login Count Spike | emma: +5 logins in 1 day        | Could indicate account compromise   |

**Visual Dashboard – Login Activity Overview**

The dashboard below was used to analyze login patterns across users, time, and geography. It includes:

- Login Locations Map  
- Top Login Locations Bar Chart  
- Daily Login Count by User Line Chart  
- Impossible Travel Logins Table  

![Login Dashboard Panels](./07_full_dashboard.png)
*Splunk dashboard showing unusual login activity*

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
