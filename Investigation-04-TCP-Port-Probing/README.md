# Investigation 04: TCP Port Probing Analysis
**Case ID:** C-001  
**Investigation Type:** Network Reconnaissance / TCP Port Probing  
**Environment:** Controlled Kali Linux Laboratory  
**Analysis Tool:** Wireshark  
**Status:** Completed

## 1. Project Overview

This investigation examines TCP network traffic captured using Wireshark to identify and analyze connection attempts made to multiple TCP destination ports.

The investigation focuses on sequential TCP SYN packets sent from a source workstation to different destination ports, followed by RST, ACK responses from the destination host.

The purpose was to determine whether the observed communication pattern was consistent with TCP port probing and assess its potential security significance from a SOC analyst's perspective.

This was an authorized, controlled cybersecurity laboratory exercise. The network activity was intentionally generated for training purposes and did not involve a real victim or production organization.

## 2. Investigation Objectives

The objectives of this investigation were to:

- Analyze TCP connection attempts using Wireshark.
- Identify TCP SYN packets and interpret their purpose.
- Examine RST, ACK responses from the destination host.
- Identify connection attempts targeting multiple destination ports.
- Analyze the timing and sequence of the connection attempts.
- Determine whether the observed traffic was consistent with TCP port probing.
- Distinguish suspicious-looking network behaviour from confirmed malicious activity.

## 3. Lab Environment

The investigation was conducted in a controlled virtual laboratory using Kali Linux running in VirtualBox.

### Network Configuration

| Component | Details |
|---|---|
| Source Host | `10.0.2.15` |
| Destination Host | `10.0.2.2` |
| Network Interface | `eth0` |
| Loopback Address | `127.0.0.1` |
| Default Gateway | `10.0.2.2` |
| Operating System | Kali Linux |
| Packet Analysis Tool | Wireshark |

The traffic was intentionally generated within the controlled laboratory environment for analysis and training purposes.

## 4. Investigation Scenario

A security analyst observes repeated TCP connection attempts originating from a workstation and targeting multiple ports on another host.

The connection attempts receive TCP RST, ACK responses instead of completing the normal TCP three-way handshake.

The analyst is required to investigate the traffic, identify the communication pattern, and determine whether the observed behaviour is consistent with network reconnaissance or port-scanning activity.

The investigation uses packet-level evidence to support the assessment without assuming malicious intent solely from the observed TCP traffic.

## 5. Observed Network Activity

The source host `10.0.2.15` initiated TCP SYN connection attempts to the destination host `10.0.2.2`.

The connection attempts targeted eight TCP destination ports:

- `22`
- `80`
- `443`
- `8080`
- `9001`
- `9002`
- `9003`
- `9004`

Each connection attempt received a TCP `RST, ACK` response from the destination.

No TCP application payload was observed in the captured connection attempts.

## 6. TCP Traffic Pattern and Timeline

The connection attempts occurred sequentially at approximately one-second intervals.

The following table presents the packet timestamps recorded in the Wireshark capture:

| Destination Port | Relative Capture Time |
|---:|---:|
| 22 | 0.0031 seconds |
| 80 | 1.0263 seconds |
| 443 | 2.10325 seconds |
| 8080 | 3.1283 seconds |
| 9001 | 4.14426 seconds |
| 9002 | 5.1462 seconds |
| 9003 | 6.156122 seconds |
| 9004 | 7.16484 seconds |

The first connection attempt occurred at approximately 0.0031 seconds, while the final attempt occurred at approximately 7.16484 seconds in the capture timeline.

The sequential timing pattern across multiple destination ports is consistent with a port-probing process.

## 7. TCP Response Analysis

The captured traffic followed a repeated TCP SYN → RST, ACK pattern.

The communication can be represented as follows:

```text
10.0.2.15 → 10.0.2.2:PORT    SYN
10.0.2.2  → 10.0.2.15        RST, ACK
```

### SYN Packet

The SYN flag indicates that the source host attempted to initiate a TCP connection with the destination host.

### RST, ACK Response

The RST, ACK response indicates that the destination reset or refused the connection attempt rather than completing the normal TCP handshake.

This commonly occurs when no service is listening on the targeted port, although network security controls can also generate reset responses.

### TCP Three-Way Handshake

A normal TCP connection establishment follows this sequence:

```text
Client                 Server

SYN ------------------>

     <------------------ SYN, ACK

ACK ------------------>
```

The captured probing attempts did not complete this sequence because the destination responded with RST, ACK packets instead of SYN, ACK packets.

## 8. Payload Analysis

No TCP application payload was observed in the captured connection attempts.

This indicates that the connection attempts did not progress to an application-level data exchange.

The observed traffic consisted primarily of TCP connection requests and reset responses.

The absence of application payload is consistent with connection probing, where the objective may be to determine whether particular TCP ports accept connection requests.

## 9. Investigation Findings

The investigation identified the following findings:

1. The source host `10.0.2.15` initiated TCP connection attempts towards destination host `10.0.2.2`.
2. Eight different TCP destination ports were targeted.
3. The connection attempts were initiated using TCP SYN packets.
4. Each attempt received a TCP RST, ACK response.
5. The connection attempts occurred sequentially at approximately one-second intervals.
6. The captured connection attempts did not complete the normal TCP three-way handshake.
7. No TCP application payload was observed during these attempts.
8. The overall traffic pattern was consistent with TCP port probing or scanning behaviour.

## 10. Analyst Interpretation

The repeated SYN connection attempts targeting multiple destination ports indicate that the source host was testing the availability or accessibility of different TCP ports on the destination host.

The sequential timing and repeated RST, ACK responses provide evidence of a connection-probing pattern rather than normal application-level communication.

However, this network behaviour alone does not establish malicious intent.

Similar activity can result from authorized security assessments, network diagnostics, administrative testing, or potentially malicious reconnaissance.

Because this investigation was conducted in an authorized laboratory environment, the activity was intentionally generated for cybersecurity training purposes.

The distinction between identifying port-scanning behaviour and determining whether the activity is malicious is important in SOC investigations.

## 11. Investigation Classification

**Observed Behaviour:** TCP Port Probing / Scanning

**Security Assessment:** Suspicious pattern when considered without contextual information.

**Laboratory Context:** Authorized and intentionally generated activity.

The network evidence supports the identification of TCP port probing. However, it does not establish unauthorized access, malicious intent, or compromise of the destination host.

Further investigation in a real-world environment would require additional endpoint and network evidence.

## 12. Evidence and Artifacts

The primary evidence for this investigation is the Wireshark packet capture containing the TCP connection attempts between `10.0.2.15` and `10.0.2.2`.

### Packet Capture

The original packet capture is included as an investigation artifact:

[c-001.pcapng](evidence/c-001.pcapng)

The packet capture supports the examination of the TCP SYN requests, RST, ACK responses, destination ports, and connection timing.

Supporting Wireshark screenshots will also be included in the evidence folder to illustrate the key findings discussed in this report.

## 13. Recommended SOC Investigation Actions

If similar activity were detected on a production workstation, the following actions would be recommended:

1. Identify the process responsible for generating the TCP connection attempts.
2. Determine the user account associated with the activity.
3. Review the exact timestamps and frequency of the connection attempts.
4. Examine whether similar activity was directed towards other destination hosts.
5. Review additional network and endpoint security logs for related activity.
6. Determine whether the activity was associated with authorized security testing or administrative operations.
7. Correlate the network evidence with endpoint telemetry before making a final security determination.
8. Escalate the activity for further investigation if unauthorized reconnaissance is suspected.

## 14. Investigation Limitations

This investigation was conducted in a controlled virtual laboratory environment.

The available packet capture provided visibility into TCP connection attempts and responses but did not independently identify the process or user responsible for generating the traffic.

The investigation could not establish:

- The exact process that initiated the connection attempts.
- The user account responsible for the activity.
- Whether the same activity occurred against other destination hosts.
- Whether the activity was malicious in a real-world operational environment.
- Whether any system compromise occurred.

Additional endpoint telemetry, process information, authentication records, and relevant network logs would be required to establish these details in a production environment.

## 15. Investigation Conclusion

The investigation successfully identified sequential TCP SYN connection attempts from workstation `10.0.2.15` to eight different TCP destination ports on host `10.0.2.2`.

Each connection attempt received a TCP RST, ACK response, and the captured attempts did not complete the normal TCP three-way handshake.

The connection attempts occurred at approximately one-second intervals, and no TCP application payload was observed.

These findings are consistent with TCP port probing or scanning behaviour.

Although the observed pattern could be considered suspicious in a real-world environment, the available network evidence does not establish malicious intent or unauthorized activity.

The activity was intentionally generated in an authorized laboratory environment for cybersecurity training.

This investigation demonstrates the importance of analyzing TCP flags, connection patterns, timing, and packet-level evidence before making security classifications.

## 16. Skills Demonstrated

- Network traffic analysis using Wireshark.
- TCP protocol and TCP flag interpretation.
- Identification of SYN and RST, ACK packets.
- Analysis of TCP connection failures.
- Identification of port-probing and scanning patterns.
- Packet timing and sequence analysis.
- Network reconnaissance investigation.
- Evidence-based security classification.
- SOC investigation documentation and reporting.

## 17. Disclaimer

This investigation was conducted in an authorized, controlled cybersecurity laboratory for educational and professional skill-development purposes.

The findings are based on the available packet capture and laboratory observations. They should not be interpreted as evidence of an actual cyberattack or compromise of a production system.
