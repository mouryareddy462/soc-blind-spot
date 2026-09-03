# Detection Coverage Matrix

## Overview

This matrix documents the controlled security-event tests performed against the Windows 11 endpoint and records whether the activity was observed in Wazuh.

## Detection Matrix

| # | Security Use Case | Windows Event / Source | Wazuh Result | Status |
|---|---|---|---|---|
| 1 | Successful Logon | 4624 | Observed in Threat Hunting | Detected |
| 2 | Failed Logon | 4625 | Observed in Threat Hunting | Detected |
| 3 | PowerShell Activity | PowerShell Operational | Observed after telemetry configuration | Detected |
| 4 | Windows Application Error | Application Error | Observed in Threat Hunting | Detected |
| 5 | Windows Defender Scan | 1001 | Observed in Threat Hunting | Detected |
| 6 | Process Creation | 4688 | Observed in Threat Hunting | Detected |
| 7 | User Account Creation | 4720 | Tested and observed | Detected |
| 8 | Scheduled Task Creation | 4698 | Tested and observed | Detected |
| 9 | Windows Firewall Activity | 2097 / 2052 | Not observed in Threat Hunting | Blind Spot |

## Observed Coverage

Total controlled test cases: **9**

Detected test cases: **8**

Observed blind spots: **1**

### Coverage Calculation

**8 / 9 × 100 = 88.9%**

The **88.9% value represents observed coverage of the controlled test cases in this project only**. It does not represent the overall detection coverage of Wazuh or Windows.

## Key Detection Results

### Authentication

- Event ID 4624 — Successful Logon — Detected
- Event ID 4625 — Failed Logon — Detected

### PowerShell

PowerShell Operational telemetry was initially missing from Wazuh.

After enabling PowerShell Script Block Logging and configuring the Wazuh agent to collect:

`Microsoft-Windows-PowerShell/Operational`

PowerShell events were observed in Wazuh.

### Windows Defender

Windows Defender scan completion activity was observed using Event ID 1001.

Wazuh Rule 62108 was observed.

### Process Creation

Windows process creation telemetry was validated using Event ID 4688.

Wazuh Rule 67027 was observed.

### Windows Application Error

Windows Application Error activity was observed in Wazuh.

Wazuh Rule 66002 was observed.

## Primary Blind Spot

### Windows Firewall

Windows Firewall events were confirmed locally in Windows Event Viewer.

Relevant events included:

- Event ID 2097 — Firewall rule added
- Event ID 2052 — Firewall rule deleted

The Wazuh agent was configured to analyze the Windows Firewall event channel.

However, the corresponding Firewall events were not observed in Wazuh Threat Hunting during the test.

**Status: BLIND SPOT**

The exact root cause requires further investigation into event ingestion, filtering, decoding, or rule handling.

## Conclusion

The detection matrix demonstrates that most of the controlled Windows security activities tested in this project were observable through Wazuh.

The testing also demonstrated the importance of validating telemetry at both the endpoint and SIEM levels.

A security event being present on the endpoint does not automatically guarantee that the SOC can observe or detect it.
