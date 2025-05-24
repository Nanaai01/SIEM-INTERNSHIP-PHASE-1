📄Brute-force Detection Lab



🧠 Detection Logic

```spl
index="wineventlog" source="WinEventLog:Security" EventCode=4625
| stats count by Account_Name, host, src_ip, _time
| where count > 5
| sort - count
```



🖊️ Write-up

🔍 Detection Use Case: Brute-force Detection

🎯 Scenario Description

This use case simulates a brute-force attack using valid domain enumeration but repeated invalid credentials to trigger failed login events. The goal is to help SOC analysts detect abnormal authentication patterns and take appropriate action.




🧪 Objective

Detect excessive failed login attempts that may signal a brute-force attack, using Splunk and Windows Event Logs.




🧰 Tools Used

| Tool               | Purpose                                     |
| ------------------ | ------------------------------------------- |
| Kali Linux         | Attack simulation                           |
| Windows Event Logs | Log source for authentication failures      |
| Sysmon             | Supplementary process visibility (optional) |
| Splunk Enterprise  | SIEM for detection and log correlation      |
| Splunk Forwarder   | Collects logs from the Windows system       |




📋 Event ID / Data Mapping

| Source      | Event ID | Description          |
| ----------- | -------- | -------------------- |
| WinEventLog | 4625     | Failed login attempt |
| Sysmon      | 1        | Process execution    |



🔥 Attack Simulation

The following command was run on **Kali Linux** to simulate **domain-based SMB authentication attempts** using `smbclient`:

```bash
smbclient -L //192.168.189.150 -U 'CYBERSTAR\Star%Password123' -m SMB2
```

This attempts to enumerate SMB shares on the victim Windows machine using domain credentials. If the credentials are incorrect or access is denied, **Windows logs Event ID 4625**, indicating a failed login.

By repeatedly attempting authentication with different passwords or usernames, this becomes a brute-force simulation.


🔎 Detection in Splunk

The detection query identifies accounts or IPs with more than 5 failed login attempts. This logic helps in surfacing brute-force attempts that occur over short periods.

SOC analysts can build alerts around this logic to trigger real-time notifications or triage responses.



✅ Result

Splunk successfully detected multiple failed login attempts using domain account `CYBERSTAR\Star`. These were flagged in the Security Event Log as EventCode 4625 and visualized in the search UI.

This confirms the effectiveness of the detection logic in identifying brute-force behavior based on authentication failure patterns.



