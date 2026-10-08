# APT29 / SolarWinds: Threat Intelligence & MITRE ATT&CK Analysis

A structured analysis of **APT29** and the **2019–2020 SolarWinds supply-chain compromise** (MITRE campaign C0024), mapped to **MITRE ATT&CK**. It runs from actor profile to attack chain, technique mapping, detection ideas and defensive lessons.

> Based on public reporting from MITRE ATT&CK and government advisories. Sources are in [`references/References.md`](references/References.md).

---

## Threat actor at a glance

| | |
|---|---|
| **Group** | APT29 (MITRE ATT&CK G0016) |
| **Also tracked as** | Cozy Bear, The Dukes, CozyDuke, NOBELIUM, UNC2452, SolarStorm, Dark Halo, Midnight Blizzard |
| **Attribution** | Russia's Foreign Intelligence Service (SVR) |
| **Motivation** | Cyber espionage and intelligence collection |
| **Campaign** | SolarWinds Compromise (C0024), first observed August 2019, last observed January 2021 |

## Attack flow

```text
Compromise SolarWinds build process (SUNSPOT)
        ↓
Trojanized Orion update delivers SUNBURST
        ↓
Selective follow-on activity at chosen victims
        ↓
Discovery, credential and identity abuse (federation trust, forged SAML tokens)
        ↓
Lateral movement with legitimate admin tools (RDP, SMB, WinRM)
        ↓
Collection of email, source code and files
        ↓
Staging and exfiltration over HTTPS
```

## Where to start

| Question | Read this |
|---|---|
| Who is APT29? | [`analysis/APT29_Profile.md`](analysis/APT29_Profile.md) |
| What happened in the campaign? | [`analysis/Campaign_Narrative.md`](analysis/Campaign_Narrative.md) |
| How did the attack unfold stage by stage? | [`analysis/Attack_Chain.md`](analysis/Attack_Chain.md) |
| Which ATT&CK techniques were used? | [`mitre/Technique_Mapping.md`](mitre/Technique_Mapping.md) |
| How can defenders detect it? | [`detection/Detection_Analysis.md`](detection/Detection_Analysis.md) |
| What should organisations learn? | [`analysis/Defensive_Lessons.md`](analysis/Defensive_Lessons.md) |
| The full report in one file | [`report/APT29_SolarWinds_Final_Report.md`](report/APT29_SolarWinds_Final_Report.md) |
| Sources | [`references/References.md`](references/References.md) |

## Mapped ATT&CK techniques (24)

Each technique is documented by MITRE for the SolarWinds Compromise. Full descriptions of how APT29 used each one are in [`mitre/Technique_Mapping.md`](mitre/Technique_Mapping.md).

| ID | Technique | Tactic |
|---|---|---|
| `T1195.002` | Supply Chain Compromise: Compromise Software Supply Chain | Initial Access |
| `T1553.002` | Subvert Trust Controls: Code Signing | Defense Evasion |
| `T1059.001` | Command and Scripting Interpreter: PowerShell | Execution |
| `T1087.002` | Account Discovery: Domain Account | Discovery |
| `T1482` | Domain Trust Discovery | Discovery |
| `T1555.003` | Credentials from Web Browsers: Chrome | Credential Access |
| `T1098.001` | Account Manipulation: Additional Cloud Credentials | Persistence |
| `T1098.003` | Account Manipulation: Additional Cloud Roles | Persistence / Privilege Escalation |
| `T1484.002` | Domain or Tenant Policy Modification: Trust Modification | Defense Evasion / Privilege Escalation |
| `T1606.002` | Forge Web Credentials: SAML Tokens | Credential Access |
| `T1550.001` | Use Alternate Authentication Material: Application Access Token | Defense Evasion / Lateral Movement |
| `T1021.001` | Remote Services: RDP | Lateral Movement |
| `T1021.002` | Remote Services: SMB/Windows Admin Shares | Lateral Movement |
| `T1021.006` | Remote Services: Windows Remote Management | Lateral Movement / Execution |
| `T1053.005` | Scheduled Task/Job: Scheduled Task | Persistence / Execution |
| `T1114.002` | Email Collection: Remote Email Collection | Collection |
| `T1213.003` | Data from Information Repositories: Code Repositories | Collection |
| `T1074.002` | Data Staged: Remote Data Staging | Collection |
| `T1560.001` | Archive Collected Data: Archive via Utility | Collection |
| `T1071.001` | Application Layer Protocol: Web Protocols | Command and Control |
| `T1568` | Dynamic Resolution | Command and Control |
| `T1048.002` | Exfiltration Over Alternative Protocol: Asymmetric Encrypted Non-C2 Protocol | Exfiltration |
| `T1685.001` | Disable or Modify Windows Event Log | Defense Evasion |
| `T1686` | Disable or Modify System Firewall | Defense Evasion |

## Detection focus

The analysis argues that the best point to detect and disrupt the operation is right after the compromised Orion update is installed and starts running, by correlating updater activity, unexpected child processes and unusual outbound connections. Identity monitoring (federation trust changes, new service principals, mailbox export activity) is the key secondary control. Detection ideas, log sources and MITRE mitigations for three techniques are in [`detection/Detection_Analysis.md`](detection/Detection_Analysis.md):

- `T1195.002` Compromise Software Supply Chain
- `T1606.002` Forge Web Credentials: SAML Tokens
- `T1114.002` Email Collection: Remote Email Collection

## ATT&CK Navigator layer

1. Open the [ATT&CK Navigator](https://mitre-attack.github.io/attack-navigator/).
2. Choose **Open Existing Layer**, then **Upload from local**.
3. Select [`mitre/APT29_SolarWinds_Navigator_Layer.json`](mitre/APT29_SolarWinds_Navigator_Layer.json).

The 24 techniques above are highlighted on the Enterprise matrix.

## Repository structure

| Folder | Contents |
|---|---|
| [`analysis/`](analysis/) | Actor profile, campaign narrative, attack chain, defensive lessons |
| [`mitre/`](mitre/) | Technique mapping and ATT&CK Navigator layer |
| [`detection/`](detection/) | Detection ideas, log sources and mitigations |
| [`references/`](references/) | Source list |
| [`report/`](report/) | Full report in one file |

---

Part of my security portfolio: [github.com/chagbox](https://github.com/chagbox)
