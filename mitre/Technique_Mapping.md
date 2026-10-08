> Part of [APT29 / SolarWinds: Threat Intelligence & MITRE ATT&CK Analysis](../README.md)

# 4. MITRE ATT&CK Technique Mapping

The following techniques were selected because they are specifically documented by MITRE for the SolarWinds Compromise rather than simply being techniques that APT29 has used in unrelated operations.

| Tactic | Technique ID | Technique Name | How APT29 Used It |
|---|---|---|---|
| Initial Access | **T1195.002** | Supply Chain Compromise: Compromise Software Supply Chain | APT29 gained access to victims through a trojanized SolarWinds Orion software update. SUNSPOT was used to insert SUNBURST into the Orion build process. |
| Defense Evasion | **T1553.002** | Subvert Trust Controls: Code Signing | SUNBURST was incorporated into the Orion software lifecycle and signed using legitimate SolarWinds code-signing certificates. |
| Execution | **T1059.001** | Command and Scripting Interpreter: PowerShell | APT29 used PowerShell to execute commands, create remote tasks, identify configurations, and conduct other post-compromise activities. |
| Discovery | **T1087.002** | Account Discovery: Domain Account | APT29 used `Get-ADUser` and `Get-ADGroupMember` to identify domain accounts. |
| Discovery | **T1482** | Domain Trust Discovery | The group used `Get-AcceptedDomain` and AdFind to identify domains and trust relationships. |
| Credential Access | **T1555.003** | Credentials from Web Browsers: Chrome | APT29 stole saved Chrome passwords from targeted users. |
| Persistence | **T1098.001** | Account Manipulation: Additional Cloud Credentials | The attackers added credentials to OAuth applications and service principals. |
| Persistence / Privilege Escalation | **T1098.003** | Account Manipulation: Additional Cloud Roles | APT29 granted administrator privileges to a newly created service principal. |
| Defense Evasion / Privilege Escalation | **T1484.002** | Domain or Tenant Policy Modification: Trust Modification | APT29 modified federation trust settings so the victim environment would accept tokens signed with the attackers' SAML certificate. |
| Credential Access | **T1606.002** | Forge Web Credentials: SAML Tokens | The attackers created forged SAML tokens using compromised SAML signing certificates to impersonate legitimate users. |
| Defense Evasion / Lateral Movement | **T1550.001** | Use Alternate Authentication Material: Application Access Token | APT29 used compromised service principals and associated authentication material to access and modify Office 365 environments. |
| Lateral Movement | **T1021.001** | Remote Services: RDP | APT29 used RDP sessions from public-facing systems to internal servers. |
| Lateral Movement | **T1021.002** | Remote Services: SMB/Windows Admin Shares | The attackers used administrative accounts to connect to targeted users through SMB. |
| Lateral Movement / Execution | **T1021.006** | Remote Services: Windows Remote Management | APT29 used WinRM through PowerShell to execute commands and payloads on remote hosts. |
| Persistence / Execution | **T1053.005** | Scheduled Task/Job: Scheduled Task | APT29 created or modified scheduled tasks to execute tools remotely and maintain persistence. |
| Collection | **T1114.002** | Email Collection: Remote Email Collection | The group used Exchange mailbox commands to collect emails from selected executives and IT personnel. |
| Collection | **T1213.003** | Data from Information Repositories: Code Repositories | APT29 downloaded source code from victim code repositories. |
| Collection | **T1074.002** | Data Staged: Remote Data Staging | Collected files and data were staged in password-protected archives on an OWA server. |
| Collection | **T1560.001** | Archive Collected Data: Archive via Utility | APT29 used 7-Zip to compress stolen email and other information into password-protected archives. |
| Command and Control | **T1071.001** | Application Layer Protocol: Web Protocols | HTTP was used for command and control and data exfiltration. |
| Command and Control | **T1568** | Dynamic Resolution | APT29 used dynamic DNS resolution to construct and resolve randomly generated subdomains for C2. |
| Exfiltration | **T1048.002** | Exfiltration Over Alternative Protocol: Asymmetric Encrypted Non-C2 Protocol | Collected information was exfiltrated using HTTPS requests to password-protected archives staged on OWA servers. |
| Defense Evasion | **T1685.001** | Disable or Modify Windows Event Log | APT29 used `AUDITPOL` to prevent the collection of audit logs. |
| Defense Evasion | **T1686** | Disable or Modify System Firewall | The attackers used `netsh` to modify firewall rules and restrict certain outbound traffic. |

**Note:** More than 15 techniques are provided intentionally so that the report demonstrates the breadth of the campaign. The techniques are not intended to imply that every stage occurred identically in every victim environment.
