# SOC Analyst Home Lab

A fully functional Security Operations Center (SOC) home lab built to simulate real-world attack detection, log correlation, and incident response.

## Architecture

![Lab Architecture](SOC_Lab_Architecture.png)

## Lab Components

| Component | Purpose | IP Address |
|-----------|---------|------------|
| Windows 11 Pro (Host) | Splunk Enterprise SIEM | 10.143.161.200 |
| Windows 11 Enterprise (VM) | Victim endpoint + Universal Forwarder + Sysmon | 10.143.161.201 |
| Ubuntu Server (VM) | Linux target | 10.143.161.205 |
| Kali Linux (VM) | Attack platform | 10.143.161.204 |

## Key Features

- **Resilient Log Forwarding:** Host-Only network (192.168.56.0/24) ensures telemetry persists independent of WAN connectivity
- **Deep Endpoint Telemetry:** Sysmon captures process creation, network connections, file changes, and LSASS access
- **Real-Time Detection:** Automated Splunk alerts for encoded PowerShell execution
- **Attack Simulation:** Mimikatz credential dumping and brute-force attacks detected in SIEM

## Detection Rules

### Encoded PowerShell Execution
Detects Base64-encoded PowerShell commands (common malware obfuscation).

```spl
index=* source="WinEventLog:Microsoft-Windows-Sysmon/Operational" 
EventCode=1 Image="*powershell.exe" CommandLine="*-enc*"

MITRE ATT&CK: T1059.001 (PowerShell)
Mimikatz Credential Dumping
Detects execution of Mimikatz and LSASS memory access.

index=* source="WinEventLog:Microsoft-Windows-Sysmon/Operational" 
EventCode=1 Image="*mimikatz*"

MITRE ATT&CK: T1003.001 (LSASS Memory)
Screenshots
Splunk Ingesting Endpoint Telemetry
01_Splunk_Receiving_Logs.png
Sysmon Process Creation Telemetry
02_Sysmon_Process_Create.png
Real-Time Alert Triggered
03_Real_Time_Alert.png
Mimikatz Detection
04_Mimikatz_Detected.png

Tools Used
| Category           | Tool                                 |
| ------------------ | ------------------------------------ |
| SIEM               | Splunk Enterprise                    |
| Endpoint Telemetry | Sysmon (Olaf Hartong modular config) |
| Log Forwarder      | Splunk Universal Forwarder           |
| Attack Simulation  | PowerShell, Mimikatz, nmap, Hydra    |
| Virtualization     | VirtualBox, UTM                      |


What I Learned

    Designing resilient network architectures with dual-homed VMs
    Configuring and tuning Windows event logging and Sysmon
    Writing Splunk SPL queries and real-time detection alerts
    Mapping detections to MITRE ATT&CK framework
    Simulating adversary behavior to validate detection coverage

Future Enhancements

    [ ] Add Ubuntu Server to Splunk for multi-OS monitoring
    [ ] Build Splunk dashboard for attack timeline visualization
    [ ] Write Sigma rules for portable detection engineering
    [ ] Integrate TheHive for case management


Built by: Emmanuel Asamoah Kwabena
LinkedIn: https://www.linkedin.com/in/emmanuel-asamoah-kwabena-b85830311
Date: August 2026
