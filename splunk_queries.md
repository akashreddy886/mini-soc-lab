# Splunk Queries – RDP Brute Force Detection Lab

## Failed RDP Logins (Event ID 4625)
index=wineventlog EventCode=4625

## Successful RDP Logins (Event ID 4624, Logon Type 10)
index=wineventlog EventCode=4624 Logon_Type=10

## Privileged Logon (Event ID 4672)
index=wineventlog EventCode=4672

## Brute Force Detection (Multiple Failed Attempts from Same IP)
index=wineventlog EventCode=4625
| stats count by Account_Name, Source_Network_Address
| where count > 5

## Advanced Detection – Brute Force Success Correlation

index=wineventlog (EventCode=4625 OR EventCode=4624)
| eval src_ip=coalesce(Source_Network_Address, src)
| eval user=coalesce(Account_Name, user)
| bin _time span=10m
| stats 
    count(eval(EventCode=4625)) as failed_attempts
    count(eval(EventCode=4624)) as success_attempts
    by _time, user, src_ip
| where failed_attempts >= 5 AND success_attempts >= 1
