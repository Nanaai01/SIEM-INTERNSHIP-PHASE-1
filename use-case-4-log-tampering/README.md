🧼 Log Tampering via Security Audit Log Clearing

📘 Overview

Log tampering is a common anti-forensic technique used by attackers to cover their tracks after gaining access to a system. One of the most direct ways is clearing the Windows Security Event Log, which is a red flag in both defensive and incident response contexts.

This report focuses on simulating and detecting **audit log clearing** using the `wevtutil` command in Windows and monitoring its footprint via Splunk.

---

## 🧪 Scenario

* 🔹 The attacker has valid credentials or admin access on the target Windows system.

* 🔹 They execute the following command to erase the **Security Event Log**:

  ```bash
  wevtutil cl Security
  ```

* 🔹 This triggers **Event ID 1102**, indicating the **audit log was cleared**.



## 🛠️ Lab Setup

* **Attacker Machine**: Kali Linux (for lateral movement and remote command execution)
* **Victim Machine**: Windows 11 VM
* **Log Collector**: Splunk Enterprise with Universal Forwarder on the victim machine



## 🧾 MITRE ATT\&CK Mapping

* **T1070.001** – [Indicator Removal on Host: Clear Windows Event Logs](https://attack.mitre.org/techniques/T1070/001/)



🔍 Detection Logic

### Splunk Query – Detect Log Clearing (Event ID 1102)

```spl
index=wineventlog EventCode=1102
| table _time, ComputerName, Account_Name, Message
| sort - _time
```

* **EventCode 1102** = Security log cleared.
* Always investigate the **Account\_Name** and **ComputerName**.



Extended Hunting Queries

1️⃣ Timeline of Key Security Events

```spl
index=wineventlog EventCode IN (1102,4624,4688)
| timechart span=30m count by EventCode
```

 2️⃣ Log Gaps (Sudden Drop in Events)

```spl
index=wineventlog source="WinEventLog:Security"
| timechart span=1h count
```

> Watch out for **sudden silence** or **count drops**.

---

📦 Test Simulation

1. On the victim Windows VM (with admin access), run:

   ```cmd
   wevtutil cl Security
   ```

2. In Splunk, query for:

   ```spl
   index=wineventlog EventCode=1102
   ```

3. You should observe the exact time, user, and host responsible for the log clearance.



🛡️ Recommendations

* ✳️ **Alert** on `EventCode=1102` in high-integrity environments.
* 🚫 Restrict unnecessary admin privileges.
* 🔁 Enable log forwarding to SIEM or external log storage.
* 📌 Monitor for `1102` paired with lack of expected events (e.g., no `4624`, `4688`, etc.).



📊 Summary Table

| Behavior             | Detection Event | Tool Used     | MITRE ID  |
| -------------------- | --------------- | ------------- | --------- |
| Security Log Cleared | 1102            | `wevtutil cl` | T1070.001 |


