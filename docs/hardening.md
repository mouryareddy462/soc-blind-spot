# Windows Security Hardening

## Overview

CIS-aligned Windows security controls were reviewed and selected controls were hardened using Windows configuration commands and registry settings.

The purpose of the hardening activity was to improve endpoint security and measure the resulting Security Configuration Assessment (SCA) status in Wazuh.

## Password Policy Hardening

The following password controls were configured:

- Minimum password length: 14 characters
- Password history: 24 previous passwords
- Minimum password age: 1 day
- Maximum password age: 365 days

These settings were configured using Windows account policy commands.

## Account Lockout Hardening

The following account lockout controls were configured:

- Lockout threshold: 5 failed attempts
- Lockout duration: 15 minutes
- Lockout observation window: 15 minutes

These controls help reduce the risk of repeated password-guessing attempts.

## Account Security

Selected account-security controls were configured, including:

- Microsoft account restrictions
- Guest account disabled
- Built-in Administrator account renamed to `SOC-ADMIN`

The Administrator account rename was validated using the account SID ending in `-500`.

## Audit and Security Policy Hardening

Selected Windows audit and security policy settings were configured, including:

- Force audit policy subcategory settings
- Logon-related security settings
- Process creation auditing

Process Creation auditing was enabled to support detection of Windows Security Event ID 4688.

## Network Security Hardening

Selected Windows network-security registry settings were configured, including:

- Anonymous access restrictions
- Disable domain credential caching
- Disable ICMP redirects
- Disable multicast settings
- Insecure guest authentication restrictions
- Automatic network connection restrictions

## Printer Security

The Windows setting controlling installation of printer drivers by users was configured according to the selected CIS control.

## Cached Logon Settings

Cached interactive logon settings were reviewed and configured with a reduced cached-logon count.

## Security Configuration Assessment

Wazuh Security Configuration Assessment (SCA) was used to evaluate the endpoint after hardening.

Latest documented result:

| SCA Metric | Result |
|---|---:|
| Passed | 145 |
| Failed | 328 |
| Not Applicable | 9 |
| Compliance Score | 30% |

The SCA score represents the security configuration assessment and should not be confused with the project's detection coverage metric.

## Hardening Validation

After configuration changes:

1. Windows settings were verified locally.
2. The Wazuh agent was restarted where required.
3. Wazuh SCA was refreshed.
4. Individual controls were checked for updated pass/fail status.
5. Detection testing continued through Wazuh Threat Hunting.

Several controls changed from failed to passed after the corresponding configuration was applied.

## Important Limitation

The endpoint used in this project was Windows 11 Home, while the selected CIS benchmark is designed for Windows 11 Enterprise.

Therefore, not every benchmark control is directly applicable to the lab endpoint.

The hardening work should be interpreted as a practical CIS-aligned security improvement exercise rather than a claim of complete CIS compliance.

## Conclusion

The hardening phase demonstrated how endpoint configuration changes can be validated through Wazuh SCA.

The process also showed that security hardening and detection coverage are related but separate aspects of SOC engineering.

**Configuration hardening improves the endpoint's security posture, while detection testing verifies whether security activity remains visible to the SOC.**
