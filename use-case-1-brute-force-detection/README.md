Detection Use Case: Bruteforce Detection

Scenario Description
The threat to be simulated is a bruteforce attack. This would aid SOC Analyst how to detect, identify and respond to brutforce attacks

Objective
The SIEM should detect a failed login attempt

Tools Used
- SIEM: Splunk Instance and Splunk Forwarder
- Log Sources: Windows Event Logs and Sysmon
- Attack Simulation: Kali Linux

Event ID / Data Mapping
| Source       | Event ID | Description            |
| ------------ | -------- | ---------------------- |
| Windows Logs | 4625     | Failed login attempt   |
| Sysmon       | 1        | Process execution etc. |

Detection Logic
```spl
index="wineventlog" source="WinEventLog:Security"
```

Result
Failed Login Attempt

Screenshots
(Stored in screenshots/ folder)
```



![Failed login 1](https://github.com/user-attachments/assets/dd7b3093-09f0-49f4-86c8-d94cc3deb7be)![Failed login 5](https://github.com/user-attachments/assets/05e0c129-40a8-4ef0-8b0c-77c6e230c15f)
![Failed login 4](https://github.com/user-attachments/assets/55e4bf60-74c6-4e9f-98a6-569bcd10ab70)
![Failed login 3](https://github.com/user-attachments/assets/7b113b84-58b6-4d51-8ba1-99d5b8cf8394)
![Failed login 2](https://github.com/user-attachments/assets/4ff63acd-20dc-4501-94fc-879c394b84e9)


