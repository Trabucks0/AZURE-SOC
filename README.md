# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://exemplarysecurity.com/wp-content/uploads/2023/04/Before-Main-1024x754.png)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://exemplarysecurity.com/wp-content/uploads/2023/04/After-1-1024x956.png)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
![Screenshot 2024-04-25 220354](https://github.com/Trabucks0/AZURE-SOC/assets/146896610/b4e81af4-f35f-4878-984d-15e5ae9ce9e8)
![Linux Syslog Auth Failures](https://i.imgur.com/G1YgZt6.png)<br>
![Windows RDP/SMB Auth Failures](https://github.com/Trabucks0/AZURE-SOC/assets/146896610/e80cf73d-6a79-4077-a861-d7b6601cd7b1">
)<br>


## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-03-10T21:03:08.1360519Z
Stop Time 2023-03-11T21:03:08.1360519Z

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 4358
| Syslog                   | 2345
| SecurityAlert            | 6
| SecurityIncident         | 73
| AzureNetworkAnalytics_CL | 103

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 5/14/2024 19:25:51
Stop Time	5/15/2024 19:25:51

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 21438
| Syslog                   | 5
| SecurityAlert            | 0
| SecurityIncident         | 30
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
