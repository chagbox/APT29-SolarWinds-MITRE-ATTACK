> Part of [APT29 / SolarWinds: Threat Intelligence & MITRE ATT&CK Analysis](../README.md)

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
