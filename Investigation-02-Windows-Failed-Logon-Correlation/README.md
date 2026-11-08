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
