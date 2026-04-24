# SIEM 

## What is SIEM?
- Security Information and Event Management is a security solution that provides real time monitoring, analysis, and logging of security data from across an organization’s IT infrastructure  

---

### Core Components 
- security information management :- collects and stores log data for reporting and analysis  
- security event management :- provides real time monitoring, correlation of events, and alerting  

---

## Features of SIEM

### Centralized Log Collection 
- SIEM collects logs from multiple sources (endpoints, servers, firewalls) and stores them in one place  
- logs are collected using agents or APIs and sent to the SIEM  
- this removes the need to check logs on individual systems  



### Normalization of Logs
- logs from different sources have different formats  
- SIEM converts them into a common format  
- this process is called parsing and normalization  



### Correlation of Logs 
- individual logs are not very useful on their own  
- SIEM correlates logs from different sources to find relationships  
- this helps identify suspicious or abnormal activity  



### Real Time Alerting 
- SIEM detects suspicious activity based on rules  
- some rules are built in, and analysts can create custom rules  
- alerts are generated when conditions are met  


### Dashboard and Reporting 
- SIEM presents analyzed data in dashboards  
- helps in monitoring, investigation, and reporting  
- provides summarized and actionable insights  

---

## Alert vs Event 

- **event** :- any raw observable activity or log entry  
  example: user login, logout, process creation  

- events provide visibility  

- **alert** :- a triggered notification generated when events match specific rules or thresholds  
  example: 50 failed login attempts in one minute  

- alerts require action  

---

## Log Sources 

### Windows Machine 
- Windows records events that can be viewed through Event Viewer  
- each event has a unique Event ID, which helps in tracking and analysis  


### Linux 
- Linux stores system and application logs  

- some common locations:


 ```
 /var/log/httpd: Contains HTTP Request  / Response and error logs.
/var/log/cron: Events related to cron jobs are stored in this location.
/var/log/auth.log and /var/log/secure: Stores authentication-related logs.
/var/log/kern: This file stores kernel-related events.
```

