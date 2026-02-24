# Splunk Queries – RDP Brute Force Detection Lab

## Failed RDP Logins (Event ID 4625)
index=wineventlog EventCode=4625

## Successful RDP Logins (Event ID 4624, Logon Type 10)
index=wineventlog EventCode=4624 Logon_Type=10

## Privileged Logon (Event ID 4672)
index=wineventlog EventCode=4672

## Brute Force Detection (Multiple failed attempts)
index=wineventlog EventCode=4625
| stats count by Account_Name, Source_Network_Address
| where count > 5
