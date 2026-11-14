# Technical Incident Summary & IoCs

## SOC Ticket Closure Notes
* **Alert Title:** `net.exe service stopped on BR1-WKS-001` (Alert `#5111`)
* **Attack Category:** Network Intrusion / Lateral Movement
* **Verdict:** **True Positive**
* **Summary:** At `02:59:52`, compromised credentials for `employee44` were used to initiate a remote logon (`Event ID 4624`) from workstation `WS-3914` (`10.20.164.124`). At `03:02:52`, the threat actor escalated privileges to `NT AUTHORITY\SYSTEM` on `BR1-WKS-001` and executed `net.exe stop winDefend`, triggering Windows Defender `Event ID 5001` and disabling real-time protection.
* **Containment:** Isolated `BR1-WKS-001` and `WS-3914` from the network and forced an immediate credential reset for `employee44`.

---

## Extracted Indicators of Compromise (IoCs)

| Artifact Type | Value | Context / Role |
| :--- | :--- | :--- |
| **IP Address** | `10.20.164.124` | Internal RFC 1918 IP address assigned to pivot workstation `WS-3914`. |
| **Hostnames** | `WS-3914`, `BR1-WKS-001` | `WS-3914` (Source/Pivot Host); `BR1-WKS-001` (Target Host with disabled Defender). |
| **Usernames** | `employee44`, `System` | `employee44` (Compromised user account); `System` (`NT AUTHORITY\SYSTEM` execution context). |
| **Process** | `net.exe` | Native Windows Living-off-the-Land binary abused to stop security services. |
| **Command Line** | `net stop winDefend` | Exact command executed to terminate Microsoft Defender real-time protection. |
