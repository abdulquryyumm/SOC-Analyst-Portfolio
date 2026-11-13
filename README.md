# SOC-Analyst-Portfolio
Hands-on SOC analyst investigations, security alert analysis, incident response documentation, and cybersecurity lab projects.

## About This Portfolio

Welcome to my SOC Analyst portfolio. This repository documents my hands-on cybersecurity learning journey, focusing on Security Operations Center (SOC) analysis, security alert triage, threat detection, incident investigation, and incident response.

My goal is to develop the practical skills required to investigate security events, analyze evidence, identify suspicious activity, assess potential threats, and communicate findings clearly.

## Skills and Areas of Focus

- Security alert triage and investigation
- Log analysis and authentication monitoring
- Network traffic analysis with Wireshark
- Windows endpoint monitoring and Sysmon
- Incident response and containment concepts
- Threat hunting and detection engineering
- Investigation documentation and escalation reporting

## Investigations and Projects

### Investigation 01: Suspected Account Compromise and Malicious Email Forwarding

- **Type:** Identity security and incident investigation
- **Severity:** High (as assigned by the simulated lab)
- **Key skills:** Alert triage, authentication analysis, mailbox-rule investigation, incident-response recommendations, and evidence-based reporting
- **Environment:** Axis Security SOC Lab — simulated investigation

[Read Investigation 01](./Investigation-01-Accounnt-Compromise/README.md)

### Investigation 02: Windows Failed Logon Correlation and Custom Brute-Force Detection Using Wazuh

- **Type:** Windows authentication monitoring and detection engineering
- **Detection:** Five failed logon attempts against the same account within 60 seconds
- **Key skills:** Windows Event Log analysis, Wazuh SIEM, custom detection rules, event correlation, XML configuration, and security alert investigation
- **Environment:** Windows 11 Pro, Wazuh SIEM 4.14.9, Docker

[Read Investigation 02](Investigation-02-Windows-Failed-Logon-Correlation/README.md)

### Investigation 03: Windows Authentication Analysis

**Focus:** Windows Security Log Analysis | Event IDs 4624 and 4625

A practical investigation using Windows Event Viewer to examine successful and failed authentication activities, interpret login events, and assess whether observed behaviour indicates legitimate user activity or potential security concerns.

[View Investigation 03](./Investigation-03-Windows-Authentication-Analysis/README.md)

## Additional Practical Projects

### TCP Connection and Port-Probing Analysis

A supplementary Wireshark practical conducted in a controlled Kali Linux laboratory. It covers TCP connection attempts, SYN and RST/ACK flags, connection failures, and port-scanning patterns.

[View C-001: TCP Port Probing Investigation](https://github.com/abdulquryyumm/C-001-TCP-Port-Probing-Investigation)

### Investigation 04: Linux SSH Authentication and Privilege Activity Investigation

**Focus:** Linux Authentication Logs | SSH Monitoring | Privilege Analysis

An investigation of repeated failed SSH login attempts, successful administrative authentication, sudo activity, and suspicious privilege-related behaviour using Linux authentication, process, and network logs.

[View Investigation 04](./Investigation-04-Linux-SSH-Authentication-Analysis/README.md)

### Investigation 05: Suspicious DNS Activity Investigation

**Focus:** DNS Traffic Analysis | Wireshark | Suspicious DNS Query Detection

An investigation of unusual DNS activity involving random-looking subdomains, frequent DNS queries, and repeated resolution to the same destination IP address. The analysis uses Wireshark PCAP evidence to identify potentially suspicious automated DNS communication and assess its security implications.

[View Investigation 05](./Investigation-05-Suspicious-DNS-Activity/README.md)

### Wazuh Lab Investigation 001: Windows Failed and Successful Logon

**Focus:** Wazuh SIEM | Windows Event IDs 4624 and 4625 | Authentication Event Correlation

A retrospective investigation using Wazuh to analyse a failed Windows login followed by a successful login. The investigation examines authentication event correlation, incorrect-password indicators, process information, and user validation to determine whether the activity was consistent with legitimate user behaviour.

[View Wazuh Lab Investigation 001](./Wazuh_Lab_Investigation_001_Windows_Failed_and_Successful_Logon.md)

### Windows SOC Endpoint Monitoring Lab

**Focus:** Sysmon | Windows Endpoint Monitoring | PowerShell Investigation | Process Analysis

A practical Windows SOC laboratory covering Sysmon telemetry, suspicious PowerShell activity, parent-child process analysis, command-line investigation, and evidence-based false-positive classification.

[View Windows SOC Endpoint Monitoring Lab](https://github.com/abdulquryyumm/windows-soc-endpoint-monitoring-lab)

Additional investigation case studies and practical projects will be added as my portfolio develops.

## Lab Environment

My practical learning uses cybersecurity labs, simulated security incidents, and tools appropriate to my available lab environment.

**Note:** Simulated investigations are clearly identified as lab exercises. Their findings do not represent confirmed incidents involving real organizations or systems.

## Professional Goal

I am working toward becoming a job-ready SOC Analyst through structured learning, practical investigation, evidence-based analysis, and continuous improvement.


