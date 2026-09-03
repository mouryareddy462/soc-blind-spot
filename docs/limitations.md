# Project Limitations

## Overview

This project was performed as a controlled cybersecurity laboratory exercise using virtual machines, Wazuh, Windows 11, and Ubuntu Linux.

The following limitations should be considered when interpreting the results.

## 1. Windows Edition

The monitored endpoint used Windows 11 Home.

The selected CIS benchmark was designed for Windows 11 Enterprise.

Because of this difference, some benchmark controls may not be applicable or may behave differently on the lab endpoint.

## 2. Detection Coverage Scope

The project tested 9 controlled security-event use cases.

8 of the 9 tested use cases were observed in Wazuh.

The resulting 88.9% figure represents observed coverage of these specific test cases only.

It does not represent the overall detection coverage of Wazuh, Windows, or a production SOC environment.

## 3. Firewall Telemetry

Windows Firewall events were confirmed locally in Windows Event Viewer.

However, the corresponding events were not observed in Wazuh Threat Hunting during the test.

The exact root cause of this telemetry gap was not fully isolated.

Further investigation would be required to determine whether the issue is related to event ingestion, filtering, decoding, or detection-rule handling.

## 4. Controlled Laboratory Environment

Testing was performed in a VMware virtualized environment.

The results may differ from a production environment with:

- Multiple endpoints
- Domain controllers
- Enterprise Windows editions
- Centralized Active Directory
- Additional security products
- Network security infrastructure
- Production-scale event volumes

## 5. Limited Test Cases

The project focused on selected security activities rather than every possible Windows security event.

Additional testing would be required to evaluate broader detection coverage.

## 6. Simulated Evidence

Some documentation contains simulated evidence.

The simulated Firewall evidence is clearly labeled:

**SIMULATED EVIDENCE**

It must not be interpreted as an actual Windows or Wazuh screenshot.

## 7. Root Cause Analysis

The Firewall telemetry blind spot was identified, but the project did not fully isolate the technical root cause.

A deeper investigation could include:

- Reviewing Wazuh agent logs
- Validating event-channel ingestion
- Inspecting raw event data
- Reviewing decoders
- Reviewing Wazuh rules
- Testing custom detection rules
- Comparing endpoint and manager-side telemetry

## 8. SCA Interpretation

The Wazuh SCA score is a security configuration assessment metric.

The latest documented result was:

- Passed: 145
- Failed: 328
- Not Applicable: 9
- Compliance Score: 30%

This score should not be interpreted as the project's detection coverage percentage.

## Conclusion

These limitations do not invalidate the project results.

Instead, they define the scope of the laboratory assessment and identify areas for future investigation and improvement.
