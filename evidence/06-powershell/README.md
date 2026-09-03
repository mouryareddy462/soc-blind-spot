# PowerShell Detection Evidence

Evidence of PowerShell telemetry validation.

## Initial Finding

PowerShell activity was initially not visible in Wazuh Threat Hunting.

## Remediation

PowerShell Script Block Logging was enabled and the Wazuh agent was configured to collect:

`Microsoft-Windows-PowerShell/Operational`

## Result

PowerShell events were subsequently observed in Wazuh Threat Hunting.

Observed Wazuh rule:
- Rule 91816
