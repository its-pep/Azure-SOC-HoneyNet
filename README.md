# Azure-SOC-HoneyNet
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
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

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
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/J0dmk6z.png)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/WENny48.png)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/ZtNyNVh.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-04-19 21:32:58
Stop Time  2023-04-20 21:32:58

| Metric                               | Count
| -------------------------------------| -----
| SecurityEvent (Windows VM)           | 26214
| Syslog (Linux VM)                    | 3374
| SecurityAlert (Microsoft Defender for Cloud) | 0
| SecurityIncident (Sentinel Incidents)| 219
| NSG Inbound Malicious Flows Allowed             | 736

## Security Posture Before Hardening
![Security Posture Before](https://i.imgur.com/uc1DiZf.png)

## Hardening Steps
The initial 24-hour study revealed that the lab was vulnerable to multiple threats due to its visibility on the public internet. To address these findings, I activated NIST SP 800-53 r4 within the compliance section of Microsoft Defender and focused on fulfilling the compliance standards associated with SC.7.*. Additional assessments for SC-7 - Boundary Protection.

![SC.7](https://i.imgur.com/YkzdaTr.png)


## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-04-22 13:34:06
Stop Time	 2023-04-23 13:34:06

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent (Windows VM)| 450
| Syslog (Linux VM)         | 24
| SecurityAlert (Microsoft Defender for Cloud)| 0
| SecurityIncident (Sentinel Incidents)| 0
| NSG Inbound Malicious Flows Allowed| 0

## Security Posture After Hardening
![Security Posture After Hardening](https://i.imgur.com/9tX9ViJ.png)



## Overall Improvement

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent (Windows VM)| -98.28%
| Syslog (Linux VM)         | -99.29%
| SecurityAlert (Microsoft Defender for Cloud)| -100%
| SecurityIncident (Sentinel Incidents)| -100%
| NSG Inbound Malicious Flows Allowed|-100%

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastially reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
