# ⚡ Infrastructure & Automated Response (SOAR)

### Overview
This directory contains the automation logic used to achieve the verified total **42-second Mean Time to Respond (MTTR)** for the G-CROC project. This was accomplished by transitioning from passive alerting to active cloud-native containment.

### The Playbook: `G-CROC-Critical-Email-Alert`
The Logic App contained in this folder follows a high-fidelity response chain:
1. **Trigger:** Fires immediately upon the creation of a Critical Sentinel Incident.
2. **Triage:** Utilizing the Outlook.com API, the playbook formats a dynamic HTML alert containing the attacker's IP, targeted account, and a direct link to the Sentinel investigation.
3. **Authorization:** Rather than using vulnerable API keys, the playbook utilizes a **System-Assigned Managed Identity** with the **Virtual Machine Contributor** RBAC role.
4. **Action:** Issues an authenticated API call to the Azure Resource Manager (ARM) to physically **deallocate** the compromised virtual machine, instantly severing the attacker's connection.
