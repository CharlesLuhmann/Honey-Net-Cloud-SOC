# Building a SOC + Honeynet in Azure
![Cloud Honeynet / SOC](https://i.imgur.com/qAeYVJT.png)

## Introduction

In this project, I deployed a mini honeynet in Azure and configured it to ingest log data from multiple sources into a Log Analytics workspace. This workspace was integrated with Microsoft Sentinel to facilitate advanced threat detection, correlation, and incident response. To evaluate the effectiveness of various security controls, I first measured a baseline of security metrics in an intentionally insecure environment over a 24-hour period. After implementing security hardening measures, I conducted the same measurements over an additional 24-hour period to compare the impact of these controls. The key metrics analyzed include:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/CHI9xab.png)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/EH1Ymqp.png)

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
### NSG Allowed Inbound Malicious Flows
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/O2xOyR3.png)<br>
### Linux Syslog Auth Failures
![Linux Syslog Auth Failures](https://i.imgur.com/6p34ilG.png)<br>
### Windows RDP/SMB Auth Failures
![Windows RDP/SMB Auth Failures](https://i.imgur.com/690SqJj.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-10-14 02:54:17
Stop Time 2024-10-15 02:54:17

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 101524
| Syslog                   | 4344
| SecurityAlert            | 33
| SecurityIncident         | 378
| AzureNetworkAnalytics_CL | 4349

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-10-16 03:53:40
Stop Time	2024-10-17 03:53:40

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 18248
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

> [!NOTE]
> Within 24 hours of implementing the security controls, there was an 82.03% reduction in security events on our Windows VMs, a 99.98% decrease in security events on our Linux systems, and a 100% reduction in incidents detected by Microsoft Defender for Cloud, Sentinel, and Network Security Group (NSG) inbound malicious flows.

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.


It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
