# Cyber Kill Chain 

## What is Cyber Kill Chain?
- Cyber Kill Chain is a security framework that breaks down the lifecycle of a cyberattack into 7 distinct linear stages  
- It defines the steps used by adversaries or malicious actors in cyberspace  
- To succeed, an adversary typically goes through multiple phases of an attack  
- Developed by Lockheed Martin in 2011  

---

## Phases in Cyber Kill Chain 

### 1. Reconnaissance 
- Reconnaissance is the research and planning phase of an attack, used to gather information about the target and identify vulnerabilities  

**Types of reconnaissance:-**
- Active :- involves direct interaction with the target  
  - methods :- social engineering, port scanning, banner grabbing  
- Passive :- involves no direct interaction with the target  
  - methods :- social media, OSINT, data breaches  

**Examples :-** harvesting email addresses, mapping network architecture, finding subdomains and IP addresses  



### 2. Weaponization 
- This phase involves turning information gathered during reconnaissance into a usable attack by creating malware or combining exploits into a payload  
- *Payload* is the malicious code executed on the target system  

**Examples:-**
- tailoring phishing attacks  
- creating infected documents  
- setting up command and control infrastructure  



### 3. Delivery 
- Delivery refers to the method used to transmit the payload to the target system  

**Examples :-**
- phishing emails  
- USB drops  
- watering hole attacks  



### 4. Exploitation 
- Exploitation refers to taking advantage of vulnerabilities to execute malicious code on the target system  

**Signs:-**
- unexpected process creation  
- registry changes or new service creation  
- suspicious command line arguments  



### 5. Installation
- In this phase, the attacker installs malware or a backdoor to establish persistence and maintain long term access after exploitation  

**Examples:-**
- installing a web shell  
- installing a backdoor  
- creating or modifying Windows services  
- modifying registry keys  



### 6. Command & Control 
- After gaining persistence, the attacker connects the compromised system to a command and control server to remotely control it and execute commands  

**Techniques :-** DNS tunneling, DNS beaconing  


### 7. Actions on Objectives 
- This is the final phase where the attacker achieves their goal  

**Examples:-**
- credential harvesting  
- privilege escalation  
- internal reconnaissance  
- lateral movement  
- data collection and exfiltration  

---

## Why Cyber Kill Chain 
- The primary purpose of the Cyber Kill Chain is to help security teams understand and disrupt attacks by breaking them into stages  
- It also acts as a foundation for more advanced frameworks like Unified Kill Chain and MITRE ATT&CK  

### Detection Perspective (Where to Observe)

- Reconnaissance → firewall logs, web server logs (scanning, probing)
- Delivery → email logs, web proxy logs (phishing, downloads)
- Exploitation → endpoint logs (process creation, crashes)
- Installation → system logs (services, registry changes)
- Command & Control → DNS logs, network traffic (beaconing)
- Actions on Objectives → data access logs, unusual activity

### Breaking the Kill Chain

- The attack does not need to be stopped at the final stage
- Disrupting any one phase can prevent the attack from succeeding
- Earlier detection gives more time to respond and reduces impact

 ### Limitation of Cyber Kill Chain 

- linear constraint :- the model assumes a strict sequence, but attackers often loop back or skip stages (for example, a ransomware attack might simultaneously execute delivery, exploitation, and action without clear separate step by step progression)  

- too strategic, not tactical :- it identifies that an attack is occurring but not how it is happening, making it less useful for technical and granular detection compared to MITRE ATT&CK  

- reactive focus :- over investment in preventing initial perimeter breaches can lead to poor detection of intruders already inside the network  

- insiders and identity threat :- the framework assumes external threats, missing malicious employees who already have authorized access and can skip reconnaissance and weaponization. It also fails to track attackers using valid credentials for lateral movement instead of malware  
### Key Points to Remember

- Cyber Kill Chain breaks attacks into stages
- Each stage provides an opportunity for detection
- Early detection improves defense effectiveness
- Not all attacks strictly follow this sequence
