# 01. Project Overview

## Purpose & Scope
This case study simulates a real-world Security Operations Center (SOC) triage and incident response workflow inside **VirtualSOC**. The objective was to investigate a critical severity detection (`Alert #5111`), validate whether the activity represented benign administrative maintenance or malicious tampering, reconstruct the attacker's lateral movement path, and execute appropriate containment runbooks.

## Environment & Network Topology
The simulated corporate network utilizes segmented RFC 1918 private IPv4 subnets:

| Asset Name | IP Address | Subnet Segment | Role in Incident |
| :--- | :--- | :--- | :--- |
| **`WS-3914`** | `10.20.164.124` | `10.20.164.0/24` (User Workstations) | **Patient Zero / Pivot Host:** Originating source of the lateral authentication request. |
| **`BR1-WKS-001`** | Internal Branch Subnet | Branch Workstation Segment | **Compromised Target Host:** Endpoint where `WinDefend` was stopped under `SYSTEM` context. |
| **`SEC-WRK-01`** | `10.0.10.15` | `10.0.10.0/24` (Security / SOC VLAN) | **SIEM Monitoring Hub:** Endpoint hosting the VirtualSOC detection engine that flagged the alert. |

## Tools & Telemetry Sources
* **SIEM Platform:** VirtualSOC Detection Engine & Log Analysis Console
* **Windows Security Event Log:** `Event ID 4624` (An account was successfully logged on)
* **Windows Defender Operational Log:** `Event ID 5001` (Microsoft Defender Antivirus real-time protection was disabled)
* **Threat Framework:** MITRE ATT&CK Enterprise Matrix & Lockheed Martin Cyber Kill Chain
