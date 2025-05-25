🛡️ Use Case: Lateral Movement via RDP

📌 Overview

Lateral movement using Remote Desktop Protocol (RDP) is a common post-exploitation technique used by attackers to expand their presence across systems after initial access. By leveraging legitimate credentials, adversaries can log in to other systems in the network using RDP and perform further actions while evading detection.

This detection logic focuses on identifying successful RDP logins (Logon Type 10) using Windows Event Logs.



🧰 Techniques

* Tactic: Lateral Movement
* Technique: Remote Desktop Protocol (RDP)
* MITRE ATT\&CK ID: [T1021.001 - Remote Services: Remote Desktop Protocol](https://attack.mitre.org/techniques/T1021/001/)



🧪 Simulation Tools

* [`xfreerdp`](https://www.freerdp.com/)
* [`rdesktop`](https://linux.die.net/man/1/rdesktop)
* Windows built-in **Remote Desktop Connection (mstsc.exe)**



🔍 Detection Logic

🎯 Goal

Detect successful RDP-based lateral movement attempts using Windows Security Event Logs, focusing on Event ID 4624 with Logon Type 10.

📥 Log Source

* Index: `wineventlog`
* Source: `WinEventLog:Security`

📄 SPL Query

```spl
index="wineventlog" source="WinEventLog:Security" EventCode=4624 Logon_Type=10
| table _time, ComputerName, Account_Name, Logon_Type, Ip_Address, Message
| sort - _time
```



📘 Interpretation

* Event ID: `4624` – Successful logon
* Logon Type: `10` – Remote Interactive (i.e., RDP)
* Account\_Name: User account used to log in
* Ip\_Address: Source IP of the RDP connection



⚠️ Detection Use Cases

| Use Case                       | Description                                                  |
| ------------------------------ | ------------------------------------------------------------ |
| RDP from unusual IP            | Detect RDP logins from IPs outside expected subnets            |
| RDP using non-admin accounts   | Flag non-privileged accounts performing RDP logins             |
| RDP outside business hours     | Trigger alerts for logons during suspicious times (e.g., 2 AM) |
| First-time RDP                 | Alert when a user logs in to a new host for the first time     |



🧪 How to Simulate

1. **Enable RDP** on a Windows VM
2. From Kali:

   ```bash
   xfreerdp3 /v:<target_ip> /u:<username> /p:<password> /cert:ignore /sec:rdp
   ```
3. Confirm log generation using Splunk query above

> ✅ Successful RDP login generates a `4624` with `Logon_Type=10`



📎 Related Event IDs

| Event ID | Description               |
| -------- | ------------------------- |
| `4624`   | Successful logon          |
| `4625`   | Failed logon              |
| `4778`   | RDP session reconnection  |
| `4779`   | RDP session disconnection |

---

🧠 Recommendations

* Enable Windows firewall logging to monitor incoming RDP traffic
* Restrict RDP access to trusted IPs only
* Enforce multi-factor authentication for remote access
* Monitor for account usage anomalies

