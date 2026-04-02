# Kusto Query Language (KQL) Analytics Rules

### Overview
This directory contains the custom detection engineering logic utilized in the Microsoft Sentinel environment for the G-CROC project. 

Standard out-of-the-box SIEM rules often generate high volumes of false positives or miss advanced, in-memory threats. The KQL rules in this repository were engineered to focus on **Behavioral Correlation** and **Deep OS Telemetry Parsing**, specifically mapped to the MITRE ATT&CK framework.

### The Detections
The JSON ARM templates in this folder contain the full deployment logic, severity thresholds, and raw KQL queries for the following detections:

* **Suspicious Command Line Execution (LotL) [T1059.001]:** Utilizes Regular Expressions (`extract()`) to slice raw XML Sysmon Event ID 1 data, hunting for Living-off-the-Land Binaries (e.g., PowerShell, Certutil) executing download commands like `-urlcache` or `Invoke-WebRequest`.
* **Successful Breach after Recon/Brute Force [T1110.001, T1021.001]:** An advanced behavioral rule that uses an `inner join` to correlate inbound Azure network probes (`NTANetAnalytics`) with successful Windows Security logons (`Event ID 4624`). It detects when a threat actor transitions from the reconnaissance phase to a successful initial access breach.
* **Persistent Network Reconnaissance [T1595.001]:** Tracks aggressive multi-port scanning (e.g., Ports 80, 445, 1433, 3306) from single external IP addresses to map adversary infrastructure targeting.
* **Anti-Forensics - Event Logs Cleared [T1070.001]:** Monitors for defense evasion tactics by correlating the clearing of the Windows Security Log (Event 1102) and System Log (Event 104), triggering an immediate high-severity incident.

### Deployment
These `.json` files are exported Azure Resource Manager (ARM) templates. They can be imported directly into any Microsoft Sentinel workspace to immediately deploy the analytic rules, incident configuration, and dynamic severity logic.
