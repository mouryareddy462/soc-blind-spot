# Firewall Telemetry Blind Spot

This evidence documents the observed Windows Firewall telemetry gap.

## Test

Windows Firewall events were generated locally and verified in Windows Event Viewer.

Observed event IDs:
- 2097 — Firewall rule added
- 2052 — Firewall rule deleted

## Wazuh Result

The Firewall event channel was configured in the Wazuh agent, but the corresponding Firewall events were not observed in Wazuh Threat Hunting during the test.

## Finding

**BLIND SPOT OBSERVED**

Windows Firewall telemetry was available locally but was not observed in the Wazuh detection view.

The exact root cause was not fully isolated and may involve event ingestion, filtering, decoding, or rule handling.

> Any simulated Firewall evidence in this repository is explicitly labeled as simulated and is not presented as real Wazuh evidence.
