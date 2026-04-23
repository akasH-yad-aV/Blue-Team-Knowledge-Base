# Mitre Attack Framework

## What is mitre attack framework ?
- MITRE ATT&CK is a structured framework and knowledge base that maps how attackers operate in real world scenarios by organizing their behavior into tactics (goals) and techniques (how they achieve those goals)

---

### Key component 
- tactic :- describes the high level goal or why of the attack  
- techniques :- general method used to achieve a tactic (how of the attack)  
- procedure :- step by step actions taken by attacker in real attack  
- sub techniques :- more specific version of a technique  
- mitigation and detection :- recommendations on how to prevent or detect specific techniques  


## Attack Matrix 
- ATT&CK matrix is a table like structure  
- columns represent tactics (attacker goals)  
- under each tactic there are multiple techniques  
- it shows how an attacker can move step by step in an attack  

example flow:
```
Initial Access --> Execution --> Persistence --> Privilege Escalation --> Lateral Movement --> Collection --> Exfiltration
```

---

## Example Mapping 
- tactic :- Credential Access  
- technique :- OS Credential Dumping  
- example :- attacker dumps LSASS memory to extract passwords  

---

## Why mitre attack matter 
- helps understand how attackers behave in real world  
- connects attacker activity with detection  
- widely used by SOC analysts, threat hunters and security teams  
- helps in mapping logs to attacker behavior  

---

## Detection Perspective 
- MITRE ATT&CK helps defenders move from indicator based detection to behavior based detection  
- instead of just blocking IP or hash, we focus on what attacker is doing  
- logs, alerts and events can be mapped to techniques  
- this helps in identifying attack patterns and ongoing attacks  

---

## Use Cases 
- SOC monitoring  
- threat hunting  
- incident response  
- red teaming  

---

## Limitations 
- does not show exact sequence of attack (not linear)  
- very large and can be overwhelming  
- requires understanding to use properly  

---

## Relation with other frameworks 
- Cyber Kill Chain :- shows stages of attack  
- Unified Kill Chain :- more detailed lifecycle  
- MITRE ATT&CK :- focuses on attacker behavior and techniques  
- Pyramid of Pain :- focuses on detection difficulty  

---

## Key Points 
- MITRE ATT&CK is behavior based framework  
- tactics = why attacker is doing something  
- techniques = how attacker is doing it  
- helps in detection, analysis and threat hunting  
- focuses on real world attack patterns  