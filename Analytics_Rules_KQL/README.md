# 🧠 Kusto Query Language (KQL) Analytics Rules

### Overview
This directory contains the custom detection engineering logic utilized in the Microsoft Sentinel environment for the G-CROC project. These rules are the "brain" of the SIEM, transforming millions of raw logs into high-fidelity, actionable security incidents.

Standard out-of-the-box SIEM rules often generate high volumes of false positives or miss advanced, in-memory threats. The KQL rules in this repository focus on **Behavioral Correlation** and **Deep OS Telemetry Parsing**, specifically mapped to the MITRE ATT&CK framework to ensure comprehensive coverage across the kill chain.

---

### 🛡️ Detailed Detection Inventory

| Rule Name | MITRE Tactics | Primary Technique (ID) | Logic Summary |
| :--- | :--- | :--- | :--- |
| **Suspicious Command Line Execution (LotL)** | Execution, C2 | T1059.001 (PowerShell) | Uses Regex to parse Sysmon Event ID 1 XML data for malicious `Invoke-WebRequest` or `certutil` downloads. |
| **Successful Breach after Recon/Brute Force** | Recon, Initial Access | T1078.003 (Local Accounts) | Behavioral correlation (inner join) linking external port probing with successful Event ID 4624 logons. |
| **Brute Force / Failed RDP Logins** | Initial Access, Credential Access | T1110 (Brute Force) | Detects aggressive Event ID 4625 failures with **Dynamic Severity** scaling for high-value accounts. |
| **Persistent Network Reconnaissance** | Reconnaissance, Discovery | T1595.001 (Scanning IP Blocks) | Monitors `NTANetAnalytics` for multi-port probing (80, 445, 1433, 3306) exceeding established thresholds. |
| **Anti-Forensics - Event Logs Cleared** | Defense Evasion | T1070.001 (Clear Windows Event Logs) | Immediate high-severity trigger for Security (1102) or System (104) log clearing to prevent forensic erasure. |

---

### 🔍 Technical Deep Dive

#### 1. Suspicious Command Line Execution (LotL)
* **Technique:** T1059.001, T1105
* **Logic:** Targets "Living off the Land" binaries. The query uses `extract()` to pull the `CommandLine` from Sysmon logs and filters for download flags like `-urlcache` or `Invoke-WebRequest`. This is the primary trigger for the 41-second automated kill switch.

#### 2. Successful Breach after Recon/Brute Force
* **Technique:** T1595.001, T1110.001, T1078.003, T1021.001, T1021.002
* **Logic:** A complex correlation rule. It first identifies "Suspicious IPs" that have either failed 5+ logins or probed honeypot ports, then joins that list against successful logins from those same IPs within a specified window.

#### 3. Brute Force / Failed RDP Logins
* **Technique:** T1133, T1110, T1021.001
* **Logic:** Beyond simple counting, this rule uses an `iff` statement to dynamically assign severity. If an attacker targets "Administrator" or "root," the incident is automatically escalated to **High**, ensuring the SOC prioritizes critical account threats.

#### 4. Persistent Network Reconnaissance Detected
* **Technique:** T1595.001, T1046
* **Logic:** Analyzes `NTANetAnalytics` to identify external actors performing high-frequency scanning. It uses `dcount(DestPort)` to identify multi-vector probes, distinguishing between a simple misconfiguration and a deliberate environment scan.

#### 5. Anti-Forensics - Event Logs Cleared
* **Technique:** T1070.001
* **Logic:** A "True Positive" indicator of compromise. It monitors the `SecurityEvent` and `Event` tables simultaneously. It also auto-generates a direct **Host Investigation URL** in the custom details to allow an analyst to jump straight to the compromised entity.

---

### 🚀 Deployment
These files are exported as **Azure Resource Manager (ARM) templates**. To deploy:
1. Navigate to **Microsoft Sentinel > Analytics**.
2. Click **Import** and select the desired `.json` file.
3. Review the logic and click **Save**.
