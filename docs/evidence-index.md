# Evidence Index

This index maps each screenshot to the question it answers during the investigation.

| File | Evidence Value |
| --- | --- |
| `01-windows-ipconfig.png` | Windows target IP configuration |
| `02-sysmon-download.png` | Sysmon source download evidence |
| `03-sysmon-install-config.png` | Sysmon configuration was installed or updated |
| `04-sysmon-event-validation.png` | Sysmon events were generated and readable |
| `05-baseline-tasklist.png` | Baseline Windows process command output |
| `06-baseline-get-service.png` | Baseline PowerShell command output |
| `07-powershell-test-activity.png` | Test activity used to generate telemetry |
| `08-sysmon-eid1-whoami.png` | Sysmon Event ID `1` process creation for `whoami` |
| `09-sysmon-eid1-powershell.png` | Sysmon Event ID `1` process creation for PowerShell |
| `10-sysmon-eid3-network.png` | Sysmon Event ID `3` network connection telemetry |
| `11-nmap-smb-scan-results.png` | Nmap scan results against Windows target |
| `12-wireshark-syn-scan.png` | Wireshark SYN scan evidence |
| `13-netexec-smb-bruteforce.png` | NetExec SMB password guessing output |
| `14-security-event-query.png` | Windows Security log query evidence |
| `15-eid4624-successful-logon.png` | Successful logon event evidence |
| `16-eid4624-source-ip-details.png` | Event fields showing source IP and logon details |
| `17-wireshark-smb-ntlm-traffic.png` | Wireshark SMB and NTLMSSP traffic evidence |

## Naming Standard

Screenshots use a two-digit sequence number followed by a short description:

```text
NN-short-description.png
```

This keeps the evidence in investigation order and makes Markdown references predictable.
