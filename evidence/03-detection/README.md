# Detection Evidence

Evidence of controlled security-event detection using Wazuh Threat Hunting.

Tested detections include:

- Successful Logon — Event ID 4624
- Failed Logon — Event ID 4625
- PowerShell Operational Events
- Windows Application Error
- Windows Defender Scan — Event ID 1001
- Process Creation — Event ID 4688
- User Creation — Event ID 4720
- Scheduled Task Creation — Event ID 4698

Observed test-case coverage:
- 8 detected
- 1 observed blind spot
- 8/9 = 88.9%
