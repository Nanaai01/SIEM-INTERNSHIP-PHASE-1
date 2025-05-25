🛡️ Lateral Movement via Remote Desktop Protocol (RDP) and PsExec

📌 Overview

Lateral movement is a post-exploitation technique where attackers move from a compromised host to other systems within the network to expand their access. This document outlines two primary methods of lateral movement: via Remote Desktop Protocol (RDP) and PsExec, using a realistic lab setup.



🧪 Lab Setup

* Attacker Machine: Kali Linux
* Victim Machine: Windows 11 VM 
* onitoring Host: Splunk Enterprise on a separate host to capture and analyze Windows Event Logs

🎯 Goal

Detect successful RDP-based lateral movement attempts using Windows Security Event Logs, focusing on Event ID 4624 with Logon Type 10.

📥 Log Source

* Index: `wineventlog`
* Source: `WinEventLog:Security`

🧰 Technique 1: Remote Desktop Protocol (RDP)

Tools Used

* `xfreerdp` (Kali)
* Windows RDP service (Victim)

MITRE ATT\&CK Reference

* T1021.001** - Remote Services: Remote Desktop Protocol

🔍 Detection Logic

```spl
index=wineventlog EventCode=4624 Logon_Type=10
```

* `EventCode=4624` indicates successful logon.
* `Logon_Type=10` specifies an RDP logon.

Test Scenario

1. From Kali:

```bash
xfreerdp /v:192.168.189.150 /u:Dummy /p:Password12345 /cert:ignore /sec:rdp
```

2. ✅ Authenticate successfully to Windows via RDP.

 Splunk Query

```spl
index=wineventlog source="WinEventLog:Security" EventCode=4624 Logon_Type=10
| table _time, ComputerName, Account_Name, Ip_Address, Logon_Type
| sort - _time
```



🧰 Technique 2: PsExec (SMB Remote Service Execution)

Tools Used

* `psexec.py` from Impacket (Kali)

MITRE ATT\&CK Reference

* T1021.002 - Remote Services: SMB/Windows Admin Shares

🔍 Detection Logic

* 4624: Successful logon (Logon Type 3 or 5)
* 7045: A service was installed on the system

🧰 Test Scenario

1. From Kali:

```bash
python3 /usr/share/doc/python3-impacket/examples/psexec.py domain name/user:Password@192.168.189.150
```

2. Observe the following:

   * Connection to the ADMIN\$ share
   * Upload of a random executable
   * Creation and start of a temporary service

Splunk Queries

Successful Remote Logon

```spl
index=wineventlog EventCode=4624
| search Logon_Type=3 OR Logon_Type=5
| table _time, ComputerName, Account_Name, Logon_Type, Ip_Address
| sort - _time
```

#### Service Creation

```spl
index=wineventlog EventCode=7045
| table _time, ComputerName, Message
| sort - _time
```


Data Required for Detection

* Windows Event Logs (Security & System)
* Splunk Universal Forwarder on the Windows host
* Proper indexing in Splunk (e.g., `index=wineventlog`)


Summary

This test demonstrates how attackers use RDP and PsExec to move laterally within a network. With proper monitoring, such activity can be detected using Event IDs 4624, 7045, and contextual logon types.

| Technique | Tool      | Event IDs             | MITRE ID  |
| --------- | --------- | --------------------- | --------- |
| RDP       | xfreerdp  | 4624 (Type 10)        | T1021.001 |
| PsExec    | psexec.py | 4624 (Type 3/5), 7045 | T1021.002 |



🧠 Recommendations

* Restrict RDP and admin share access with firewalls
* Enable logging of service installations (7045)
* Monitor for unusual logon types and service names
* Alert on execution from ADMIN\$ share or remote service creation

