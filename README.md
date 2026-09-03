# SOC Blind Spot — Automated Detection Coverage & Telemetry Gap Assessment Platform

## Overview

SOC Blind Spot is a hands-on cybersecurity lab project built using Wazuh, Windows 11, Ubuntu Linux, VMware, PowerShell, and Windows Event Logs.

The project evaluates how effectively a SIEM detects security activity from a Windows endpoint and identifies telemetry gaps where security events are generated locally but are not observed by the SIEM.

## Objectives

- Deploy and configure Wazuh in a VMware-based lab.
- Connect and monitor a Windows 11 endpoint.
- Validate Windows security event collection.
- Test multiple security detection use cases.
- Identify telemetry and detection blind spots.
- Perform CIS-aligned Windows security hardening.
- Document detection coverage and security findings.

## Lab Architecture

```text
Windows 11 Endpoint
IP: 192.168.63.132
        |
        | Wazuh Agent
        |
        v
Ubuntu Wazuh Server
IP: 192.168.63.131
        |
        +-- Wazuh Manager
        +-- Wazuh Indexer
        +-- Wazuh Dashboard
        
## Technologies

* Wazuh 4.14.7
* Windows 11
* Ubuntu Linux
* VMware
* PowerShell
* Windows Event Logs
* CIS Security Configuration Assessment (SCA)

## Detection Coverage

The project tested 9 controlled security-event use cases.

| Use Case | Windows Event ID | Result |
|---|---:|---|
| Successful Logon | 4624 | Detected |
| Failed Logon | 4625 | Detected |
| PowerShell Activity | PowerShell Operational | Detected |
| Windows Application Error | Application Error | Detected |
| Windows Defender Scan | 1001 | Detected |
| Process Creation | 4688 | Detected |
| User Account Creation | 4720 | Detected |
| Scheduled Task Creation | 4698 | Detected |
| Windows Firewall Activity | 2097 / 2052 | Blind Spot |

### Observed Test-Case Coverage

8 of 9 tested use cases were observed in Wazuh.

**Observed coverage: 88.9%**

This percentage represents the controlled test cases performed in this project. It does not represent the overall detection capability of Wazuh or Windows.
## PowerShell Telemetry Gap

PowerShell Operational events were initially generated on the Windows endpoint but were not observed in Wazuh.

The investigation showed that Windows was producing PowerShell Operational events locally.

PowerShell Script Block Logging was enabled and the Wazuh agent configuration was updated to collect:

`Microsoft-Windows-PowerShell/Operational`

After the configuration change, PowerShell events were successfully observed in Wazuh Threat Hunting.

## Windows Security Hardening

CIS-aligned Windows security controls were reviewed and selected controls were hardened.

Examples include:

- Minimum password length
- Password history
- Minimum password age
- Account lockout threshold
- Account lockout duration
- Account lockout observation window
- Microsoft account restrictions
- Administrator account rename
- Audit policy settings
- Printer driver installation restrictions
- Anonymous access restrictions
- Network security registry settings
- Cached logon settings

## SCA Result

Latest documented SCA result:

- Passed: 145
- Failed: 328
- Not Applicable: 9
- Compliance Score: 30%

The SCA score is a configuration-hardening measurement and is separate from the detection coverage metric.

## Primary Blind Spot

### Windows Firewall Telemetry

Windows Firewall events were confirmed locally in Windows Event Viewer.

Relevant event IDs included:

- 2097 — Firewall rule added
- 2052 — Firewall rule deleted

The Wazuh agent was configured to analyze the Windows Firewall event channel, but the corresponding Firewall events were not observed in Wazuh Threat Hunting during the test.

### Finding

**Windows Firewall telemetry was identified as the primary observed telemetry blind spot in this project.**

The exact root cause requires further investigation into event ingestion, filtering, decoding, or rule handling.

## Key Findings

1. Wazuh successfully monitored the Windows endpoint.
2. Authentication events were detected.
3. PowerShell telemetry required additional configuration.
4. Process creation telemetry was detected.
5. Windows Defender scan activity was detected.
6. Application error events were detected.
7. User and scheduled-task activity was tested.
8. Windows Firewall activity was generated locally but was not observed in Wazuh.
9. CIS-aligned security hardening improved selected Windows security controls.

## Security Recommendations

- Continue investigating Windows Firewall event ingestion.
- Validate Wazuh event-channel collection and decoders.
- Develop custom detection rules where required.
- Enable additional Windows security auditing.
- Regularly perform SCA assessments.
- Monitor telemetry coverage after configuration changes.
- Maintain a detection coverage matrix for critical Windows security events.

## Limitations

- The lab uses a Windows 11 Home endpoint while the selected CIS benchmark is designed for Windows 11 Enterprise.
- The detection coverage percentage is based only on the controlled test cases performed.
- The Firewall blind spot root cause was not fully isolated.
- Some evidence in the project documentation is simulated and is clearly labeled as such.

## Project Outcome

The project demonstrates a practical SOC workflow:

**Generate Security Activity → Collect Telemetry → Analyze in SIEM → Validate Detection → Identify Blind Spots → Harden Endpoint → Document Findings**

The main outcome is a repeatable approach for assessing endpoint detection coverage and identifying missing security telemetry.

## Author

MOURYA SAI REDDY N
        
