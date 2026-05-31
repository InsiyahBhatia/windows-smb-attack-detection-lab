# Remediation Plan

## Immediate Actions

| Action | Reason |
| --- | --- |
| Reset the compromised account password | The account `user1` was successfully authenticated |
| Disable or remove weak lab accounts | Prevent reuse of known weak credentials |
| Review recent logons from `192.168.56.3` | Confirm whether additional access occurred |
| Preserve Security logs and packet captures | Retain evidence for the investigation record |

## Hardening Recommendations

| Control | Recommendation |
| --- | --- |
| Account lockout | Lock accounts after a small number of failed attempts, such as five failures |
| Password policy | Require longer passwords and block common passwords |
| SMB exposure | Restrict TCP `445` to trusted systems only |
| SMB signing | Enable SMB signing where operationally feasible |
| Local accounts | Avoid shared local accounts and use unique administrative credentials |
| Monitoring | Alert on repeated `4625` failures followed by `4624` success |

## Suggested Detection Alert

Alert name:

```text
Possible SMB Dictionary Attack Followed by Successful Network Logon
```

Core condition:

```text
Multiple Windows Security Event ID 4625 failures from one source IP
followed by Event ID 4624 Logon Type 3 for the same account or host.
```

## Lessons Learned

- Windows Security logs are the strongest source for authentication outcomes.
- Sysmon adds useful endpoint context but does not replace Security log auditing for logon events.
- Wireshark is valuable for confirming protocol behavior when validating lab detections.
- Weak local passwords are easy to exploit when SMB is reachable.
