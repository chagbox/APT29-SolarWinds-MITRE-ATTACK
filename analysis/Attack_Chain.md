> Part of [APT29 / SolarWinds: Threat Intelligence & MITRE ATT&CK Analysis](../README.md)

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
