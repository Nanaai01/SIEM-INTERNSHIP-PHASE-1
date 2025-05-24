Project Overview

Welcome to my SIEM Monitoring Lab! The project entails setting up a defence lap for Security Monitoring and Log Analysis.
This repository contains several use cases demonstrating how to detect different types of cyber attacks using a SIEM (Security Information and Event Management) platform.

Each use case simulates an attack scenario and includes detection logic, writeups, and screenshots to help you understand how to monitor and respond to security events.


Tools & Environment   
- VMware (Hypervisor)

- Kali Linux (Simulation of Attacks)

- Splunk Forwarder, Sysmon (For Log collection)

- Splunk Instance (For Log monitoring)


Use Cases

1. [Brute Force Detection](use-case-1-brute-force-detection/)  
   Detect repeated failed login attempts to identify brute force attacks.

2. [Suspicious Logon Time](use-case-2-suspicious-logon-time/)  
   Detect logins outside of normal business hours.

3. [Lateral Movement via RDP](use-case-3-lateral-movement-rdp/)  
   Identify unauthorized lateral movement using Remote Desktop Protocol.

4. [Log Tampering](use-case-4-log-tampering/)  
   Detect when logs are being altered or deleted.

5. [Hidden User Account](use-case-5-hidden-user-account/)  
   Find unauthorized hidden user accounts on the system.

Lessons Learned

I learned a highly demanded skill needed by all SOC Analystto excel. i gained hands-on experience by single handedly setting up a defence lab which is used for Security Monitoring and Log Analysis.
I also learned how to simulate and respond to some common attacks. They are; Bruteforce Attack, suspicious login attempt, log tampering etc

How to Use This Repository

- Click on any use case folder to explore detailed documentation, detection scripts, attack simulations, and screenshots.
- Use this lab for learning, testing, or improving your SIEM skills.


Contributing

Contributions are welcome! Feel free to add new use cases, improve detection rules, or suggest enhancements.
