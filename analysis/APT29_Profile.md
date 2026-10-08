> Part of [APT29 / SolarWinds: Threat Intelligence & MITRE ATT&CK Analysis](../README.md)

# 1. Introduction

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
