# Investigation 04: Linux SSH Authentication and Privilege Activity Investigation

**Investigation Type:** Linux Authentication and Privilege Activity Analysis  
**Environment:** Linux Workstation  
**Primary Evidence:** Authentication Logs, Process Logs and Network Logs  
**Protocol of Interest:** SSH (TCP Port 22)  
**Status:** Completed

## 1. Project Overview

This investigation examines suspicious authentication and privilege-related activities observed on a Linux workstation.

The investigation focuses on repeated failed login attempts, a subsequent successful login to the administrative account, sudo privilege enumeration, unsuccessful root login attempts, and access to authentication logs.

The purpose was to determine whether the observed activities represented normal administrative behaviour or potentially unauthorized access requiring further investigation.

The investigation demonstrates how a SOC analyst can use Linux authentication, process and network logs to investigate suspicious login patterns and assess potential security risks.

## 2. Investigation Objectives

The objectives of this investigation were to:

- Examine Linux authentication logs for suspicious login activities.
- Identify repeated failed SSH login attempts.
- Investigate successful authentication following multiple failed attempts.
- Examine subsequent sudo and privilege-related activities.
- Identify unsuccessful authentication attempts against the root account.
- Analyse network activity associated with SSH connections.
- Determine whether the observed activities were consistent with possible brute-force or password-guessing behaviour.
- Establish an appropriate security classification based on available evidence.

## 3. Investigation Environment and Tools

The investigation was conducted in a Linux environment using system logs and network activity records as the primary evidence sources.

### Evidence Sources

| Evidence Source | Investigation Purpose |
|---|---|
| Authentication Logs | Examine failed and successful login attempts |
| Process Logs | Investigate sudo and other process-related activities |
| Network Logs | Examine network connections and repeated SSH connection attempts |

### Important Network Information

| Field | Observed Value |
|---|---|
| Source IP Address | `192.168.1.50` |
| Destination Port | `22` |
| Protocol | SSH |
| Target Account | Administrative account and root account |

## 4. Investigation Scenario

A Linux workstation exhibited unusual authentication activity involving multiple failed login attempts followed by a successful login to an administrative account.

Subsequent activities included sudo privilege enumeration, filesystem inspection, failed root authentication attempts and access to authentication logs.

A SOC analyst was required to examine the available authentication, process and network evidence to determine whether the sequence of activities was consistent with potential unauthorized access.

## 5. Investigation Process

The investigation involved reviewing three primary categories of evidence.

### 5.1 Authentication Log Analysis

Authentication logs were examined to identify unsuccessful and successful login attempts.

The review identified:

- Multiple failed login attempts occurring within a short period.
- A subsequent successful login to the administrative account.
- Two unsuccessful login attempts against the root account.
- Access to the authentication log after the preceding login activities.

The sequence of failed and successful authentication attempts warranted further investigation.

### 5.2 Process Activity Analysis

Process-related evidence was examined to understand the activities performed following the successful administrative login.

The observed activities included:

- Sudo privilege enumeration.
- Filesystem inspection.
- Access to authentication logs.

These activities may occur during legitimate system administration, but in the context of repeated failed login attempts and the subsequent successful login, they increased the security concern.

### 5.3 Network Activity Analysis

Network evidence was reviewed to investigate repeated connection attempts involving TCP port `22`.

The activity showed repeated targeting of SSH, the protocol commonly used for remote Linux administration.

The network observations were considered alongside the authentication logs to assess whether the connection pattern was consistent with possible SSH password-guessing activity.

## 6. Key Investigation Findings

The investigation identified the following activities:

1. Multiple failed login attempts occurred within a short period.
2. A successful login to the administrative account followed the failed attempts.
3. Sudo privilege enumeration was subsequently observed.
4. Filesystem inspection activities were performed.
5. Two unsuccessful login attempts against the root account were identified.
6. The authentication log was subsequently accessed.
7. The network evidence showed repeated activity involving TCP port `22`.
8. The observed activities were associated with the source IP address `192.168.1.50`.

The combination of repeated authentication failures, a subsequent successful administrative login and additional privilege-related activities raised concerns about possible unauthorized access.

## 7. Evidence and Analysis

The investigation was supported by authentication, process and network activity evidence.

### Evidence 1: Authentication Log

The authentication log provides evidence of the observed login activities, including repeated failed authentication attempts, a subsequent successful administrative login and unsuccessful root login attempts.

![Linux Authentication Log](evidence/01-authentication-log.png)

**Analysis:**

The sequence of authentication failures followed by a successful administrative login is noteworthy and may be consistent with password-guessing activity.

However, the available evidence alone does not establish whether the successful login was performed by an unauthorized individual.

### Evidence 2: Process Activity Log

The process evidence supports the examination of activities performed following the successful administrative login.

![Linux Process Log](evidence/02-process-log.png)

**Analysis:**

The observed sudo privilege enumeration and filesystem inspection indicate that administrative or system-related activities were performed after authentication.

When considered alongside the preceding failed login attempts, these activities warrant further investigation into the user and process responsible.

### Evidence 3: Network Activity Log

The network evidence provides additional context concerning the repeated connection activity involving SSH over TCP port `22`.

![Linux Network Log](evidence/03-network-log.png)

**Analysis:**

The repeated SSH-related network activity is consistent with the authentication activity observed in the system logs.

Correlating the network records with authentication timestamps and source addresses helps establish whether the activities are related.

## 8. Analysis and Security Assessment

The observed activity followed a sequence involving repeated authentication failures, a successful administrative login and subsequent privilege-related activities.

The sequence can be summarized as follows:

```text
Multiple Failed Login Attempts
              |
              v
Successful Administrative Login
              |
              v
Sudo Privilege Enumeration
              |
              v
Filesystem Inspection
              |
              v
Failed Root Login Attempts
              |
              v
Authentication Log Access
```

This sequence is consistent with a possible SSH brute-force or password-guessing attempt followed by administrative activity.

The successful administrative login following repeated failures increases the security concern, particularly when considered alongside the subsequent sudo and authentication-log activities.

However, sudo enumeration and filesystem inspection can also occur during legitimate administration.

Therefore, the findings should be assessed collectively rather than treating any individual activity as proof of compromise.

## 9. Investigation Classification

**Observed Behaviour:** Suspicious SSH Authentication and Privilege-Related Activity

**Security Classification:** Suspicious — Requires Further Investigation

**Potential Threat:** SSH Brute-Force or Password-Guessing Activity

The available evidence supports a suspicious authentication pattern involving repeated failed login attempts followed by a successful administrative login.

The subsequent privilege-related activities increase the possibility of unauthorized access.

However, the investigation does not independently establish the identity of the person responsible or conclusively prove that the administrative account was compromised.

Further investigation would be required to determine the legitimacy of the successful login and the activities that followed.

## 10. Business and Security Risks

If similar activity occurred on a production Linux workstation, potential risks could include:

- Unauthorized access to administrative accounts.
- Exposure of confidential organisational information.
- Unauthorized modification of system files.
- Privilege abuse and further access to protected resources.
- Lateral movement to other systems within the network.
- Disruption of normal business operations.
- Potential loss of system integrity and confidentiality.

## 11. Recommended SOC Response

If this activity were identified in a production environment, the following actions would be recommended:

1. Investigate the identity and legitimacy of the source IP address.
2. Identify the user account associated with the successful administrative login.
3. Review authentication timestamps and correlate them with network and process records.
4. Determine whether the successful login was authorized.
5. Investigate the administrative account for possible unauthorized use.
6. Review sudo activity and relevant command execution records.
7. Examine additional authentication and system logs for related suspicious activity.
8. Consider temporarily restricting SSH access or isolating the affected workstation where unauthorized access is suspected.
9. Reset affected credentials and review privileged account access if compromise is confirmed.
10. Continue monitoring for repeated authentication failures and suspicious administrative activities.

## 12. Investigation Limitations

The investigation was based on the available authentication, process and network evidence.

Although the logs provided useful information about the sequence of activities, some aspects could not be independently established.

The investigation could not conclusively determine:

- The identity of the individual responsible for the login attempts.
- Whether the successful administrative login was authorized.
- Whether the administrative account was compromised.
- The exact intention behind the sudo and filesystem inspection activities.
- Whether additional unauthorized actions occurred outside the available logs.
- Whether the observed source IP address represented the original device responsible for the activity.

Additional endpoint investigation, user verification and further log correlation would be required to establish the full impact of the activity.

## 13. Investigation Conclusion

The investigation identified suspicious authentication and privilege-related activity on a Linux workstation.

The observed sequence included multiple failed login attempts, a subsequent successful administrative login, sudo privilege enumeration, filesystem inspection, unsuccessful root login attempts and access to authentication logs.

The network evidence also showed repeated activity involving SSH over TCP port `22` from the source IP address `192.168.1.50`.

The combination of these activities is consistent with a possible brute-force or password-guessing attempt followed by potentially unauthorized administrative activity.

Based on the available evidence, the activity was classified as **Suspicious — Requires Further Investigation**.

However, the investigation did not conclusively establish successful account compromise or malicious intent.

The findings highlight the importance of correlating Linux authentication logs, process activities and network evidence when investigating potential security incidents.

## 14. Skills Demonstrated

- Linux authentication log analysis.
- SSH security monitoring.
- Identification of repeated failed login attempts.
- Investigation of successful and unsuccessful authentication events.
- Analysis of sudo and privilege-related activities.
- Linux process and network log investigation.
- Authentication event correlation.
- Identification of potential brute-force activity.
- Evidence-based security assessment.
- SOC incident documentation and reporting.

## 15. Disclaimer

This investigation was conducted for educational and cybersecurity skill-development purposes.

The findings and classification are based on the available authentication, process and network evidence. The investigation does not represent a confirmed real-world security breach or independently establish successful account compromise.
