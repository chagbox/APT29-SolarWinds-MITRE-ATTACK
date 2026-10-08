# APT29 / SolarWinds: Threat Intelligence & MITRE ATT&CK Analysis

A structured analysis of **APT29** and the **2019–2020 SolarWinds supply-chain campaign**, mapped to **MITRE ATT&CK**. The project follows the full path from actor profile to attack chain, technique mapping, detection ideas and defensive lessons.

> Based on public reporting and the MITRE ATT&CK knowledge base. All sources are listed in [`references/References.md`](references/References.md).

---

## Threat actor

APT29 is also tracked as **Cozy Bear** and **The Dukes**, and is publicly attributed to Russia's foreign intelligence service, the **SVR**.

## What this project covers

```text
APT29 profile
      ↓
SolarWinds campaign
      ↓
Attack chain
      ↓
MITRE ATT&CK technique mapping (15+ techniques)
      ↓
Detection analysis
      ↓
Defensive lessons
      ↓
ATT&CK Navigator layer
      ↓
References and final report
```

## Where to start

| Question | Read this |
|---|---|
| Who is APT29? | [`analysis/APT29_Profile.md`](analysis/APT29_Profile.md) |
| What happened in the SolarWinds campaign? | [`analysis/Campaign_Narrative.md`](analysis/Campaign_Narrative.md) |
| How did the attack unfold, step by step? | [`analysis/Attack_Chain.md`](analysis/Attack_Chain.md) |
| Which ATT&CK techniques were used? | [`mitre/Technique_Mapping.md`](mitre/Technique_Mapping.md) |
| How could defenders detect it? | [`detection/Detection_Analysis.md`](detection/Detection_Analysis.md) |
| What should organisations learn from it? | [`analysis/Defensive_Lessons.md`](analysis/Defensive_Lessons.md) |
| What sources were used? | [`references/References.md`](references/References.md) |

## Repository structure

| Folder | Contents |
|---|---|
| [`analysis/`](analysis/) | Actor profile, campaign narrative, attack chain, defensive lessons |
| [`mitre/`](mitre/) | Technique mapping and the ATT&CK Navigator layer (JSON) |
| [`detection/`](detection/) | Detection analysis for the mapped techniques |
| [`references/`](references/) | Source list |
| [`report/`](report/) | Final report |

## Using the ATT&CK Navigator layer

1. Open the [ATT&CK Navigator](https://mitre-attack.github.io/attack-navigator/).
2. Choose **Open Existing Layer**, then **Upload from local**.
3. Select the layer JSON from the [`mitre/`](mitre/) folder.

The techniques used in this campaign will be highlighted on the ATT&CK matrix.

## Skills shown in this project

Threat intelligence analysis, MITRE ATT&CK mapping, attack-chain analysis, detection engineering, and technical reporting.

---

Part of my security portfolio: [github.com/chagbox](https://github.com/chagbox)
