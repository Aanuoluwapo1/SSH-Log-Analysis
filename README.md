# # SSH Log Analysis & Threat Detection

## Overview

This project focuses on analyzing SSH authentication logs using Splunk to detect suspicious activities such as brute-force attacks, failed login attempts, and unauthorized access attempts.

The goal is to simulate real-world SOC analysis by ingesting logs, writing SPL queries, and building dashboards and alerts.

## Objective

- Detect failed SSH login attempts  
- Identify brute-force attack patterns  
- Monitor successful logins for suspicious behavior  
- Detect unauthenticated SSH connections  
- Build dashboards and alerts in Splunk  

## Skills Learned

- Advanced understanding of SIEM concepts and practical application.
- Proficiency in analyzing and interpreting network logs.
- Enhanced knowledge of network protocols and security vulnerabilities.
- Development of critical thinking and problem-solving skills in cybersecurity.

### Tools & Setup

- Splunk (Enterprise / Free)
- Data Source: ssh_log.json
- Sourcetype: _json
- Index: ssh_logs

## Key Data 
Fields

- event_type → type of SSH activity  
- auth_success → login success (true/false)  
- auth_attempts → number of login attempts  
- id.orig_h → source IP address  
- id.resp_h → destination host

  
## Analysis
### Failed Login Attempts

```spl
index=ssh_logs event_type="Failed SSH Login"
| stats count by id.orig_h
```
<img width="1349" height="505" alt="Screenshot (41)" src="https://github.com/user-attachments/assets/9ebd3b4e-8ad6-4153-ada1-8d19ab2e60a7" />

*Ref 1:This query identifies failed login attempts.*

### Brute Force Detection

```spl
index=ssh_logs event_type="Multiple Failed Authentication Attempts"
| stats count by id.orig_h, id.resp_h
```
<img width="1366" height="618" alt="Screenshot (42)" src="https://github.com/user-attachments/assets/c802abc8-9699-4d6e-8d21-ef71c4fdacbe" />

*Ref 2: This helps detect repeated login failures from the same source, indicating possible brute-force activity.*

Configured alert to detect brute-force activity:

- Condition: More than 3 failed login attempts
- Time window: 10 minutes

This helps identify potential attack behavior in real time.

### Successful Logins
```spl
index=ssh_logs event_type="Successful SSH Login"
| stats count by id.orig_h, id.resp_h
```
This query was used to used to track successful access and correlate with previous failed attempts.
<img width="1366" height="383" alt="Screenshot (43)" src="https://github.com/user-attachments/assets/c15d68d0-b883-4641-a88f-e145f6ad3ba6" />

*Ref 3: Dashboard panel showing top source IPs for successful logins.*


### Unauthenticated Connections
```spl
index=ssh_logs event_type="Connection Without Authentication"
| timechart count by id.orig_h
```
<img width="1342" height="289" alt="Screenshot (44)" src="https://github.com/user-attachments/assets/b0462894-7e4c-48ab-bae7-4991777d4d64" />

*Ref 4: This query helps identify scanning or probing activity.*


### 9. Key Takeaways
- Learned how to ingest and parse JSON logs in Splunk
- Improved SPL query writing skills
- Understood brute-force and scanning attack patterns
- Gained hands-on experience with SIEM workflows

## Conclusion
This project demonstrates practical experience in security log analysis using Splunk. It highlights the ability to detect suspicious behavior, visualize attack patterns, and configure alerts for proactive threat monitoring.
