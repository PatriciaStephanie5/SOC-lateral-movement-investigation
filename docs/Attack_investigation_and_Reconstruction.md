# Attack Investigation & Reconstruction

## Chronological Attack Timeline

By pivoting from the initial critical alert into raw Windows Event Logs, the following sequence of events was reconstructed:

| Timestamp | Log Source | Event ID | Source Host / IP | Target Host | Account Context | Observed Activity |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **02:59:52** | `WinEventLog:Security` | **4624** | `WS-3914` (`10.20.164.124`) | `BR1-WKS-001` | `employee44` | Successful remote logon authenticated using compromised domain/local credentials. |
| **02:59:52 - 03:02:52** | *3-Minute Gap* | Internal | `BR1-WKS-001` | `BR1-WKS-001` | `employee44` - `SYSTEM` | Attacker performs local reconnaissance, bypasses UAC/escalates privileges, or deploys remote execution tooling (e.g., PsExec/Impacket). |
| **03:02:52** | `Windows Defender/Operational` | **5001** | Local (`BR1-WKS-001`) | `BR1-WKS-001` | `NT AUTHORITY\SYSTEM` | Executed `net.exe` with arguments `net stop winDefend`, disabling real-time antivirus protection. |

---

## Technical Analysis

### 1. Initial Access & Credential Abuse
* The authentication log at `02:59:52` identifies `10.20.164.124` (`WS-3914`) as an internal RFC 1918 private IP address. Because private IPs have no external threat intelligence reputation, this confirmed the threat actor had already breached an internal workstation-likely via phishing (`T1566`)-and harvested valid credentials for `employee44`.

### 2. The 3-Minute Privilege Escalation Window
* Standard user accounts (`employee44`) lack the permissions required to stop core security services.
* Exactly three minutes after authentication, `net.exe` executed directly under `NT AUTHORITY\SYSTEM`. This transition indicates either a local privilege escalation / UAC bypass on `BR1-WKS-001` or remote service execution via administrative utilities such as Sysinternals PsExec or Impacket (`psexec.py` / `wmiexec.py`).

### 3. Defense Impairment (`Event ID 5001`)
* Using the Living-off-the-Land binary (`net.exe`), the adversary issued `net stop winDefend`.
* Stopping `WinDefend` triggers **Event ID 5001** in `Microsoft-Windows-Windows Defender/Operational`. Attackers routinely blind endpoint detection immediately prior to dropping credential dumpers (e.g., Mimikatz) or ransomware payloads.

### 4. SIEM Architecture Verification
* While the VirtualSOC alert header referenced `SEC-WRK-01` (`10.0.10.15`) as the monitored target asset where the SIEM agent/console resided, raw payload inspection confirmed that `BR1-WKS-001` was the actual host where `WinDefend` was stopped. `SEC-WRK-01` remained uncompromised.
