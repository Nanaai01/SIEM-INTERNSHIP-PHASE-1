DETECTION USE CASE: BRUTEFORCE DETECTION

SCENARIO DESCRIPTION
The threat to be simulated is a bruteforce attack. This would aid SOC Analyst how to detect, identify and respond to brutforce attacks

OBJECTIVE
-The SIEM should detect a failed login attempt

TOOLS USED
- SIEM: Splunk Instance and Splunk Forwarder
- Log Sources: Windows Event Logs and Sysmon
- Attack Simulation: Kali Linux

EVENT ID/ DATA MAPPING
| Source       | Event ID | Description            |
| ------------ | -------- | ---------------------- |
| Windows Logs | 4625     | Failed login attempt   |
| Sysmon       | 1        | Process execution etc. |

DETECTION LOGIC
```spl
index="wineventlog" source="WinEventLog:Security"
```

Result
Failed Login Attempt

Screenshots
(Stored in screenshots folder)
