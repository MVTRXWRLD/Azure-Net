# Building a SOC + Honeynet in Azure (Live Traffic)
![HONEYNET](https://github.com/MVTRXWRLD/Azure-Net/assets/153859869/67ff3e34-2d09-43dd-87e0-2438a9633dd6)



## Introduction

In this project, I set up a mini honeynet on Azure. I collected log data from different sources into a Log Analytics workspace. Microsoft Sentinel used this workspace to track attacks, send alerts, and manage incidents. First, I checked security measures for 24 hours in the vulnerable setup. Then, I added more security features, rechecked for another 24 hours, and now I'll show the results. The metrics we'll cover are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

The mini honeynet architecture in Azure includes the following parts:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

Before checking the metrics, all resources were set up to be accessed from the Internet. The Virtual Machines had unrestricted access through their Network Security Groups and built-in firewalls, and all other resources were accessible via public endpoints. This made Private Endpoints unnecessary.

After assessing the metrics, I strengthened the Network Security Groups by blocking ALL traffic except for my admin workstation. Additionally, all other resources were safeguarded by their built-in firewalls and Private Endpoint protection.

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/1qvswSX.png)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/G1YgZt6.png)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/ESr9Dlv.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics I measured in my insecure environment for 24 hours:
Start Time 2024-01-25 4:12:08
Stop Time 2024-01-26 4:12:08

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 19470
| Syslog                   | 3028
| SecurityAlert            | 10
| SecurityIncident         | 348
| AzureNetworkAnalytics_CL | 843

## Attack Maps Before Hardening / Security Controls


None of the map queries showed any results because there were no instances of malicious activity during the 24-hour period after strengthening security measures.

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in the environment for another 24 hours, but after I have applied security controls:
Start Time 2024-02-05 2:48
Stop Time	2024-02-06 2:48

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8778
| Syslog                   | 25
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.
6
It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
