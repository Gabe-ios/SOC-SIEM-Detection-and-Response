# LIVE SOC/SIEM Detection and Response Lab

This repository accompanies the **LIVE SOC/SIEM Detection and Response Lab** developed in an Azure cloud environment. It captures real-time attack simulations, log ingestion, alerting, and incident response workflows using Windows 11 VMs, Log Analytics, and Microsoft Sentinel.

To kick off the new senior school year, we built a mini-honeynet that processes live event logs and feeds them into a SIEM solution. The network remained intentionally exposed for two separate 24-hour sessions.

Check out the lab’s GitHub for ongoing enhancements including custom security rules, network expansion, and threat hunting tactics:  
**GitHub:** https://github.com/Gabe-ios/SOC-SIEM-Detection-and-Response

---

##  Architecture Overview

The lab environment consists of:

- **Virtual Network**  
- **Network Security Group (NSG)**  
- **Two Windows 11 Virtual Machines**  
- **Log Analytics Workspace**  
- **Azure Storage Account**  
- **Microsoft Sentinel (SIEM)**

---

## Live Attack (Before Hardening)

- The network was configured without any firewall or NSG restrictions, allowing free access.
- Windows Firewall was disabled on each VM; NSG allowed **all inbound traffic**.
- The lab ran for 24-hour intervals to record activity and simulate attack vectors.

### Metrics Monitored:

- Failed login attempt map via Azure Workbooks  
- Event logs forwarded for alert generation

### Observables:

- **Failed Login Attempt Map** — visualized via Azure Workbooks (JSON available on GitHub)
- **24-hour Live Attack Graph** — timestamped from **3:00 pm CST (Sept 4, 2025)** to **3:00 pm CST (Sept 5, 2025)**  


- **Incident Response Example:**
  - **Incident 1** — triggered by an “Excessive Windows Logon Failure” alert. Analyzed using a KQL query, then labeled as **False Positive** and closed.  
  :contentReference[oaicite:1]{index=1}
  - **Incident 9** — a real brute-force attack simulated with 41 failed login attempts across multiple usernames.

#### Example KQL Query Used:
```kql
SecurityEvent
| where EventID == 4625
| project TimeGenerated, Account, IpAddress, Computer, LogonType, FailureReason
| summarize Attempts = count() by IpAddress
| order by Attempts desc
