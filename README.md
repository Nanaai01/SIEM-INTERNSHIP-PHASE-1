# 🔐 SIEM MONITORING LAB




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







REFECTION QUESTIONS AND EVALUATION


1.	What is the role of SIEM in modern Cybersecurity?
Security information and event management (SIEM) solution role in modern cybersecurity is to collect, aggregate, and analyze large volumes of data from organization-wide applications, devices, servers, and users in real time. By consolidating this vast array of data into a single, unified platform, SIEM solutions provide a comprehensive view of an organization's security posture, empowering the Security Operations Center (SOC) to detect, investigate, and respond to security incidents swiftly and effectively.


2.	 Challenges faced while setting up my lab
i.	 Limited processing power or memory in virtualized lab setups can slow down analysis or cause instability during testing.
ii.	 Not being able to connect the splunk forwarder and splunk instant due to a firewall blocking port 9997 which was later resolved.
iii. Not being able to establish a TCP connection between the victim machine (windows 11 in VMware, splunk forwarder and sysmon) and the monitoring machine (Windows 11, splunk instant)
iv.  Network segmentation or strict firewall rules might have prevented certain tools from connecting or executing commands remotely. 


4.	Difference between Sysmon Logs and Windows Security Logs
(Sysmon) System Monitor and Windows Security Log are both event logging mechanisms in windows but have some differences. Specifically, the differences are;
Sysmon provides detailed, high-quality event logging for security monitoring, 
While Windows Event Log offers general system, application, and security event logging. Sysmon focuses on tracking specific activities like process creation and network connections, . It provided detailed information about process creations, network connections, and changes to the file.
whereas Windows Event Log captures a broader range of system events.


5.  Brute force attacks show up in Windows logs as many failed login attempts:

* Event ID 4625: Failed login attempts. Multiple such events from the same user or IP in a short time suggest brute force.
* Event ID 4740: Account lockout after too many failed tries (if lockout policies are enabled).

Detection involves spotting repeated 4625 events for the same user/IP quickly.

Example query: Find users/IPs with more than 5 failed logins in a timeframe.


5.  To detect logins outside normal hours, you:

* Use Event ID 4624 (successful logins).
* Filter logins by time, flagging those that occur outside defined “normal” business hours.
* Compare login times against expected user activity windows to spot anomalies.

This helps catch suspicious access during odd hours, possibly indicating unauthorized use.


6.  RDP lateral movement is tracked in logs primarily through Windows Security Event ID 4624 with Logon Type 10, which indicates a successful Remote Interactive logon via RDP. This event records the username, source IP, timestamp, and machine name, helping identify when and from where a remote desktop session was initiated.

Additional events, like Event ID 4648 (explicit credentials used) or network logs showing RDP connection attempts (port 3389), can supplement detection. Monitoring for unusual or unauthorized RDP logins, especially from unexpected IPs or outside business hours, helps spot lateral movement in the network.

 
7.	Risk of Log Tampering and how it can be detected
Log tampering posses a significant risk because it allows attackers to hide their malicious activities, obstruct investigations, and undermine sytem integrity.
The following are the risks of log tampering;
i.	Evasion of detection
ii.	Financial losses and reputation damage
iii.	Impeding Investigation
iv.	Undermining system integrity

Detection of log tampering are;
i.	Monitoring for unusual system behavior
ii.	Log pattern analysis
iii.	Secure logging protocols
iv.	Centralized log management


8.	Improvements I would have made in my lab set up
i.	Test run different tools that can be used for log capturing if given the time to see which is best or most suitable
ii.	Uninstall and reinstall some tools to see that I master the lab set up off hand

9.	This will help me in real life interviews or job because this lab provides me with real life scenario and hands on experience in this field as mastering the different SIEM tools, log and event capturing, detecting, mitigating and responding to threats are very important skills to have in order to successfully build a career as a Security Operations Center (SOC) Analyst.

10.	My biggest takeaway from this phase is how I am able to set up a real time monitoring and detection lab, as well as simulate and respond to real life scenario. This is a big knowledge acquired for me and I excitedly look forward to the next 5 phases. 




📫 CONTACT

If you have questions or need help with setup or logic building:

📬 Reach out on LinkedIn
Made with ❤️ by [Aishat Olayinka Yusuf](https://www.linkedin.com/in/aishat-olayinka-yusuf-3a16aa1b4)



