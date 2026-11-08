# Windows Failed Logon Correlation and Custom Brute-Force Detection Using Wazuh
## Project Overview

This project focuses on investigating repeated failed login attempts on a Windows 11 system using Wazuh SIEM.

A test user account was created, and multiple incorrect password attempts were deliberately generated to produce Windows Security Event ID 4625 (Failed Logon).

The events were monitored and analysed in Wazuh. A custom detection rule was then developed to identify five failed logon attempts involving the same target account within 60 seconds.

The objective was to demonstrate how a SOC analyst can investigate authentication failures, correlate related security events, and develop custom detection rules to identify potentially suspicious login activity.
## Objectives

The objectives of this practical were to:

1. Investigate Windows failed logon events (Event ID 4625) using Windows Event Viewer.
2. Monitor and analyse failed authentication events using Wazuh SIEM.
3. Examine existing Wazuh detection rules and understand their correlation logic.
4. Develop a custom Wazuh rule to detect repeated failed logon attempts against the same account.
5. Configure the rule to trigger after five failed attempts within 60 seconds.
6. Validate the custom rule and test its detection capability through controlled login attempts.
7. Analyse the resulting alerts and determine how event correlation can support security monitoring and investigation.
8. ## Tools and Technologies Used

The following tools and technologies were used during the investigation:

| Tool / Technology | Purpose |
|---|---|
| Windows 11 Pro | Host operating system used to generate and investigate failed logon events. |
| Wazuh SIEM (v4.14.9) | Security monitoring, event analysis, alert generation and custom rule detection. |
| Docker Desktop | Running the Wazuh manager, indexer and dashboard containers. |
| Windows Event Viewer | Examining Windows Security Event ID 4625 records. |
| PowerShell | Executing commands, managing the lab environment and interacting with Docker containers. |
| XML | Writing and configuring the custom Wazuh detection rule. |
## Lab Environment and Setup

The investigation was conducted in a controlled local laboratory environment.

The setup consisted of a Windows 11 Pro host machine running Docker Desktop, with Wazuh deployed through Docker containers.

The Wazuh deployment consisted of three main components:

| Component | Function |
|---|---|
| Wazuh Manager | Receives and analyses security events and applies detection rules. |
| Wazuh Indexer | Stores and indexes security event data for searching and analysis. |
| Wazuh Dashboard | Provides the interface for monitoring alerts and investigating security events. |

A Windows endpoint was connected to Wazuh to collect and forward Windows security events.

The test environment was configured to generate controlled failed authentication attempts, allowing the events to be monitored and investigated through the Wazuh dashboard.

All testing was performed within this isolated learning environment.
