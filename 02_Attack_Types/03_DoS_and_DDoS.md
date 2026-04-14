# DoS And DDoS

# DoS and DDoS

## DoS
- A DoS (Denial of Service) attack is a type of cyberattack where a single internet connected system floods a target system, usually a server, with excessive traffic to make it unavailable.
- The attacker sends a large number of requests, which can overwhelm the server, causing it to slow down, crash, or stop responding to legitimate users.
- These attacks are often aimed at websites or online services to disrupt availability.

### Key Characteristics
- Single source: originates from one system
- Traffic volume: high traffic, but from a single origin
- Traceability: easier to trace since it comes from one source

### Flow 

### Flow 
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

### Flow 


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
       (blocked)    (blocked)
```

     