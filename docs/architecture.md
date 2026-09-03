# SOC Blind Spot — Architecture

## Overview

The SOC Blind Spot lab uses a Windows 11 endpoint connected to a Wazuh server running on Ubuntu Linux in VMware.

The architecture is designed to generate security activity on the Windows endpoint, collect the resulting telemetry through the Wazuh agent, and analyze the events in the Wazuh platform.

## Architecture

```text
+-----------------------------+
|       Windows 11 VM         |
|                             |
| IP: 192.168.63.132          |
|                             |
| - Wazuh Agent               |
| - Windows Event Logs        |
| - PowerShell                |
| - Windows Defender          |
| - Windows Firewall          |
+--------------+--------------+
               |
               | Security Telemetry
               |
               v
+-----------------------------+
|       Ubuntu Linux VM       |
|                             |
| IP: 192.168.63.131          |
|                             |
| - Wazuh Manager             |
| - Wazuh Indexer             |
| - Wazuh Dashboard           |
+--------------+--------------+
               |
               v
+-----------------------------+
|       SOC Analyst           |
|                             |
| - Threat Hunting            |
| - Event Analysis            |
| - Detection Validation      |
| - Blind Spot Identification |
+-----------------------------+

## Components

### Windows 11 Endpoint

The Windows 11 virtual machine acts as the monitored endpoint.

The endpoint generates security activity such as:

- Successful logons
- Failed logons
- PowerShell activity
- Process creation
- Windows Defender activity
- Application errors
- User account activity
- Scheduled task activity
- Windows Firewall activity

The Wazuh agent collects selected Windows telemetry and forwards it to the Wazuh server.

### Wazuh Manager

The Wazuh Manager receives telemetry from the Windows endpoint and processes security events.

It applies Wazuh rules and generates security alerts when matching activity is detected.

### Wazuh Indexer

The Wazuh Indexer stores the collected security events and alerts so they can be searched and analyzed.

### Wazuh Dashboard

The Wazuh Dashboard provides the SOC analyst with visibility into:

- Security events
- Alerts
- Threat Hunting
- Agent status
- Security Configuration Assessment
- Detection results

## Telemetry Flow

The general telemetry flow is:
Windows Security Activity
          |
          v
Windows Event Logs
          |
          v
Wazuh Agent
          |
          v
Wazuh Manager
          |
          v
Wazuh Indexer
          |
          v
Wazuh Dashboard
          |
          v
SOC Analyst

## Detection Validation Process

The project follows a controlled validation process:
Generate Test Activity
          |
          v
Verify Event on Windows
          |
          v
Check Wazuh Telemetry
          |
          v
Search Wazuh Threat Hunting
          |
          v
Classify Result
     /           \
    /             \
Detected        Blind Spot

## Telemetry Gap Analysis

The architecture also supports comparison between endpoint visibility and SIEM visibility.

For example:
Windows Event Viewer
        |
        | Event Exists
        v
Wazuh Agent
        |
        | Collection
        v
Wazuh Threat Hunting
        |
        +---- Event Found = Detected
        |
        +---- Event Missing = Potential Blind Spot
###During the project, this methodology identified a Windows Firewall telemetry blind spot.

##Security Configuration Assessment

Wazuh SCA was used alongside detection testing to evaluate Windows security configuration.

The project therefore combines two areas:
Endpoint Security
       |
       +------------------+
       |                  |
       v                  v
Configuration         Detection
Assessment            Coverage
       |                  |
       v                  v
CIS / SCA             Threat Hunting
       |                  |
       +--------+---------+
                |
                v
          SOC Assessment

##Summary

The architecture provides a practical SOC laboratory for:

* Endpoint monitoring
* Security telemetry collection
* SIEM analysis
* Detection validation
* Security hardening
* Telemetry gap identification
* SOC documentation












