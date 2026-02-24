# RDP Brute Force Lab Notes

## Setup
- Kali Linux VM (attacker)
- Windows 10 VM (target) with RDP enabled
- Splunk Forwarder installed on Windows

## Steps
1. Check RDP port:

nmap -p 3389 <Windows_IP>

2. Test RDP connectivity:

xfreerdp3 /v:<Windows_IP> /u:akash /cert:ignore

3. Simulate brute-force:
- Enter wrong password multiple times
4. Verify Event IDs on Windows:
- 4625, 4624, 4672
5. Check Splunk searches:

index=wineventlog EventCode=4625

6. Harden Windows RDP:
- Enable NLA
- Set firewall rules
- Configure auditing
7. Troubleshooting:
- IP changes via DHCP → set static IP
- Broken pipe / TLS errors → check security layer (NLA/TLS)
