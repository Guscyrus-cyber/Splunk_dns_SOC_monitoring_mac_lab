**Splunk DNS SOC Monitoring Lab**

**Lab Goal**

The goal of this lab is to ingest DNS activity into Splunk Enterprise and analyze domain name resolution activity to identify legitimate services, external communications, suspicious domains, and potential indicators of compromise through dashboards, reports, alerts, detections, and threat-hunting queries.

**Dataset:** dns_traffic.log
Index: dns

**Create DNS Index**

Settings → Indexes → New Index

Note: dns has already indexed in previous lab in splunk enterprise.

**Upload DNS Dataset**

Upload: dns_traffic.log

Index: dns\
Host: MacBookPro\
Sourcetype: dns_traffic

Description:DNS traffic data was ingested into Splunk Enterprise to support domain resolution monitoring and DNS-based threat detection.\

Please refer to images # 1 and 2 in the repository.

**Verify Ingestion and Activity**

Search: index=dns\
Purpose: Verify successful DNS data ingestion and Review DNS requests and responses contained within the dataset.\

Please refer to images # 3 and 4 in the repository.


**Verify Event Count**

Search:\
index=dns\
\| stats count\
\
Purpose: Confirm DNS dataset availability.

Please refer to images # 5 in the repository.

**Identify Queried Domains**

Search:\
\
index=dns\
\| table \_raw

Purpose: Identify domains requested by the endpoint.

Please refer to images # 6 in the repository.

**Search for Google DNS Activity**

Search: index=dns google

Purpose: Identify communications involving Google-related domains.

Please refer to images # 7 in the repository.

**Search for Microsoft DNS Activity**

Search: index=dns microsoft

Purpose: Identify communications involving Microsoft-related domains.

Please refer to images # 8 in the repository.

**Search for Github DNS Activity**

Search: index=dns github

Purpose: Identify communications involving Github-related domains.\

Please refer to images # 9 in the repository.

**Visualization**

Query:\
index=dns\
\| stats count

Visualization: Single Value\
Panel Title: DNS Event Count\
Description: Displays the total number of DNS events ingested into the DNS index.

Please refer to images # 10 in the repository.

**Report**

Report Name: DNS Activity Report

Description: Summarizes DNS activity identified within the dataset.

Please refer to images # 11 in the repository.

 **Dashboard**

Dashboard Name: DNS SOC Monitoring Dashboard

Panel: DNS Activity Overview\
\
Description: Provides visibility into DNS resolution activity and queried domains.

Please refer to images # 12 in the repository.

**Step 13 — Alert**

Search: index=dns\
\
Alert Name: DNS Activity Detection

Description: Generates notifications when DNS activity is observed.

Please refer to images # 13 in the repository.

**Threat Hunting**

Search:\
index=dns\
\| table \_time host sourcetype \_raw

Purpose: Review DNS requests and domain resolution activity for suspicious or unusual domains.

\
Investigation of maliciousdomain.com identified 7 DNS queries originating from multiple source IP addresses. The domain was queried by 10.0.0.10, 10.0.0.15, 10.0.0.20, 192.168.1.50, and 88.88.88.88, indicating that the activity was not isolated to a single host. Repeated DNS requests to the same suspicious domain across multiple systems may indicate phishing activity, malware communication, user access to a malicious website, or testing activity within the environment. Because DNS requests often precede network connections, the domain should be investigated further using threat intelligence sources, reputation services, firewall logs, proxy logs, and endpoint telemetry to determine whether successful communication occurred after the DNS resolution.

### 

| **Finding**       | **Result**          |
|-------------------|---------------------|
| Suspicious Domain | maliciousdomain.com |
| Total Queries     | 7                   |
| Unique Source IPs | 5                   |
| First Seen        | 2026-06-03 08:05:00 |
| Last Seen         | 2026-06-03 08:49:00 |
| Affected Systems  | Multiple source IPs |

### 
Assessment

The most notable characteristic is: Multiple source IPs queried the same suspicious domain.

This is generally more concerning than:

One host queried one suspicious domain one time.

because repeated activity across several systems may suggest broader exposure or coordinated activity.

So In this lab, Threat-hunting analysis identified 7 DNS requests for maliciousdomain.com originating from 5 unique source IP addresses. The repeated queries across multiple systems made the domain a high-priority investigation candidate and demonstrated how DNS monitoring can be used to identify potentially malicious communications within a SOC environment.

Please refer to images # 14 in the repository.
