# 📡 Telemetry Collection & OS Hardening (Sysmon)

### Overview
To overcome the limitations of standard Windows Event Logging, this project utilized **Microsoft Sysinternals Sysmon** to provide deep visibility into host-level activities. This telemetry was essential for detecting the malicious process creation events (Event ID 1) that triggered the automated SOAR response.

### Step 1: Configuration & Binaries
Instead of using a default configuration, I implemented the industry-standard **SwiftOnSecurity** Sysmon configuration. This template is specifically designed to filter out benign system noise and highlight actual adversary behavior.

* **Sysmon Binary:** [Official Microsoft Download](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon)
* **Config Template:** [SwiftOnSecurity GitHub](https://github.com/SwiftOnSecurity/sysmon-config)

### Step 2: Installation
The installation was performed via an elevated command prompt, applying the custom XML configuration to the 64-bit Sysmon service:

```powershell
# Navigate to the directory
cd C:\Sysmon

# Install Sysmon with the SwiftOnSecurity configuration
sysmon64.exe -accepteula -i sysmonconfig-export.xml
