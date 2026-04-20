# IDS & IPS

## What is IDS?
- An intrusion detection system is a software or hardware solution that monitors network traffic or system activity. Its primary function is to detect suspicious behavior and send alerts to administrators when potential threats are identified  
- IDS monitors internal network traffic  

### Analogy 
- If a firewall is the gatekeeper of a building (checking and allowing or blocking entry), then an IDS is like a security camera inside the building that monitors activity for suspicious behavior  

### Why IDS?
- A firewall acts as a barrier between LAN and the internet, but it can be bypassed  
- Protocols like DNS and HTTPS are legitimate and usually allowed by firewalls  
- So we need a layer that monitors traffic after it passes the firewall  
- IDS provides this additional layer of monitoring  

### Purpose of IDS
- Threat visibility: helps detect suspicious activity and abnormal behavior  
- Alerting and logging: generates alerts and records events for analysis  
- Policy monitoring: identifies deviations from expected behavior and possible policy violations  

### Where it sit in network 
IDS can be deployed:
- on the network (NIDS), usually after the firewall to monitor internal traffic
- on hosts (HIDS), to monitor system level activity

---

## Types of IDS

### Deployment Mode
- **Host IDS (HIDS):** installed on individual systems, monitors activity on that specific host  
- **Network IDS (NIDS):** deployed on the network, monitors traffic across multiple systems  

---

### Detection Mode
- **Signature based IDS:**  
  - detects attacks using known patterns (signatures) stored in a database  
  - more signatures = better detection  
  - cannot detect unknown attacks without signatures  

- **Anomaly based IDS:**  
  - learns normal network behavior (baseline)  
  - detects deviations from this baseline  
  - may generate more false positives  

- **Hybrid IDS:** combines both signature based and anomaly based detection  

---

## What is IPS 
- An intrusion prevention system is a security solution that monitors network traffic and system activity to detect malicious behavior and automatically block or mitigate threats in real time  

### Purpose of IPS 
- Real time threat mitigation: detects and blocks attacks, malware, and unauthorized access  
- Deep packet inspection: analyzes traffic to identify suspicious patterns that firewalls may miss  
- Automated response: blocks connections, isolates systems, or updates rules to stop threats immediately  

- By acting as an active filter, IPS reduces manual effort and works with firewalls and other security tools to protect the network  

### Where it sit in network 
- IPS is deployed inline in the network so that all traffic passes through it.
- This allows IPS to detect and block malicious activity in real time.
- It is commonly placed after the firewall but can also be used to protect internal network segments.
---

## IPS vs IDS

- IDS detects and alerts on suspicious activity, while IPS detects and blocks it in real time  
- IDS is passive (monitoring and alerting), while IPS is active (prevention)  
- IDS helps in analysis and visibility, while IPS helps in stopping attacks immediately  
- IDS may alert on unusual traffic patterns, while IPS can block malicious traffic based on rules or signatures  

### Key Points to Remember

- IDS monitors traffic and generates alerts, while IPS can block traffic in real time  
- IDS is passive (out of path), IPS is active (inline in the network)  
- IDS provides visibility into suspicious activity, IPS focuses on prevention  
- Network IDS monitors traffic across the network, Host IDS monitors activity on a single system  
- Signature based detection works for known attacks, anomaly based helps detect unknown behavior  
- IPS must be inline to block traffic, IDS does not need to be inline  
- IDS is useful for analysis and investigation, IPS is useful for stopping attacks immediately  
- Both IDS and IPS work alongside firewalls as additional security layer  
