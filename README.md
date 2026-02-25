# mini-soc-lab
Project Summary

This project simulates a real-world L1 SOC monitoring environment using Splunk Enterprise as the SIEM platform.

The lab was designed to:

Monitor Windows security logs

Detect brute-force login attempts

Identify suspicious authentication activity

Configure alerts based on defined thresholds

Perform basic incident investigation workflow

The environment reflects the day-to-day responsibilities of an L1 SOC Analyst handling authentication-related security events.

Lab Environment
Infrastructure

SIEM Server: Ubuntu – Splunk Enterprise

Victim Machine: Windows 10

Attacker Machine: Kali Linux

Log Forwarding: Splunk Universal Forwarder

Log Flow Architecture
Kali Linux (Attacker)
        ↓
Windows 10 (Victim – RDP Enabled)
        ↓
Splunk Universal Forwarder
        ↓
Splunk Enterprise (SIEM)

Windows Security and System logs were forwarded to Splunk in real time.

Configuration Overview
Data Collection

Configured inputs.conf on Windows:

[WinEventLog://Security]
disabled = 0
index = wineventlog

[WinEventLog://System]
disabled = 0
index = wineventlog

Configured outputs.conf:

[tcpout]
defaultGroup = splunk_indexer

[tcpout:splunk_indexer]
server = <Splunk_Server_IP>:9997

Splunk receiver was enabled on port 9997 for log ingestion.

Use Case 1 – RDP Brute Force Detection
Attack Simulation

A brute-force attack was performed using Hydra from Kali Linux against the Windows RDP service.

Detection – Failed Logins

Event ID: 4625
Logon Type: 10 (RemoteInteractive – RDP)

index=wineventlog EventCode=4625 Logon_Type=10
| stats count by Account_Name, Source_Network_Address
| sort - count
Analysis

Multiple failed login attempts observed

High number of failures from a single source IP

Targeted authentication attempts against one user account

This activity indicated a brute-force attempt.

Use Case 2 – Successful Login Detection

Event ID: 4624
Logon Type: 10

index=wineventlog EventCode=4624 Logon_Type=10
Analysis

Successful login detected from the same source IP

Timestamp correlation with previous failed attempts

Indicates possible credential compromise

Use Case 3 – New User Creation Monitoring

Event ID: 4720

index=wineventlog EventCode=4720

Used to monitor potential unauthorized account creation.

Use Case 4 – Password Reset Monitoring

Event ID: 4723, 4724

index=wineventlog (EventCode=4723 OR EventCode=4724)

Used to detect suspicious password changes or resets.

Alert Configuration (L1 SOC Level)

Configured real-time alerts for:

More than 5 failed login attempts from the same IP within 5 minutes

Successful login after multiple failures

New account creation

Password reset activity

Alert severity classification:

Medium: Multiple failed login attempts

High: Successful login following brute-force attempts

High: Unauthorized account creation

Investigation Workflow (L1 SOC Process)

When an alert was triggered:

Verified Event ID and Logon Type

Identified the source IP address

Reviewed number of failed attempts

Checked for successful login activity

Correlated timestamps

Documented findings and escalated if required

MITRE ATT&CK Mapping
Technique	ID	Description
Brute Force	T1110	Repeated password attempts
Valid Accounts	T1078	Successful credential abuse
Create Account	T1136	Persistence through account creation
Dashboard Panels Created

Failed Login Trend

Top Source IPs by Login Failures

Successful RDP Logins

New User Creations

Password Changes

Authentication Activity Timeline

Detection Outcome

During the attack simulation:

Multiple failed RDP login attempts were detected

A successful login was identified from the attacker IP

Correlation confirmed brute-force compromise scenario

Alerts triggered according to configured thresholds

Skills Demonstrated

Log Monitoring and Analysis

Windows Event ID Investigation

Threat Detection

SPL Query Writing

Alert Configuration and Tuning

Brute Force Analysis

Incident Correlation

MITRE ATT&CK Mapping

Key Learning Outcomes

Understanding Windows authentication logs

Detecting brute-force attack patterns

Correlating failed and successful login events

Performing L1-level incident validation

Building practical SIEM detection use cases

Author

Akash Reddy A K
Aspiring SOC Analyst | SIEM and Log Monitoring
Cybersecurity Enthusiast  
