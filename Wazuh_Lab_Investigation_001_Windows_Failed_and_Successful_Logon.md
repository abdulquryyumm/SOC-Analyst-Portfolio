# Wazuh Lab Investigation 001: Windows Failed Logon Followed by Successful Logon

## 1. Investigation summary

| Item | Details |
|---|---|
| Investigation ID | WAZUH-LAB-001 |
| Investigation type | Windows authentication monitoring |
| Environment | Windows 11 endpoint monitored by Wazuh Agent |
| Wazuh version | 4.14.9 |
| Agent name | `MAIN-WINDOWS-HOST` |
| Agent ID | `001` |
| Account involved | `Ayoola` |
| Initial event | Windows Security Event ID `4625` — failed logon |
| Related event | Windows Security Event ID `4624` — successful logon |
| Assessment | Likely benign / consistent with expected user activity |

### Executive summary

A Windows Security Event ID `4625` recorded a failed interactive logon for the account `Ayoola`. The event showed the substatus `0xc000006a`, which indicates an incorrect password. A successful interactive logon (Event ID `4624`) for the same account was then found approximately 9.734 seconds later.

The failed and successful events shared several attributes, including the account name, workstation, logon type, event source address, process name, and process ID. The user subsequently confirmed that they were attempting to sign in to the computer at the relevant time.

Based on the evidence reviewed, the activity is consistent with a legitimate user entering an incorrect password and then successfully signing in. The available evidence does not establish malicious activity.

## 2. Objective

Determine whether the failed Windows logon was an isolated authentication mistake or part of a suspicious sequence, and establish whether a successful logon followed it.

**Exercise type:** This was a retrospective investigation of events already present in the local Windows Security log and Wazuh. We did not deliberately generate the original events as part of this exercise. A later practical will deliberately generate a controlled event so we can observe the complete event pipeline.

## 3. Environment and data sources

- **Endpoint:** `DESKTOP-J68IN4P` (Windows 11)
- **Monitoring platform:** Wazuh 4.14.9, local single-node Docker deployment
- **Wazuh Agent:** `MAIN-WINDOWS-HOST`, Agent ID `001`
- **Windows data source:** Windows Security event channel
- **Investigation interfaces:** Windows Event Viewer and Wazuh Dashboard

The Windows Agent configuration included the following collection block:

```xml
<localfile>
    <location>Security</location>
    <log_format>eventchannel</log_format>
    <query>Event/System[EventID != 5145 and EventID != 5156 and EventID != 5447 and
      EventID != 4656 and EventID != 4658 and EventID != 4663 and EventID != 4660 and
      EventID != 4670 and EventID != 4690 and EventID != 4703 and EventID != 4907 and
      EventID != 5152 and EventID != 5157]</query>
</localfile>
```

**Configuration finding:** The Agent was configured to collect the Windows `Security` event channel using `eventchannel`. Event ID `4625` was not listed among the excluded event IDs in this filter.

Windows Event Viewer also contained Event ID `4625` records, and a corresponding failed-logon alert was found in Wazuh. This provided practical evidence that this type of Windows Security event was reaching the SIEM.

## 4. Investigation timeline

The timestamps below are those displayed in the Wazuh Dashboard. The timezone setting was not independently documented during this exercise.

| Time shown in Wazuh | Event | Account | Result |
|---|---:|---|---|
| Oct 27, 2026 @ 21:04:44.256 | `4625` | `Ayoola` | Failed logon |
| Oct 27, 2026 @ 21:04:53.990 | `4624` | `Ayoola` | Successful logon |

**Elapsed time:** approximately 9.734 seconds between the two displayed timestamps.

## 5. Event 4625 — failed logon analysis

The failed-logon event contained these relevant fields:

| Field | Value | Interpretation |
|---|---|---|
| `data.win.system.eventID` | `4625` | Failed logon |
| `data.win.eventdata.targetUserName` | `Ayoola` | Account presented in the logon attempt |
| `data.win.eventdata.logonType` | `2` | Interactive logon |
| `data.win.eventdata.ipAddress` | `127.0.0.1` | Loopback address recorded in the event |
| `data.win.eventdata.workstationName` | `DESKTOP-J68IN4P` | Workstation recorded in the event |
| `data.win.eventdata.subStatus` | `0xc000006a` | Incorrect password |
| `data.win.eventdata.processName` | `C:\Windows\System32\svchost.exe` | Process associated with the logon attempt |
| `data.win.eventdata.processId` | `0x790` (decimal `1936`) | Process identifier recorded in the event |
| Wazuh rule ID | `60122` | Rule that generated the Wazuh alert |
| Wazuh rule level | `5` | Alert severity level assigned by this rule |

### Interpretation

The event records an interactive logon attempt for `Ayoola` on `DESKTOP-J68IN4P`. The substatus `0xc000006a` indicates that the password was incorrect. The event's source address was `127.0.0.1`; in this context, it is a loopback address and does not indicate a remote external source.

The process name was `svchost.exe`, a legitimate Windows executable used to host Windows services. Its presence alone does not establish that the activity was malicious or benign.

## 6. Process correlation attempt

The Event ID `4625` recorded process ID `0x790` (decimal `1936`) and process name `svchost.exe`.

An attempt was made to locate a Windows Security Event ID `4688` whose **New Process ID** matched `0x790`, in order to identify the process-creation record and inspect its creator process. No matching Event ID `4688` was found in the date range searched (October 27, 2026).

### What this means

- The process-creation record was **not available in the records searched**.
- The parent/creator process for this `svchost.exe` instance was therefore **not established**.
- The absence of a matching Event ID `4688` does not prove that the process was never created. Possible explanations include process-creation auditing not being enabled at the relevant time, log retention limits, or the process having been created outside the searched period.
- No parent process should be inferred without supporting evidence.

This is recorded as an investigative limitation, not as evidence of malicious activity.

## 7. Event 4624 — successful logon analysis

A successful-logon event was found for the same account after the failed attempt.

| Field | Value | Interpretation |
|---|---|---|
| `data.win.system.eventID` | `4624` | Successful logon |
| `data.win.eventdata.targetUserName` | `Ayoola` | Account successfully logged on |
| `data.win.eventdata.logonType` | `2` | Interactive logon |
| `data.win.eventdata.ipAddress` | `127.0.0.1` | Loopback address recorded in the event |
| `data.win.eventdata.workstationName` | `DESKTOP-J68IN4P` | Workstation recorded in the event |
| `data.win.eventdata.processName` | `C:\Windows\System32\svchost.exe` | Process associated with the logon event |
| `data.win.eventdata.processId` | `0x790` | Process identifier recorded in the event |
| `data.win.eventdata.authenticationPackageName` | `Negotiate` | Authentication package reported by Windows |
| `data.win.eventdata.logonProcessName` | `User32` | Logon process reported by Windows |
| Wazuh rule ID | `60118` | Rule that generated the success alert |
| Wazuh rule level | `3` | Alert severity level assigned by this rule |

### Interpretation

The successful event occurred approximately 9.734 seconds after the failed event and shared the same account, workstation, logon type, event source address, process name, and process ID.

These matching attributes support correlating the two authentication records. Because the events occurred close together on the same endpoint, the matching PID is consistent with the same process instance being involved in both records. A PID match should still be interpreted with timestamps and host identity because Windows can reuse PIDs.

The Wazuh rule's MITRE ATT&CK mapping to Valid Accounts is a classification associated with the rule; it is not, by itself, proof that an attacker used a compromised account.

## 8. Repeated-attempt check

A search for Event ID `4625` associated with `Ayoola` returned one failed-logon event in the results reviewed.

No repeated failed-login pattern was identified in the records examined. This reduces support for a password-guessing or brute-force interpretation, but does not independently prove that no other attempts occurred outside the searched data or time range.

## 9. User validation

The user confirmed that they were attempting to log in to the Windows computer at approximately the time of the events.

This contextual confirmation is consistent with the observed sequence: one failed interactive logon followed shortly by a successful interactive logon for the same account.

## 10. Assessment and conclusion

**Disposition: Likely benign — consistent with expected user activity.**

The available evidence supports the following sequence:

1. Windows recorded one failed interactive logon for `Ayoola` (Event ID `4625`).
2. The failure substatus indicated an incorrect password (`0xc000006a`).
3. Approximately 9.734 seconds later, Windows recorded a successful interactive logon for `Ayoola` (Event ID `4624`).
4. The two events shared several identifying attributes, including workstation, logon type, source address, process name, and process ID.
5. The user confirmed that they were attempting to sign in at the relevant time.

The evidence is consistent with the user entering an incorrect password and then successfully signing in. No evidence of repeated password guessing or unauthorized remote access was identified in the events reviewed.

This is a reasoned assessment based on the available telemetry and user confirmation, not a guarantee that all possible malicious activity has been ruled out.

## 11. Evidence limitations

- No matching Event ID `4688` process-creation record for New Process ID `0x790` was found in the date range searched, so the creator/parent process was not established.
- The investigation relied on the events available in the local Windows Security log and Wazuh alerts reviewed during the exercise.
- The timezone configuration for the displayed Wazuh timestamps was not independently recorded.
- The source address `127.0.0.1` is a loopback address; it should not be interpreted as the address of an external remote host.
- The search result count describes the records returned within the search scope, not necessarily every authentication event that may have existed outside that scope.

## 12. Lessons learned

1. An alert is an investigation starting point, not automatic proof of an attack.
2. An Active Wazuh Agent does not, by itself, prove that every desired event source is configured and being collected.
3. Windows Event ID `4625` records failed logons; Event ID `4624` records successful logons.
4. Logon type, account, workstation, source address, failure substatus, process name, PID, and timestamp provide useful context for authentication investigations.
5. The Agent IP and the event's `data.win.eventdata.ipAddress` are different fields and should not be confused.
6. Event correlation should use multiple attributes and a consistent timeline, not a PID match alone.
7. When supporting evidence is unavailable, document the limitation rather than inventing a conclusion.
8. User context can help distinguish expected activity from suspicious activity, but it should be considered alongside technical evidence.

## 13. Suggested evidence to attach

Add screenshots or exported records from the lab, where available:

- Windows Agent `ossec.conf` showing the Security event-channel collection block.
- Windows Event Viewer showing the Event ID `4625` record.
- Wazuh Dashboard showing the Event ID `4625` alert and its relevant fields.
- Wazuh Dashboard showing the Event ID `4624` alert and its relevant fields.
- The search showing one Event ID `4625` result for `Ayoola` in the reviewed scope.
- The Event ID `4688` search result or filter showing that no matching New Process ID was found in the searched date range.

**Handling note:** Redact personal account names, SIDs, host identifiers, internal addresses, and other identifying details before making screenshots or records public. Use only evidence you are comfortable publishing.
