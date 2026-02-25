🛡️ Mini SOC Lab – Splunk SIEM (L1 SOC Analyst Project)
📌 Project Summary

This project simulates a real-world L1 SOC monitoring environment using Splunk Enterprise as the SIEM platform.

The lab was built to:

Monitor Windows security logs

Detect brute-force attempts

Identify suspicious login activity

Create alerts based on defined thresholds

Perform basic incident investigation workflow

The environment replicates how an L1 SOC Analyst monitors authentication-based threats in an enterprise network.

🏗️ Lab Environment
🔹 Infrastructure

SIEM Server: Ubuntu – Splunk Enterprise

Victim Machine: Windows 10

Attacker Machine: Kali Linux

Log Forwarding: Splunk Universal Forwarder

🔹 Log Flow
Kali Linux (Attacker)
        ↓
Windows 10 (Victim – RDP Enabled)
        ↓
Splunk Universal Forwarder
        ↓
Splunk Enterprise (SIEM)

Windows Security and System logs were forwarded to Splunk in real time.

⚙️ Configuration Overview
🔹 Data Collection

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

Splunk receiver was enabled on port 9997.

🔍 Use Case 1 – RDP Brute Force Detection
🛑 Attack Simulation

Used Hydra from Kali Linux to perform an RDP brute-force attack against Windows 10.

🔎 Detection – Failed Logins

Event ID: 4625
Logon Type: 10 (RemoteInteractive – RDP)

index=wineventlog EventCode=4625 Logon_Type=10
| stats count by Account_Name, Source_Network_Address
| sort - count
📊 Observation

Multiple failed login attempts detected

Same source IP generating high number of failures

Targeting a single user account

This triggered brute-force suspicion.

🔍 Use Case 2 – Successful Login Detection

Event ID: 4624
Logon Type: 10

index=wineventlog EventCode=4624 Logon_Type=10
📊 Observation

Successful login detected from same attacker IP

Correlated with prior 4625 failures

Indicates compromised credentials

🔍 Use Case 3 – New User Creation Monitoring

Event ID: 4720

index=wineventlog EventCode=4720

Monitored for unauthorized account creation (Persistence attempt).

🔍 Use Case 4 – Password Reset Monitoring

Event ID: 4723, 4724

index=wineventlog (EventCode=4723 OR EventCode=4724)

Used to detect potential account manipulation activity.

🚨 Alert Configuration (L1 SOC Level)

Configured real-time alerts for:

More than 5 failed login attempts from same IP within 5 minutes

Successful login after multiple failures

New account creation

Password reset activity

Alert severity categorized as:

Medium – Multiple failed logins

High – Successful login after brute force

High – Unauthorized account creation

🧠 Investigation Workflow (L1 SOC Process)

When alert triggered:

Reviewed Event ID and Logon Type

Identified source IP address

Checked number of failed attempts

Verified if successful login occurred

Correlated timestamps

Escalated as confirmed brute-force compromise

🛡️ MITRE ATT&CK Mapping
Technique	ID	Description
Brute Force	T1110	Repeated password attempts
Valid Accounts	T1078	Successful credential usage
Create Account	T1136	Persistence mechanism
📊 Dashboard Panels Created

Failed Login Trend

Top Source IPs by Login Failures

Successful RDP Logins

New User Creations

Password Changes

Authentication Activity Timeline

📈 Detection Outcome

During simulation:

20+ failed RDP login attempts detected

Successful login identified from attacker IP

Clear correlation between brute-force attempts and account compromise

Alerts triggered as expected

🎯 Skills Demonstrated (L1 SOC Role)

Log Monitoring & Analysis

Event ID Investigation

Basic Threat Detection

SPL Query Writing

Alert Tuning

Brute Force Analysis

Incident Correlation

MITRE ATT&CK Understanding

📚 Key Learning

Understanding Windows Authentication Logs

Detecting brute-force patterns

Correlating failed and successful logins

Performing L1-level incident validation

Building practical SIEM monitoring use cases

👨‍💻 Author

Akash Reddy A K
Aspiring SOC Analyst | SIEM & Log Monitoring Enthusiast
