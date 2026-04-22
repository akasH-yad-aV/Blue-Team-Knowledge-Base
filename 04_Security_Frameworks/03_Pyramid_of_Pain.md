# Pyramid of Pain 

## What is Pyramid of Pain?
- Pyramid of Pain is a security framework that shows how much effort we force an attacker to spend when we detect or block something  


### Purpose of Pyramid of Pain 
- to guide organizations and individuals away from relying on easily changeable indicators and toward detecting TTPs, which are significantly harder for adversaries to modify  
- by focusing on higher levels of the pyramid, defenders can force attackers to spend more time, money, and resources, increasing the cost of the attack and sometimes making them abandon the campaign  

---

## 6 Levels of Pyramid of Pain 

### 1. Hash Values (Trivial) 
- cryptographic signatures of malware files  
- easy for attackers to change with small modifications  

### 2. IP Addresses (Easy)
- can be bypassed using proxies, dynamic IPs, or VPNs  



### 3. Domain Names (Simple)
- malicious domains can be quickly registered or abandoned  



### 4. Network / Host Artifacts (Annoying)
- includes URL patterns, registry keys, logs, or file paths  
- causes inconvenience for attackers but still manageable  


### 5. Tools (Challenging)
- attacker tools and executables are harder to change  
- modifying them requires effort and can affect functionality  


### 6. Tactics, Techniques, and Procedures (Tough)
- behavioral patterns used by attackers  
- very difficult to change because they represent how the attacker operates  

---
## Detection Strategy

- lower levels (hash, IP, domain) are easy to detect but also easy for attackers to change  
- higher levels (tools, TTPs) are harder to detect but more effective in long term defense  
- focusing on behavior gives better detection compared to static indicators  

---

## Attacker vs Defender Perspective

- lower levels → low effort for attacker to change  
- higher levels → high effort and cost for attacker  
- goal of defender is to push detection toward higher levels of the pyramid  

---

## Limitations

- detecting higher level behaviors can be complex  
- may require advanced tools like SIEM or EDR  
- not all organizations can fully operate at higher levels  

---
## Key Points to Remember

- Pyramid of Pain measures how difficult it is for attackers to adapt  
- lower level indicators are easy to detect but easy to bypass  
- higher level detection focuses on behavior and is more effective  
- defenders should aim to move detection toward TTPs  
- increasing attacker effort can slow down or stop attacks  
