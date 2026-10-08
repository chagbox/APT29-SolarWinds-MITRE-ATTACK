# APT Profiling with MITRE ATT&CK: APT29 and the SolarWinds Compromise

## 1. Introduction

Advanced Persistent Threat (APT) groups conduct long-term, targeted cyber operations designed to obtain intelligence, maintain access to valuable networks, and avoid detection. One of the most significant examples of this type of activity is the SolarWinds Compromise, a large-scale supply-chain operation attributed to APT29, a threat group associated with Russia's Foreign Intelligence Service (SVR).

This report profiles APT29 using the MITRE ATT&CK framework and examines the SolarWinds Compromise as a representative campaign. The analysis focuses on how the attackers gained access, established persistence, performed discovery, abused identities and authentication mechanisms, moved through victim environments, collected information, and exfiltrated data.

The campaign demonstrates why modern defenders must look beyond individual malware detections. The attackers combined a compromised software supply chain with legitimate administrative tools, stolen credentials, identity infrastructure abuse, and carefully controlled post-compromise activity. MITRE identifies the SolarWinds Compromise as campaign C0024 and associates it with APT29 (G0016). The campaign was first observed in August 2019 and was last observed in January 2021.

---

# 2. APT29 Group Identification

### Group

**APT29**

### MITRE ATT&CK Group ID

**G0016**

### Aliases

APT29 has been associated with several names, including **Cozy Bear, The Dukes, CozyDuke, NOBELIUM, UNC2452, SolarStorm, Dark Halo, and Midnight Blizzard**. Different security vendors and government organizations have used different names for activity associated with the group.

### Attribution

MITRE ATT&CK attributes APT29 to Russia's **Foreign Intelligence Service (SVR)**. In April 2021, the United States and United Kingdom publicly attributed the SolarWinds Compromise to the SVR and referenced APT29, Cozy Bear, and The Dukes in their public statements.

### Active Period

APT29 has operated since at least **2008**. MITRE describes the group as having a long history of targeting government networks, European and NATO-related organizations, research institutes, and think tanks.

### Motivation

APT29 is primarily associated with **cyber espionage and intelligence collection**. Its operations are generally designed to obtain sensitive information rather than cause immediate destructive impact.

### Targets

APT29 has historically targeted government organizations, diplomatic entities, research institutions, think tanks, technology organizations, and other strategically valuable organizations. During the SolarWinds campaign, victims included government, consulting, technology, telecommunications, and other organizations across North America, Europe, Asia, and the Middle East.

---

# 3. Campaign Narrative: SolarWinds Compromise

The SolarWinds Compromise, identified by MITRE as **C0024**, was a sophisticated supply-chain operation in which APT29 compromised the software development and build process associated with SolarWinds Orion. Rather than attempting to compromise every intended victim individually, the attackers compromised a trusted software supplier and used its legitimate update mechanism to reach organizations downstream. MITRE records the campaign as beginning in August 2019 and ending in January 2021.

The attack began with access to the SolarWinds environment and manipulation of the Orion software build process. APT29 used **SUNSPOT**, a piece of malware designed to monitor and manipulate the Orion build process so that the malicious **SUNBURST** backdoor could be inserted into legitimate software builds. This distinction is important: SUNSPOT functioned as part of the build-stage compromise, while SUNBURST was the backdoor that was subsequently delivered to victims.

The resulting Orion software update appeared legitimate to customers. Because the malicious code was incorporated into the software lifecycle, the attackers were able to take advantage of the trust organizations placed in SolarWinds software and its update mechanism. MITRE classifies this behavior as **T1195.002 — Compromise Software Supply Chain**.

The malicious update was distributed to a large number of organizations. However, the operation did not mean that APT29 actively pursued every organization that installed the compromised software. MITRE notes that approximately 18,000 SolarWinds customers were affected by the compromised Orion updates, while a much smaller number experienced follow-on activity by APT29. This selective approach allowed the attackers to concentrate on valuable targets while maintaining a relatively small operational footprint.

Once the compromised update was installed, SUNBURST provided the attackers with an initial foothold and a means of communicating with victim environments. APT29 then conducted additional reconnaissance and attempted to understand the victim's users, domains, systems, applications, and identity infrastructure.

For example, APT29 used PowerShell commands such as `Get-ADUser` and `Get-ADGroupMember` to discover domain accounts. The group also used tools such as AdFind to discover domains, domain groups, trust relationships, and remote systems. These activities allowed the attackers to map the environment before deciding how to proceed.

The attackers subsequently abused legitimate accounts and authentication mechanisms. One particularly significant part of the campaign involved compromising identity infrastructure and federation trust. APT29 modified federation trust settings and configured the environment to accept SAML authentication tokens signed with a certificate controlled by the attackers. This enabled the attackers to forge SAML tokens and impersonate legitimate users, including bypassing certain authentication controls.

APT29 also added credentials to OAuth applications and service principals, granted administrative privileges to service principals, registered devices, and manipulated mailbox permissions. These actions provided additional ways to maintain access and interact with cloud environments.

After establishing access, the attackers moved laterally using legitimate administrative mechanisms. MITRE documents the use of RDP, SMB/Windows administrative shares, and Windows Remote Management. Scheduled tasks were also used to execute tools on remote hosts and support persistence.

The attackers then pursued information of intelligence value. They collected email from selected individuals, including executives and IT personnel, and accessed internal knowledge repositories and code repositories. They also extracted files from compromised systems and attempted to obtain credentials stored in password stores and web browsers.

Before exfiltration, APT29 staged information in password-protected archives on victim OWA servers and used 7-Zip to compress stolen email and other information. The collected information was then exfiltrated through HTTPS and other mechanisms.

The operation was significant because it demonstrated how compromising a trusted software provider can provide an attacker with access to numerous downstream organizations. It also demonstrated the increasing importance of identity security. Even after gaining an initial foothold through software, APT29 relied heavily on legitimate credentials, cloud identities, remote administration tools, and authentication mechanisms. CISA described the activity as involving lateral movement into multiple systems, including Microsoft cloud environments, and difficult-to-detect persistence mechanisms.

The SolarWinds campaign therefore illustrates a complete espionage operation: compromise the trusted supplier, distribute malicious code through legitimate software, identify valuable victims, establish persistent access, abuse identity infrastructure, move laterally, collect sensitive information, and exfiltrate it while attempting to remain undetected.

---

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

---

# 5. Kill Chain Analysis

The SolarWinds Compromise can be viewed through a traditional cyber kill-chain structure while using MITRE ATT&CK to identify the techniques associated with each stage.

| Kill Chain Stage | SolarWinds Activity | ATT&CK Mapping |
|---|---|---|
| Reconnaissance | APT29 identified valuable organizations and gathered information that could support later access. | Victim identity and credential information |
| Weaponization | APT29 developed/used SUNSPOT and SUNBURST and prepared malicious infrastructure. | T1587.001; T1195.002 |
| Delivery | A malicious Orion update was distributed through SolarWinds' legitimate update mechanism. | T1195.002 |
| Exploitation / Initial Access | Victims installed the trojanized Orion update, providing the attackers with an initial foothold. | T1195.002 |
| Installation / Persistence | APT29 established additional persistence through scheduled tasks, account manipulation, service principals, and identity mechanisms. | T1053.005; T1098.001; T1098.003 |
| Command and Control | SUNBURST and subsequent tooling communicated with attacker infrastructure using web protocols and dynamic resolution. | T1071.001; T1568 |
| Discovery | Attackers identified accounts, domains, trusts, remote systems, processes, and Exchange configuration. | T1087.002; T1482; T1018; T1057 |
| Credential / Identity Abuse | Attackers stole credentials, obtained browser credentials, modified federation trust, and forged SAML tokens. | T1555.003; T1484.002; T1606.002 |
| Lateral Movement | APT29 moved through networks using RDP, SMB, WinRM, and other remote access mechanisms. | T1021.001; T1021.002; T1021.006 |
| Collection | Email, source code, files, and internal business information were collected. | T1114.002; T1213.003; T1005 |
| Staging | Information was placed into password-protected archives on remote infrastructure. | T1074.002 |
| Exfiltration | Staged information was transferred out using web-based communication and HTTPS. | T1048.002; T1071.001 |
| Cleanup / Defense Evasion | Tools and indicators were removed and security logging or monitoring could be impaired. | T1685.001; T1686; T1070.004 |

## Best Point for Detection and Disruption

The **best overall detection and disruption point is the transition immediately after the compromised Orion software update is installed and begins executing on an endpoint**.

This point is particularly valuable because defenders can correlate several abnormal events that should normally not occur together:

1. A trusted software updater installs or modifies application files.
2. The newly installed software launches unexpected child processes or modules.
3. The application initiates outbound communication to unusual infrastructure.
4. The destination does not correspond to expected vendor update infrastructure.
5. The process or network behavior is inconsistent with normal SolarWinds Orion activity.

MITRE's current detection guidance for T1195.002 specifically recommends correlating updater activity, file changes, first-run child processes, module/signature anomalies, and unexpected outbound connections.

This point is preferable to relying only on traditional antivirus detection because the software may be legitimately signed and installed through a legitimate update mechanism. Detecting **behavior and relationships between events**, rather than only malicious file signatures, provides a stronger opportunity to identify a supply-chain compromise.

However, identity monitoring is an equally important secondary control. If the attackers progress beyond the initial foothold, abnormal SAML trust changes, new service principals, new authentication material, mailbox permissions, and unusual cloud authentication should be treated as high-priority events. CISA specifically recommended investigating potential SAML abuse following the SolarWinds compromise.

---

# 6. Detection and Mitigation

## Technique 1: T1195.002 — Compromise Software Supply Chain

### Detection Idea

Monitor software update installations and correlate them with process creation, file modifications, digital-signature information, and outbound network connections.

A legitimate software update should generally behave consistently with previous versions. An update that unexpectedly launches PowerShell, creates unusual child processes, loads unexpected DLLs, modifies unrelated files, or communicates with previously unknown external infrastructure should receive additional investigation.

### Relevant Log Sources / Data Components

- Windows Sysmon Process Creation logs
- Endpoint Detection and Response (EDR) telemetry
- Windows Event Logs
- File integrity monitoring
- Application installation/update logs
- DNS logs
- Proxy/firewall logs
- Network connection telemetry

MITRE's detection strategy for this technique specifically recommends correlating installer/updater execution with file changes, first-run child processes, module/signature anomalies, and unexpected outbound connections.

### What Defenders Should Look For

Defenders should look for:

- Unexpected child processes created by software updaters
- Software updates communicating with unknown domains
- New or modified binaries immediately after an update
- Unexpected PowerShell or command-shell execution
- Digital-signature anomalies
- New outbound connections immediately following installation

### MITRE Mitigation

**M1051 — Update Software**

Organizations should maintain controlled software-update processes and ensure that software and components are appropriately maintained and monitored.

---

## Technique 2: T1606.002 — Forge Web Credentials: SAML Tokens

### Detection Idea

Monitor identity-provider and federation logs for abnormal SAML activity, especially changes to federation trust, certificates, authentication patterns, and service principals.

A particularly suspicious event would be a change to federation configuration followed by authentication using tokens associated with newly introduced or unexpected signing certificates.

### Relevant Log Sources / Data Components

- Azure AD / Microsoft Entra ID audit logs
- Active Directory Federation Services logs
- Identity-provider authentication logs
- Cloud application logs
- SAML authentication records
- Certificate-management logs
- Conditional Access logs

### What Defenders Should Look For

Defenders should investigate:

- Unexpected changes to federation trust
- Newly introduced SAML signing certificates
- Authentication using unusual token issuers
- Successful authentication despite expected MFA requirements
- New service principals or application credentials
- Administrative changes followed immediately by unusual cloud access

MITRE documents that APT29 used forged SAML tokens to impersonate users and bypass MFA.

### MITRE Mitigation

**M1042 — Disable or Remove Feature or Program**

Organizations should disable unnecessary federation or authentication functionality and remove unnecessary trust relationships or authentication mechanisms where they are not required.

In addition, strong identity governance, certificate protection, and monitoring of federation configuration changes are critical.

---

## Technique 3: T1114.002 — Email Collection: Remote Email Collection

### Detection Idea

Monitor Exchange and Microsoft 365 audit logs for unusual mailbox export operations, mailbox permission changes, or applications suddenly requesting access to multiple mailboxes.

During the SolarWinds campaign, APT29 used Exchange commands including `New-MailboxExportRequest` and `Get-MailboxExportRequest` to collect emails from selected individuals.

### Relevant Log Sources / Data Components

- Microsoft 365 Unified Audit Log
- Exchange audit logs
- Exchange Management Shell logs
- Azure/Microsoft Entra ID logs
- Mailbox access logs
- EDR telemetry on Exchange servers

### What Defenders Should Look For

Examples include:

- Mailbox export requests outside normal administrative procedures
- Sudden access to executive mailboxes
- Unexpected mailbox delegation
- New application permissions such as Mail.Read or Mail.ReadWrite
- Administrative commands executed from unusual accounts
- Large-scale mailbox access from a single identity

### MITRE Mitigation

**M1026 — Privileged Account Management**

Organizations should strictly control privileged accounts, reduce unnecessary administrative privileges, monitor privileged activity, and separate administrative identities from normal user accounts.

---

# 7. Conclusion

The SolarWinds Compromise demonstrates how a sophisticated threat actor can combine a software supply-chain attack with identity abuse, legitimate administrative tools, lateral movement, and data collection to conduct a long-term espionage operation.

APT29, attributed to Russia's SVR, compromised the SolarWinds software development and build process and used SUNSPOT to facilitate the insertion of SUNBURST into Orion software builds. The malicious update was subsequently distributed through a trusted software-update mechanism, allowing APT29 to reach numerous downstream organizations.

The attackers did not rely solely on malware after obtaining access. They performed internal discovery, identified accounts and trust relationships, obtained credentials, manipulated cloud identities, abused federation trust, forged SAML tokens, and used legitimate remote administration mechanisms. They subsequently collected email, source code, files, and other information before staging and exfiltrating the data.

From a defensive perspective, the campaign demonstrates that detecting advanced threats requires more than identifying known malware. Organizations must monitor software-update behavior, identity infrastructure, privileged activity, cloud authentication, lateral movement, and unusual data access. Supply-chain security is particularly important because a trusted and digitally signed application can still become a delivery mechanism for malicious activity.

The most important lesson from SolarWinds is that security controls must operate across the entire attack lifecycle. Preventing supply-chain compromise is valuable, but defenders must also assume that an attacker may bypass the initial security boundary and therefore maintain visibility into identity, endpoint, network, cloud, and data-access activity. By mapping these behaviors to MITRE ATT&CK, defenders can develop detections and mitigations based on attacker behavior rather than relying exclusively on malware signatures.

---

# 8. References

1. MITRE ATT&CK. **APT29 — G0016.** APT29 group profile, aliases, attribution, and associated campaigns.

2. MITRE ATT&CK. **SolarWinds Compromise — C0024.** Campaign profile, techniques, procedures, and attribution.

3. MITRE ATT&CK. **Supply Chain Compromise: Compromise Software Supply Chain — T1195.002.** Procedure examples and detection/mitigation information.

4. MITRE ATT&CK. **SUNSPOT — S0562.** Information on SUNSPOT's role in inserting SUNBURST into SolarWinds Orion builds.

5. Cybersecurity and Infrastructure Security Agency (CISA). **Russian SVR Targets U.S. and Allied Networks.** Joint advisory concerning SVR activity and the SolarWinds compromise.

6. CISA. **SolarWinds and Active Directory/M365 Compromise: Detecting APT Activity from Known TTPs.** Guidance for identifying post-compromise activity.

7. CISA. **Remediating Networks Affected by the SolarWinds and Active Directory/M365 Compromise.** Guidance concerning identity compromise, SAML abuse, and remediation.

8. National Security Agency, CISA, FBI, and UK NCSC. **Further TTPs Associated with SVR Cyber Actors (APT29).** Joint advisory on SVR techniques and mitigation.
