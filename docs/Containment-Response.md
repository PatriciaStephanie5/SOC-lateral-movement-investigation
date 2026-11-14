# Containment & Response Playbook

## Immediate Containment Actions Executed
1. **Network Isolation:** Isolated both **`BR1-WKS-001`** (target host) and **`WS-3914`** (patient zero pivot host) at the endpoint/switch level to sever active attacker sessions and prevent lateral spread toward core server subnets (`10.0.10.x`).
2. **Credential Invalidation:** Disabled active sessions and enforced an immediate password reset for **`employee44`**, revoking any active Kerberos TGTs or NTLM session tokens.
3. **Defense Restoration:** Re-enabled the `WinDefend` service and real-time protection on `BR1-WKS-001` via elevated PowerShell:
   ```powershell
   Set-MpPreference -DisableRealtimeMonitoring \$false
   Start-Service WinDefend
   ```

---

## Post-Containment Forensic Triage Checklist
Before returning `WS-3914` and `BR1-WKS-001` to production, complete the following checks:

* **Persistence & Service Audits:** Query `System` logs (`Event ID 7045`) on `BR1-WKS-001` for newly installed services (such as `PSEXESVC` or randomized 8-character binaries) created between `02:59:52` and containment.
* **Post-Blind Payload Hunting:** Inspect `C:\Windows\Temp\`, `C:\Users\Public\`, and `employee44`'s `AppData` directories for dropped scripts (`.ps1`, `.bat`, `.vbs`) or executables placed after `03:02:52`.
* **Registry Tampering Verification:** Check `HKLM\SOFTWARE\Policies\Microsoft\Windows Defender` to confirm no persistent `DisableAntiSpyware` registry keys were written.
* **Patient Zero Root Cause:** Analyze email logs and browser download history on `WS-3914` prior to `02:59:52` to identify and purge the initial phishing payload (`T1566`).
