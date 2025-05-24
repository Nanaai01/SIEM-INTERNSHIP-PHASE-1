🔐 SIEM MONITORING LAB



📝 PROJECT OVERVIEW

Welcome to my SIEM Monitoring Lab!
This project demonstrates how to set up a complete defense lab for Security Monitoring and Log Analysis using a SIEM platform.

The repository features multiple real-world attack simulations that SOC Analysts are likely to encounter. Each use case comes with:

* 🔎 Detection logic
* 📋 Detailed write-ups
* 🖼️ Screenshots
* 🎯 Clear goals

This lab helps improve your ability to monitor, detect, and respond to cyber threats using logs and SIEM technology.



🧭 LAB ARCHITECTURE

| Component               | Role                                                                       |
| ----------------------- | ---------------------------------------------------------------------------|
| 🧑‍💻 Host Machine        |  Runs the Splunk Web Interface (Splunk Enterprise)                          |
| 🪟 Windows 11 VM       | Target machine: Contains Sysmon, Event Logs, and Splunk Universal Forwarder |
| 🐱‍💻 Kali Linux VM    | Attacker machine: Simulates attacks using tools like Hydra and CrackMapExec  |



🎯 OBJECTIVES

* Set up a realistic SOC environment using virtual machines and Splunk.
* Simulate common attack scenarios such as brute-force attacks and lateral movement.
* Learn how to ingest, parse, and analyze security logs using Splunk.
* Build and validate detection logic using actual logs and events.
* Strengthen your incident response capabilities by identifying threats through log analysis.



🛠️ TOOLS & ENVIRONMENT

  🖥️  VMware – For virtual environment management
  
  🐧  Kali Linux – For simulating attacker behavior
  
  📡  Splunk Forwarder & Sysmon – For log collection from the target system
  
  📊  Splunk Enterprise – For log ingestion, correlation, and monitoring



🚨 USE CASES



1. 🔐 [Brute Force Detection](use-case-1-brute-force-detection/)

  Techniques: Credential stuffing, repeated failed login attempts
  
  Event ID: `4625` (Failed login – Windows Security Log)
  
  Tools: Hydra, CrackMapExec
  
  Logic:

  ```spl
  index="wineventlog" source="WinEventLog:Security" EventCode=4625
  ```
  Goal: Detect brute force attacks by identifying multiple failed login attempts against valid or invalid user accounts.



2. ⏰ [Suspicious Logon Time](use-case-2-suspicious-logon-time/)

  Techniques: After-hours logon by adversaries or insider threats

  Event ID: `4624` (Successful logon)
  
  Tools: `smbclient`, simulated manual login
  
  Logic:

  ```spl
  index="wineventlog" EventCode=4624 earliest=-24h@h latest=now
  ```
  Goal: Detect logons that occur outside normal working hours (9 AM – 7 PM) to identify possible unauthorized access.



3. 🖥️ [Lateral Movement via RDP](use-case-3-lateral-movement-rdp/)

  Techniques: RDP-based movement from one system to another
  
  Event ID: `4624` (Logon Type 10 for Remote Interactive Logon)
  
  Tools: `xfreerdp`, `rdesktop`, Windows RDP
  
  Logic:

  ```spl
  index="wineventlog" EventCode=4624 Logon_Type=10
  ```
  Goal: Identify unauthorized RDP logins that may indicate lateral movement within the network.



4. 📝 [Log Tampering](use-case-4-log-tampering/)

  Techniques: Clearing security logs to cover tracks
  
  Event ID: `1102` (Audit log cleared)
  
  Tools: Manual log clearing (Event Viewer)
  
  Logic:

  ```spl
  index="wineventlog" EventCode=1102
  ```
  Goal: Detect when an attacker clears logs to remove evidence of compromise.



5. 👤 [Hidden User Account](use-case-5-hidden-user-account/)

  Techniques: Creation of backdoor or stealth accounts
  
  Event ID: `4720` (User account created)
  
  Tools: `net user` command, PowerShell
  
  Logic:

  ```spl
  index="wineventlog" EventCode=4720
  ```
  Goal: Detect suspicious user account creation on endpoint systems.



🎓 LESSONS LEARNED

Throughout this project, I developed **practical SOC Analyst skills** and learned to:

* Set up and configure a SIEM lab from scratch
* Simulate common attacks like brute-force, lateral movement, and log tampering
* Collect and analyze logs with Sysmon, Event Viewer, and Splunk
* Craft **detection logic** that enables rapid threat identification
* Build intuition for **real-world detection engineering**



🚀 HOW TO USE THIS REPOSITORY

📁 Explore each use-case folder to find:

* ✅ Step-by-step attack simulations
* 🔎 Detection rules written in SPL
* 🖼️ Screenshots of detections inside Splunk
* 💬 Explanations of what the attack is and how it was detected

Whether you're training, building a home lab, or preparing for a SOC role — this repo is for hands-on learning.



🤝 CONTRIBUTION

Contributions are welcome! 🚀


📫 CONTACT

If you have questions or need help with setup or logic building:

📬 Reach out on LinkedIn
Made with ❤️ by [Aishat Olayinka Yusuf](https://www.linkedin.com/in/aishat-olayinka-yusuf-3a16aa1b4)



