# Building a SOC + Honeynet in Azure (Live Traffic)

<img width="1275" height="716" alt="image" src="https://github.com/user-attachments/assets/8d41e972-90de-41ed-ae72-ec70b43377b9" />

## Introduction

In this project, I built a mini honeynet in Azure and ingested log sources from various resources into a Log Analytics workspace, which was then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured security metrics in the insecure environment for 24 hours, applied security controls to harden the environment, and then measured metrics for another 24 hours. The metrics I will present are:

- SecurityEvent (Suspicious Windows Security Event logs)
  - Windows RDP / SMB Auth Failed Attempts
- SQL Server Auth Failed Attempts
- Syslog (Linux Event Logs)
  -  Linux SSH Auth Failed Attempts
- SecurityIncident (Incidents created by Sentinel)
- NTANetAnalytics (Malicious Flows allowed into the honeynet)

## Architecture Before Hardening / Security Controls
<img width="1140" height="630" alt="image" src="https://github.com/user-attachments/assets/2edebbc1-3da5-460c-a478-32ba9bed8d40" />

## Architecture After Hardening / Security Controls
<img width="1271" height="716" alt="image" src="https://github.com/user-attachments/assets/9b4bb2fb-70ef-4fab-8d41-7c13c5858381" />

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (1 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed and exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources were deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint.

## Attack Maps Before Hardening / Security Controls
<img width="898" height="464" alt="(before)-windows-rdp-auth-fail" src="https://github.com/user-attachments/assets/2261c023-6c7b-432b-ae3e-c794a08a7b2f" />
<img width="714" height="464" alt="(before)-mssql-auth-fail" src="https://github.com/user-attachments/assets/4e9648c5-c210-4aea-ae57-2954901da9cc" />
<img width="1092" height="475" alt="(before)-linux-ssh-auth-fail" src="https://github.com/user-attachments/assets/5b4e924a-8f81-4ddb-9a1a-c6e9cbe54751" />
<img width="1093" height="480" alt="(before)-nsg-malicious-allowed-in" src="https://github.com/user-attachments/assets/2988638b-0f5b-4312-9be5-ca103e5fc278" />


## Metrics Before Hardening / Security Controls

The following table shows the metrics I measured in my insecure environment for 24 hours:
Start Time 2025-10-09 13:20:20
Stop Time 2025-10-10 13:20:20

| Metric                    | Count
| ------------------------- | -----
| SecurityEvent             | 726
| ↳ Windows RDP Failed Auth | 422 
| SQL Server Failed Auth    | 400
| Syslog                    | 18434
| ↳ Linux SSH Failed Auth   | 4945
| SecurityIncident          | 251
| NTANetAnalytics           | 1481

## Attack Maps After Hardening / Security Controls

```All map queries returned no results due to zero instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics I measured in my environment for another 24 hours, but after I applied security controls:
Start Time 2025-10-23 19:30:10
Stop Time	2025-10-24 19:30:10

| Metric                   | Count | Total Reduction 
| ------------------------ | ----- | ---------------
| SecurityEvent            | 171   | -76.4%
 ↳ Windows RDP Failed Auth | 0     | -100%
| SQL Server Failed Auth   | 0     | -100%
| Syslog                   | 12    | -99.9%
| ↳ Linux SSH Failed Auth  | 0     | -100%
| SecurityIncident         | 0     | -100%
| NTANetAnalytics          | 0     | -100%

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. The number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
