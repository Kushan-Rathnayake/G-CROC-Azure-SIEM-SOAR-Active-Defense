# 📡 Telemetry Collection & OS Hardening (Sysmon)

### Overview
[cite_start]To overcome the limitations of standard Windows Event Logging, this project utilized **Microsoft Sysinternals Sysmon** to provide deep visibility into host-level activities. [cite_start]This telemetry was essential for detecting the malicious process creation events (Event ID 1) that triggered the automated SOAR response[cite: 2, 3].

### Step 1: Configuration & Binaries
[cite_start]Instead of using a default configuration, I implemented the industry-standard **SwiftOnSecurity** Sysmon configuration[cite: 5]. [cite_start]This template is specifically designed to filter out benign system noise and highlight actual adversary behavior[cite: 6].

* [cite_start]**Sysmon Binary:** [Official Microsoft Download](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon) [cite: 4]
* [cite_start]**Config Template:** [SwiftOnSecurity GitHub](https://github.com/SwiftOnSecurity/sysmon-config) [cite: 7]

### Step 2: Installation
[cite_start]The installation was performed via an elevated command prompt, applying the custom XML configuration to the 64-bit Sysmon service[cite: 8, 9]:

```powershell
# Navigate to the directory
cd C:\Sysmon

# Install Sysmon with the SwiftOnSecurity configuration
sysmon64.exe -accepteula -i sysmonconfig-export.xml
