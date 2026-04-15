# DoS And DDoS

## DoS
- A DoS (Denial of Service) attack is a type of cyberattack where a single internet connected system floods a target system, usually a server, with excessive traffic to make it unavailable.
- The attacker sends a large number of requests, which can overwhelm the server, causing it to slow down, crash, or stop responding to legitimate users.
- These attacks are often aimed at websites or online services to disrupt availability.

### Key Characteristics
- Single source: originates from one system
- Traffic volume: high traffic, but from a single origin
- Traceability: easier to trace since it comes from one source

### Flow 

### Diagram 
```
    Attacker
   (1 machine)
         |
         |        
         V
     Target Server 
  (overhelmed , crashes)
     |           |
     |           |
     V           V
  User A      User B 
(blocked)   (blocked)

```

---

## DDoS Attack 
- A Distributed Denial of Service (DDoS) attack is similar to a DoS attack, but it involves multiple systems working together to overwhelm a target.
- These systems are often compromised devices called bots, forming a botnet that sends large amounts of traffic at the same time.
- Because the traffic comes from many different sources, it is harder to detect, trace, and block.

### Key Characteristics
- Multiple sources: traffic comes from many systems
- Traffic volume: much higher due to combined sources
- Difficulty in tracing: harder to identify the origin
- Complexity in blocking: difficult to block due to distributed nature

### Diagram  

```
Bot 1     Bot 2   Bot 3   Bot 4
  |          |     |       |     
  |          |     |       |
  |          |     |       |
  |_______   |     |   ____|  
          |  |     |   |    
          V  V     V   V
           Target server 
        (massively overhelmed)
          |             |
          |             |
          |             |
          |             |
          V             V 
        User A       User B
       (Blocked)    (Blocked)
```
---

### Types of DoS / DDoS
- Volume based attacks: flood the network with massive traffic (e.g. UDP flood)
- Protocol attacks: target server resources or network devices (e.g. SYN flood)
- Application layer attacks: target specific services like HTTP requests (e.g. HTTP flood)

---
### How it works (simple view)
- attacker controls multiple compromised devices (botnet)
- sends large number of requests to target
- server resources get exhausted
- legitimate users cannot access the service
---

### Impact
- service downtime
- loss of revenue for businesses
- disruption of online services
- damage to reputation

---

### Prevention (overview)
- use rate limiting to control request traffic
- deploy firewalls and IDS/IPS
- use DDoS protection services (e.g. CDN)
- monitor traffic for unusual spikes

--- 

### DoS vs DDoS

| Feature        | DoS                     | DDoS                          |
|---------------|------------------------|-------------------------------|
| Source        | Single system          | Multiple systems (botnet)     |
| Traffic       | Limited                | Very high                     |
| Detection     | Easier                 | Harder                        |
| Mitigation    | Easier                 | More complex                  |


### Key Points to Remember

- DoS uses a single system, while DDoS uses multiple systems (botnet)
- DDoS attacks are harder to detect and block due to distributed sources
- Main goal is to make a service unavailable to legitimate users
- High traffic overwhelms server resources like CPU, memory, or bandwidth
- Application layer attacks (like HTTP flood) can be harder to detect than volume attacks
- Botnets are commonly used in DDoS attacks
- Even a simple attack can cause major downtime and business impact
- Monitoring traffic spikes is important for early detection

### Key Points to Remember

- Malware is designed to damage systems, steal data, or gain unauthorized access
- Common entry points include phishing emails, infected downloads, and USB devices
- Viruses require a host file, while worms spread on their own
- Ransomware encrypts data and demands payment for access
- Spyware secretly collects sensitive information
- Trojans disguise themselves as legitimate software
- Users are often the weakest point in malware infection
- Downloading cracked or unofficial software increases risk
- Regular updates, antivirus, and backups reduce impact
- Always verify files and links before opening or installing