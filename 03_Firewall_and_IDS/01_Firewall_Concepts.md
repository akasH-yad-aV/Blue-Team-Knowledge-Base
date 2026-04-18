# Firewalls

## What is a firewall?
- A firewall is a security solution that acts as a barrier between a local network and other networks, used to filter incoming and outgoing packets  
- It blocks or allows network traffic entering or leaving the local network based on rules  

### How does a firewall work
- A firewall inspects all incoming and outgoing traffic and decides whether to allow or block it  
- All data packets entering or leaving the network must pass through the firewall, which decides whether to allow or block them  
- The firewall examines each packet against predefined security rules set by the organization  
- If the packet matches safe rules, it is allowed; if it is suspicious, it is blocked  
- Blocked or unusual traffic is recorded in logs, and real time alerts may be generated for serious threats  
- Since it is not possible to define every rule, the firewall applies a default policy (accept, reject, or drop). Setting the default policy to drop or reject is considered best practice to prevent unauthorized access  

### Diagram for firewall
```
        Internal Network 
   ____________|_____________     
  |                         |              Firewall 
     File transfer server <------->         |    |       
                                            |    |
     Database server <------------>         |    |    
                                            |    |   <-------------> Internet
     computer A <----------------->         |    |
                                            |    |
     computer B <----------------->         |    |
 
```


### Purpose of firewall 

- Blocking unauthorized access: firewalls inspect packets to block attackers, bots, and malicious traffic  
- Threat prevention: they help block malicious traffic such as malware, DoS traffic, and suspicious activity  
- Network segmentation: they divide networks into zones to limit the spread of attacks  
- Traffic filtering: only traffic that matches allowed rules is permitted  
- Compliance and privacy: they help protect sensitive data and meet security requirements  

--- 

## Types of firewall 

### Stateless firewall 
- operates on layer 3 and layer 4  
- filters packets based on predefined rules, checking each packet independently  
- does not maintain information about previous connections  
- processes packets quickly but lacks context  

### Stateful firewall 
- filters traffic based on rules and connection state  
- keeps track of active connections in a state table  
- allows packets that are part of an established connection  

### Proxy firewall 
- operates at the application layer  
- acts as an intermediary between client and server  
- instead of direct communication, the firewall creates two separate connections  

```
client request -----> proxy (if allowed) ------> destination server
client <------------- if allowed (proxy) <------- Destination server response
```

### Next Generation firewall 
- advanced firewall that combines traditional firewall functions with deep packet inspection and intrusion prevention  
- traditional firewalls check IP, port, and protocol, while this type also analyzes application data and user activity  