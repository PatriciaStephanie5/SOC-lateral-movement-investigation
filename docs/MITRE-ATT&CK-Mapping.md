# MITRE ATT&CK Mapping

The adversary's behavior aligns with five core techniques across the MITRE ATT&CK Enterprise framework:

| Tactic | Technique ID | Technique / Sub-Technique Name | Evidence & Mapping Rationale |
| :--- | :--- | :--- | :--- |
| **Initial Access** | `T1566` | **Phishing** | Flagged in alert telemetry as the suspected initial vector used to compromise `WS-3914` and harvest `employee44` credentials. |
| **Defense Evasion / Privilege Escalation** | `T1078` | **Valid Accounts** | Adversary authenticated across internal subnets at `02:59:52` using legitimate credentials for `employee44` (`Event ID 4624`). |
| **Lateral Movement** | `T1021` | **Remote Services** | Pivoted laterally from source workstation `WS-3914` (`10.20.164.124`) to target endpoint `BR1-WKS-001`. |
| **Execution** | `T1569.002` | **System Services: Service Execution** | Utilized native Windows service control binary `net.exe` running under `SYSTEM` privileges to manipulate OS services. |
| **Defense Evasion** | `T1562.001` | **Impair Defenses: Disable or Modify Tools** | Executed `net stop winDefend` at `03:02:52` to terminate Microsoft Defender Antivirus real-time monitoring (`Event ID 5001`). |
