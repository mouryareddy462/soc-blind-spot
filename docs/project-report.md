# SOC Blind Spot — Project Report

## 1. Executive Summary

SOC Blind Spot is a hands-on cybersecurity lab project designed to assess Windows endpoint detection coverage and identify security telemetry blind spots using Wazuh.

The lab consists of a Windows 11 endpoint monitored by a Wazuh agent and an Ubuntu-based Wazuh server deployed in VMware.

The project tested multiple Windows security-event use cases, performed CIS-aligned security hardening, and analyzed whether generated security events were successfully observed in Wazuh.

A total of 9 controlled detection use cases were tested. 8 were observed in Wazuh, resulting in an observed test-case coverage of 88.9%.

The primary observed blind spot was Windows Firewall telemetry. Firewall events were generated and confirmed locally in Windows Event Viewer, but the corresponding events were not observed in Wazuh Threat Hunting during the test.

---

## 2. Objectives

The main objectives of the project were:

- Deploy a functional Wazuh SOC monitoring environment.
- Connect a Windows 11 endpoint to Wazuh.
- Validate Windows security-event collection.
- Test different endpoint security scenarios.
- Identify missing or incomplete telemetry.
- Perform CIS-aligned Windows security hardening.
- Measure observed detection coverage.
- Document security findings and recommendations.

---

## 3. Laboratory Environment

### Wazuh Server

- Operating System: Ubuntu Linux
- IP Address: 192.168.63.131
- Wazuh Version: 4.14.7
- Deployment: VMware
- Components:
  - Wazuh Manager
  - Wazuh Indexer
  - Wazuh Dashboard

### Windows Endpoint

- Operating System: Windows 11 Home
- IP Address: 192.168.63.132
- Wazuh Agent Version: 4.14.7
- Agent Status: Active
- Agent Group: default

---

## 4. Wazuh Deployment and Validation

The Wazuh server was deployed in the Ubuntu virtual machine.

The Windows endpoint was configured with the Wazuh agent and successfully connected to the Wazuh manager.

Network connectivity between the endpoint and server was validated using:

- ICMP connectivity
- Wazuh agent communication port 1514
- Wazuh enrollment port 1515

The Wazuh dashboard showed the Windows endpoint as active.

Security events generated on the Windows endpoint were subsequently visible through Wazuh Threat Hunting.

---

## 5. Security Configuration Assessment

Wazuh Security Configuration Assessment (SCA) was used to evaluate Windows security configuration against the selected CIS Microsoft Windows 11 benchmark.

The latest documented SCA result was:

- Passed: 145
- Failed: 328
- Not Applicable: 9
- Compliance Score: 30%

Selected security controls were hardened during the project.

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
- Network security settings
- Cached logon settings

The SCA score represents configuration-hardening status and is separate from the detection coverage measurement.

---

## 6. Detection Coverage Methodology

Controlled security activities were generated on the Windows endpoint.

The corresponding Windows events were then investigated using:

1. Windows Event Viewer
2. Wazuh agent telemetry
3. Wazuh Threat Hunting

Each test was classified as:

- Detected
- Not Detected / Blind Spot

The purpose was to determine whether security activity visible locally on the endpoint was also visible to the SOC monitoring platform.

---

## 7. Detection Coverage Matrix

| Use Case | Event ID | Wazuh Result |
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

---

## 8. Detection Test Results

### 8.1 Successful Logon

Windows Security Event ID 4624 was generated and observed in Wazuh Threat Hunting.

Wazuh identified the activity as a successful local logon.

Result: DETECTED

---

### 8.2 Failed Logon

Failed authentication attempts were generated using an incorrect password.

Windows Security Event ID 4625 was observed in Wazuh.

Result: DETECTED

---

### 8.3 PowerShell Activity

PowerShell activity was initially generated on the Windows endpoint but was not immediately visible in Wazuh.

Windows PowerShell Operational events were confirmed locally.

PowerShell Script Block Logging was enabled and the Wazuh agent configuration was updated to collect:

`Microsoft-Windows-PowerShell/Operational`

After the configuration change, PowerShell events were successfully observed in Wazuh Threat Hunting.

Wazuh Rule 91816 was observed for PowerShell-related events.

Result: DETECTED AFTER TELEMETRY CONFIGURATION

---

### 8.4 Windows Application Error

A Windows application error event was generated and observed through Wazuh.

Wazuh identified the activity with Rule 66002.

Result: DETECTED

---

### 8.5 Windows Defender

A Windows Defender quick scan was executed on the endpoint.

Windows Defender Operational events were generated locally.

Event ID 1001 was observed in Wazuh with the description:

Windows Defender: Antimalware scan finished

Wazuh Rule 62108 was observed.

Result: DETECTED

---

### 8.6 Process Creation

Windows process creation auditing was enabled.

Security Event ID 4688 was generated by launching a new process.

The activity was observed in Wazuh.

Wazuh Rule 67027 was associated with the process creation detection.

Result: DETECTED

---

### 8.7 User Account Creation

A controlled user-account creation test was performed using Windows security auditing.

Security Event ID 4720 was used to validate account-creation telemetry.

Result: DETECTED

---

### 8.8 Scheduled Task Creation

A controlled scheduled-task creation test was performed.

Security Event ID 4698 was used to validate scheduled-task telemetry.

Result: DETECTED

---

### 8.9 Windows Firewall Telemetry

Windows Firewall activity was generated locally.

Windows Event Viewer showed Firewall events including:

- Event ID 2097 — Firewall rule added
- Event ID 2052 — Firewall rule deleted

The Wazuh agent was configured to analyze the Windows Firewall event channel.

However, the corresponding Firewall events were not observed in Wazuh Threat Hunting during the test.

Result: BLIND SPOT

The exact root cause was not fully isolated and may require further investigation into event ingestion, filtering, decoding, or rule handling.

---

## 9. Detection Coverage Metric

A total of 9 controlled security-event use cases were tested.

- Detected: 8
- Blind Spot: 1

Observed test-case coverage:

**8 / 9 × 100 = 88.9%**

This metric applies only to the controlled test cases performed in this project.

It should not be interpreted as the overall detection capability of Wazuh or Windows.

---

## 10. Key Findings

1. Wazuh successfully monitored the Windows endpoint.
2. Authentication events were successfully detected.
3. PowerShell telemetry required additional configuration.
4. PowerShell events were successfully observed after telemetry configuration.
5. Windows Defender scan activity was detected.
6. Process creation telemetry was detected.
7. Application error activity was detected.
8. User-account and scheduled-task activity were tested.
9. Windows Firewall activity was confirmed locally but was not observed in Wazuh.
10. CIS-aligned hardening improved selected Windows security controls.

---

## 11. Primary Blind Spot

The primary observed blind spot was Windows Firewall telemetry.

The endpoint generated Firewall events and Windows Event Viewer confirmed their presence.

The Wazuh agent was also configured to analyze the relevant Firewall event channel.

Despite this, the events were not observed in Wazuh Threat Hunting during the test.

This demonstrates the importance of validating telemetry at both the endpoint and SIEM levels.

A security event existing on the endpoint does not automatically guarantee that the SOC can observe or detect it.

---

## 12. Security Recommendations

The following improvements are recommended:

- Continue investigating Windows Firewall event ingestion.
- Validate Wazuh event-channel collection.
- Review Wazuh decoders and detection rules.
- Develop custom detection rules where required.
- Enable additional Windows security auditing.
- Regularly perform Wazuh SCA assessments.
- Maintain a detection coverage matrix.
- Re-test telemetry after configuration changes.
- Monitor critical endpoint security logs continuously.

---

## 13. Limitations

- The Windows endpoint used Windows 11 Home.
- The selected CIS benchmark is designed for Windows 11 Enterprise.
- Detection coverage was measured only against the controlled test cases performed.
- The Firewall telemetry root cause was not fully isolated.
- Some evidence in the project documentation is simulated and is clearly labeled as simulated evidence.
- The project does not claim complete coverage of all Windows security events.

---

## 14. Evidence

The project includes evidence screenshots covering:

- Wazuh dashboard
- Wazuh agent status
- SCA results
- Windows security configuration
- Successful logon detection
- Failed logon detection
- PowerShell telemetry configuration
- PowerShell detection
- Application error detection
- Windows Defender detection
- Process creation detection
- Windows Firewall telemetry testing

The simulated Firewall evidence is clearly labeled:

**SIMULATED EVIDENCE**

It is not presented as an actual Windows or Wazuh screenshot.

---

## 15. Project Outcome

The project demonstrates a practical SOC detection-validation workflow:

**Generate Security Activity**

↓

**Collect Endpoint Telemetry**

↓

**Analyze Events in Wazuh**

↓

**Validate Detection**

↓

**Identify Telemetry Gaps**

↓

**Harden the Endpoint**

↓

**Document Findings**

The project demonstrates that effective SOC monitoring requires more than simply deploying a SIEM. Endpoint telemetry must be validated, detection coverage must be tested, and blind spots must be identified and investigated.

---

## 16. Conclusion

The SOC Blind Spot project successfully demonstrated Windows endpoint monitoring using Wazuh and identified a real telemetry limitation during controlled testing.

Most tested security activities were successfully observed in Wazuh, while Windows Firewall telemetry remained the primary observed blind spot.

The project also demonstrated practical security hardening using CIS-aligned configuration controls and showed how telemetry configuration changes can improve detection visibility.

The final result is a repeatable methodology for assessing endpoint telemetry, validating SIEM detection coverage, identifying blind spots, and improving SOC monitoring.

---

## Author

MOURYA SAI REDDY N
