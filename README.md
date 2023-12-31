# Azure-soc
# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet +  SOC](https://github.com/cusseryjr/Azure-soc/assets/87047948/87a55e15-a62c-47dd-8aa9-7e88a766aae8)


## Introduction

In this project, I built a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![PUBLIC INTERNET](https://github.com/cusseryjr/Azure-soc/assets/87047948/bbd66fc0-8a30-46cb-800b-fa0af530dc3c)

## Architecture After Hardening / Security Controls
![Secured honeypot](https://github.com/cusseryjr/Azure-soc/assets/87047948/2aed6d54-afb9-4196-b28d-4b895d4041a9)

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
![image](https://github.com/cusseryjr/Azure-soc/assets/87047948/70d9ba74-fd9f-4bcd-9215-21a56d6e1f56)

<br>
![Linux Syslog Auth Failures]!<img width="1236" alt="syslog-ssh-auth-fail" src="https://github.com/cusseryjr/Azure-soc/assets/87047948/dc31a06e-33ff-4c77-a440-384d965bacd9">

<br>
![Windows RDP/SMB Auth Failures])<img width="1239" alt="windows-rdp-auth-fail" src="https://github.com/cusseryjr/Azure-soc/assets/87047948/8dbc375f-31f2-4598-8f8d-0ab7e620ffee">
<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-05-28 9:46:59
Stop Time 2023-05-29 9:46:59

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 88552
| Syslog                   | 1999
| SecurityAlert            | 7
| SecurityIncident         | 227
| AzureNetworkAnalytics_CL | 603

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-06-01 18:31:10
Stop Time	2023-06-02  18:31 

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 11219
| Syslog                   | 24
| SecurityAlert            | 0
| SecurityIncident         | 2
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
