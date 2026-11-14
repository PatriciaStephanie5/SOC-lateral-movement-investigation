# Lessons Learned & Hardening Recommendations

## Root Cause Analysis
1. **Tamper Protection Gap:** A `SYSTEM`-level process was able to terminate `WinDefend` using `net.exe`, indicating Microsoft Defender Tamper Protection was either disabled or not enforced via policy.
2. **Unrestricted Lateral Movement:** Workstation `WS-3914` (`10.20.164.124`) was permitted to initiate administrative remote connections directly to `BR1-WKS-001` across internal network segments.
3. **Privilege Escalation Exposure:** Compromise of a standard user identity (`employee44`) rapidly led to `NT AUTHORITY\SYSTEM` execution within three minutes, pointing to excessive local privileges or unpatched local escalation vectors.

---

## Long-Term Security Hardening

| Control Area | Recommendation | Mitigated ATT&CK Technique |
| :--- | :--- | :--- |
| **Endpoint Protection** | **Enforce Defender Tamper Protection** across all Windows endpoints via Intune or Group Policy so even `SYSTEM` accounts cannot stop `WinDefend` via `net.exe`, PowerShell, or registry edits. | `T1562.001` (Impair Defenses) |
| **Network Architecture** | **Implement Micro-Segmentation** on internal firewalls/switches to block peer-to-peer workstation traffic (SMB port 445, WinRM ports 5985/5986, RDP port 3389) across subnets. | `T1021` (Remote Services) |
| **Privileged Access** | **Deploy Windows LAPS** (Local Administrator Password Solution) and remove standard users from local Administrator groups to eliminate shared local admin credential reuse. | `T1078` (Valid Accounts) |
| **Identity & Phishing** | **Enforce Phishing-Resistant MFA** and tighten email attachment/link sandboxing to prevent credential harvesting at the initial entry point. | `T1566` (Phishing) |
