# Detection Engineering: SMB Brute Force

## Detection Goal

Identify SMB password guessing against Windows accounts by detecting repeated failed logons followed by a successful network logon from the same source.

## Required Data Sources

| Source | Event ID | Purpose |
| --- | --- | --- |
| Windows Security Log | `4625` | Failed logon attempts |
| Windows Security Log | `4624` | Successful logons |
| Sysmon | `1` | Process creation context |
| Sysmon | `3` | Network connection telemetry |
| Network capture | SMB / NTLMSSP | Packet-level validation |

## Detection Logic

Alert when:

- Five or more Event ID `4625` failures occur within 60 seconds.
- The failures target the same account or originate from the same source IP.
- A successful Event ID `4624` Logon Type `3` follows from the same source IP.

## Analyst Query Examples

### Failed Logons

```powershell
Get-WinEvent -FilterHashtable @{LogName='Security'; Id=4625} -MaxEvents 25 |
  Select-Object TimeCreated, Id, ProviderName, Message
```

### Successful Network Logons

```powershell
Get-WinEvent -FilterHashtable @{LogName='Security'; Id=4624} -MaxEvents 25 |
  Where-Object { $_.Message -match 'Logon Type:\s+3' } |
  Select-Object TimeCreated, Id, ProviderName, Message
```

### SMB Traffic in Wireshark

```text
tcp.port == 445 && ip.addr == 192.168.56.3
```

## High-Value Fields

| Field | Why It Matters |
| --- | --- |
| Account Name | Identifies the attacked or compromised account |
| Source Network Address | Identifies the remote system |
| Logon Type | Type `3` confirms a network logon |
| Authentication Package | NTLM is commonly observed with SMB authentication |
| Workstation Name | May identify the attacking host |

## Severity Guidance

| Condition | Severity |
| --- | --- |
| Multiple `4625` failures only | Medium |
| `4625` failures followed by `4624` success | High |
| Success against privileged account | Critical |
| Source IP outside expected admin ranges | High |

## Triage Workflow

1. Confirm the burst of `4625` failures.
2. Check whether the same source IP produced a `4624` success.
3. Confirm Logon Type `3`.
4. Identify the target account and whether it is privileged.
5. Review SMB traffic or firewall logs for TCP `445`.
6. Reset the account password if compromise is confirmed.
7. Review other systems for the same source IP or account.

## False Positive Considerations

| Scenario | Triage Note |
| --- | --- |
| User forgot password | Failures usually come from a known workstation and do not include broad scanning |
| Stale mapped drive | Failures repeat over a longer period and may not be followed by a success |
| Service account password changed | Failures usually target one service account from expected hosts |
| Vulnerability scanner | May scan SMB but typically does not complete a valid account login |

## Recommended Alert Title

`Possible SMB Brute Force Followed by Successful Network Logon`
