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

Additional investigation case studies and practical projects will be added as my portfolio develops.

## Lab Environment

My practical learning uses cybersecurity labs, simulated security incidents, and tools appropriate to my available lab environment.

**Note:** Simulated investigations are clearly identified as lab exercises. Their findings do not represent confirmed incidents involving real organizations or systems.

## Professional Goal

I am working toward becoming a job-ready SOC Analyst through structured learning, practical investigation, evidence-based analysis, and continuous improvement.


