📄 After-Hours Login Detection Lab




🧠 Detection Logic

```spl
index=wineventlog source="WinEventLog:Security" (EventCode=4624 OR EventCode=4625)
| eval LogonHour=strftime(_time, "%H")
| where LogonHour < 7 OR LogonHour >= 19
| table _time, EventCode, Account_Name, LogonType, LogonHour, Workstation_Name, ComputerName
| sort -_time
```

Explanation:

EventCode 4624**: Successful login.
EventCode 4625**: Failed login.
Logon Type 3**: Network logon (e.g., SMB access).
The `login_hour` is extracted from the event time, and the query filters for logins **outside business hours (09:00–19:00)**.


Objective

This lab simulates and detects **after-hours login attempts** from a potential attacker machine (Kali Linux) to a victim machine (Windows 11). The goal is to demonstrate how such unauthorized or suspicious activity can be captured and analyzed using **Splunk Enterprise**.

🧪 Lab Setup

| Component             | Description                             |
| ----------------- | ------------------------------------------- |
| Attacker (Kali)   | Used to simulate SMB login attempts.        |
| Victim (Win11)    | Logs authentication events.                 |
| Splunk Forwarder  | Installed on Windows to send logs.          |
| Splunk Enterprise | Analyzes logs and detects off-hours logins. |

🧰 Tools Used

* `smbclient` (Kali)
* Windows Event Viewer
* Splunk Universal Forwarder
* Splunk Search App

 Attack Simulation

An SMB login attempt was made from Kali to Windows:

```bash
smbclient -U testuser //192.168.189.150/C$
```

This triggered a Windows Security log:

* successful: EventCode 4624
* failed: EventCode 4625

🔍 Detection in Splunk

The search query filters for login events outside the standard business hours (09:00 to 19:00). After the attacker attempts access in the evening (e.g., 20:45), Splunk captures the log entry and flags it as an after-hours login.

✅ Outcome

Splunk successfully detected and displayed the unauthorized access attempt, showing the username, login time, and method. This validates the detection logic and demonstrates the importance of time-based access monitoring in network defense.


