# Building a SOC + Honeynet in Azure (Live Traffic)
![Creating a Live Honeynet with SOC in Azure](https://github.com/CyberJCP/Cyber-SOC/assets/142761911/37fcb473-a4f9-470e-bbcd-b95bf2df5e2f)

## Introduction

In this particular project, I constructed a compact honeynet within the Azure framework. The primary objective was to aggregate log data from diverse sources into a centralized Log Analytics workspace. Microsoft Sentinel was then harnessed to leverage this data for constructing comprehensive attack maps, initiating alert mechanisms, and establishing incident records. The project involved a phased approach, commencing with the measurement of security metrics within an initially unsecured environment over a span of 24 hours. Following this, a series of security measures were implemented to fortify the environment. Subsequently, an additional 24-hour metric assessment phase was conducted to capture the post-hardening results. The ensuing sections will show the precise metrics that are under scrutiny:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls

Before the implementation of security controls and the fortification process, the initial architecture within my Azure platform encompassed several crucial components. These included a configuration comprising blob storage, a key vault, both Windows and Linux virtual machines, and limited network security groups configured with lenient access permissions for each virtual machine. This design was characterized by a more open configuration with all resources exposed to the internet, which laid the groundwork for subsequent security enhancements.

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel
- 
## Architecture After Hardening / Security Controls

In the context of the "AFTER" metrics phase, a comprehensive set of security measures was enacted. This involved the fortification of Network Security Groups through the implementation of stringent access policy that permitted traffic exclusively from my designated administrative workstation as well as internally within the Azure environment. Additionally, a layer of safeguarding was instituted for other resources through the activation of their inherent firewalls and the establishment of Private Endpoints. In parallel, a dedicated virtual network and subnet were established to serve as a secure enclave for the devices, significantly optimizing the efficacy of the security framework.

## Attack Maps Before Hardening / Security Controls
![NSG Malicious Flows - Before](https://github.com/CyberJCP/Cyber-SOC/assets/142761911/f3b088d3-033f-482b-a8fd-23517441f252)
![Linux Syslog Auth Fail - Before](https://github.com/CyberJCP/Cyber-SOC/assets/142761911/9beda8dc-d499-48c8-b00c-aac0cb97edce)
![Windows RDP Auth Fail - After](https://github.com/CyberJCP/Cyber-SOC/assets/142761911/dc92b59e-6927-4ce6-9ac8-fb25dac557ee)
![SQL Auth Fail - Before](https://github.com/CyberJCP/Cyber-SOC/assets/142761911/5e7e7b60-cf91-4644-b557-1c20d8fee716)

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-07-30 00:08:43
Stop Time 2023-03-16 00:08:43

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 116576
| Syslog                   | 867
| SecurityAlert            | 7
| SecurityIncident         | 194
| AzureNetworkAnalytics_CL | 991

## Attack Maps After Hardening / Security Controls
![NSG Malicious Flows - After](https://github.com/CyberJCP/Cyber-SOC/assets/142761911/44628901-ec54-47f0-9075-589570c8db14)
![Windows RDP Auth Fail - After](https://github.com/CyberJCP/Cyber-SOC/assets/142761911/8c6c4b38-2d92-4b32-b09c-cf087a34dc5e)

```Linux Syslog Auth Fail & SQL Auth Fail map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-08-14 04:00:58
Stop Time	2023-08-15 04:00:58

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 10483
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

The creation of a compact honeynet within the Microsoft Azure ecosystem was realized, accompanied by the seamless integration of log sources into a dedicated Log Analytics workspace. The strategic utilization of Microsoft Sentinel facilitated the dynamic initiation of alerts and the orchestration of incidents, driven by the insightful analysis of ingested logs. The project methodology encompassed the initial quantification of metrics within an unsecured environment, followed by the application of security controls, and a subsequent reevaluation of metrics post-implementation. Notably, the results underscore the substantial reduction, over 90%, in security events and incidents following the comprehensive deployment of security measures, showcasing their remarkable efficacy.

It is pertinent to acknowledge that in scenarios where network resources experience significant utilization by regular users, the post-security control phase might yield an increased generation of security events and alerts within the initial 24-hour interval. This nuanced consideration highlights the interplay between security measures and operational context, thereby enriching the broader comprehension of the project's outcomes.
