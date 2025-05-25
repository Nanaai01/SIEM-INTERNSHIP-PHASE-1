🕵️‍♂️ Hidden User Account Creation (Backdoor Accounts)

 Overview

Attackers often create hidden or backdoor user accounts on compromised systems to maintain persistent access and evade detection. This technique involves adding new user accounts and elevating their privileges by adding them to administrative groups.



🧰 Tools & Commands

* Windows built-in commands:

  * `net user <username> <password> /add`
  * `net localgroup administrators <username> /add`
* Remote execution via Impacket’s `psexec.py` from Kali Linux:

  ```bash
  python3 /usr/share/doc/python3-impacket/examples/psexec.py Domain name/user:Password@192.168.189.150 "net user backdooruser SuperSecurePass123! /add"
  python3 /usr/share/doc/python3-impacket/examples/psexec.py Domain name/user:Password@192.168.189.150 "net localgroup administrators backdooruser /add"
  ```



🔍 MITRE ATT\&CK Reference

* T1136.001** — Create Account: Local Accounts



🛠️ Detection Logic (Windows Event IDs)

| Event ID | Description                                                                 |
| -------- | --------------------------------------------------------------------------- |
| 4720     | A user account was created                                                  |
| 4732     | A member was added to a security-enabled local group (e.g., administrators) |



🧪 Test Scenario

1. Use `psexec.py` from Kali to remotely create a new user on the Windows victim.
2. Add the created user to the local administrators group.
3. Verify creation and privilege escalation through Windows Security Event Logs.



📊 Splunk Queries for Detection

1. Detect New User Account Creation (4720)

```spl
index=wineventlog EventCode=4720
| table _time, ComputerName, Target_Username, Subject_Username, Message
| sort - _time
```

2. Detect User Added to Administrators Group (4732)

```spl
index=wineventlog EventCode=4732
| table _time, ComputerName, MemberName, Subject_Username, Message
| sort - _time
```

3. Combined User and Group Changes

```spl
index=wineventlog (EventCode=4720 OR EventCode=4732)
| table _time, EventCode, ComputerName, Target_Username, MemberName, Subject_Username, Message
| sort - _time
```

---

📋 Data Required

* Windows Security Event Logs forwarded to Splunk (`index=wineventlog`)
* Proper logging configuration on Windows hosts



✅ Summary

Creating hidden or backdoor users is a stealthy persistence technique. Monitoring Windows Security Event IDs **4720** and **4732** can help detect suspicious user account creations and privilege escalations. Proper alerting and investigation can reduce the attacker’s foothold and limit lateral movement.



⚠️ Recommendations

* Restrict and monitor user creation and group membership changes
* Enforce strong password policies and multi-factor authentication
* Regularly audit local administrator group memberships
* Set up alerts on unusual account activity in Splunk or SIEM

