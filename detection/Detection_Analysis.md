> Part of [APT29 / SolarWinds: Threat Intelligence & MITRE ATT&CK Analysis](../README.md)

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
