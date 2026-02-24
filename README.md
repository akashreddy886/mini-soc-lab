# mini-soc-lab

# 🛡️ End-to-End SOC Lab – Splunk SIEM

## 📌 Project Overview

This project demonstrates the setup of a complete Security Operations Center (SOC) lab environment using:

- Ubuntu (Splunk Enterprise – SIEM)
- Windows 10 (Victim Machine)
- Kali Linux (Attacker Machine)

The objective of this lab is to simulate real-world cyber attacks and detect them using Splunk.

---

## 🏗️ Lab Architecture

Attacker (Kali Linux)
        ↓
Victim (Windows 10)
        ↓
Splunk Universal Forwarder
        ↓
Splunk Enterprise (Ubuntu SIEM)

---

## 🛠️ Tools & Technologies Used

- Splunk Enterprise
- Splunk Universal Forwarder
- Kali Linux
- Windows Event Logs
- Hydra (Brute Force Testing)
- RDP Protocol
- VirtualBox / VMware

---

## 🔧 Installation & Configuration

### 1️⃣ Installed Splunk Enterprise on Ubuntu

- Downloaded Splunk from official website
- Enabled receiving on port 9997
- Configured index for Windows logs

### 2️⃣ Installed Splunk Universal Forwarder on Windows

Configured inputs.conf:

```
[WinEventLog://Security]
disabled = 0
index = wineventlog

[WinEventLog://System]
disabled = 0
index = wineventlog
```

Configured outputs.conf:

```
[tcpout]
defaultGroup = splunk_indexer

[tcpout:splunk_indexer]
server = <Splunk_Server_IP>:9997
```

---

## 🔍 Attack Simulations Performed

### ✅ 1. Brute Force Attack (Hydra)

Performed RDP brute-force attack from Kali Linux.

Detection Query:

```
index=* EventCode=4625
| stats count by Account_Name, Source_Network_Address
```

---

### ✅ 2. Successful Login Detection

```
index=* EventCode=4624
```

---

### ✅ 3. New User Creation Detection

```
index=* EventCode=4720
```

---

### ✅ 4. Password Change Monitoring

```
index=* EventCode=4723 OR EventCode=4724
```

---

## 📊 Dashboard Created

Created Splunk Dashboard including:

- Failed Login Attempts
- Successful Logins
- New User Creations
- RDP Login Monitoring
- Source IP Monitoring

---

## 🚨 Alerts Configured

Configured real-time alerts for:

- Multiple Failed Login Attempts
- Suspicious User Creation
- Excessive Login Failures from Same IP

---

## 🎯 Skills Demonstrated

- SIEM Deployment
- Log Collection & Parsing
- Windows Event Monitoring
- Threat Detection
- SPL Query Writing
- Alert Creation
- SOC Investigation Workflow

---

## 📸 Screenshots

(Add screenshots of your Splunk dashboard here)

---

## 📚 Learning Outcome

- Understood end-to-end SOC workflow
- Hands-on experience in attack simulation & detection
- Built practical SIEM monitoring environment

---

## 👨‍💻 Author

Akash Reddy  
Cybersecurity Enthusiast  
