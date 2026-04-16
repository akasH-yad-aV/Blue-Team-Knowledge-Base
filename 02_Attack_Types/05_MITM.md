# MITM (Man In The Middle Attack)

## What it is?
- MITM is a cyberattack where an attacker secretly intercepts and may alter communication between two parties who think they are directly communicating with each other.
- In reality, the attacker is placed in between both sides and can see or modify the data being exchanged.

---
### How it works:
- attacker positions between user and server
- intercepts traffic
- reads or modifies data
- forwards it to target
---


### Diagram
```         
               (Data)                          (Data)       
          ----------------->             ---------------->  
   USER A      (Data)          Attacker        (Data)        Server
          <-----------------             <----------------- 
```

---

### Purpose of MITM 

- Data theft: attacker silently monitors traffic to steal sensitive information such as login credentials, banking details, or personal messages  

- Session hijacking: attacker captures session tokens to impersonate users on websites and gain access without needing passwords  

- Manipulation and fraud: attacker can modify data in real time, such as changing payment details, injecting malicious code, or altering content  

- Redirecting traffic: attacker can hijack DNS or routing to redirect users to fake websites that look legitimate and steal credentials  



## Common Methods 

### ARP Spoofing
ARP spoofing is a Man-in-the-Middle (MitM) attack where an attacker sends fake ARP messages on a local network to associate their MAC address with the IP address of a legitimate device, usually the gateway. This causes the victim’s traffic to be sent to the attacker instead of the real destination. Once in the middle, the attacker can monitor, modify, or block the data. By poisoning both the victim and the gateway, the attacker can control communication in both directions.

---

### DNS Spoofing 
DNS spoofing is an attack where an attacker intercepts a DNS request and responds with a fake IP address that points to their own server instead of the real one. This can redirect users to fake websites that look identical to legitimate ones, or allow the attacker to silently relay traffic and capture sensitive information without the user noticing.

---

### Evil Twin
An evil twin attack happens when an attacker creates a fake Wi-Fi network that looks like a legitimate one. Users connect to it thinking it is safe, but all their traffic passes through the attacker’s system. This allows the attacker to monitor activity, capture credentials, or manipulate data being sent over the network.

---

### SSL Stripping 
SSL stripping is a Man-in-the-Middle (MitM) attack that downgrades a secure HTTPS connection to an unsecured HTTP connection. By removing encryption, the attacker can read, steal, or modify sensitive data such as passwords, credit card details, and session 
cookies without the user being aware.

---
### Where it happens
- local networks (ARP spoofing, evil twin)
- public Wi-Fi (evil twin, SSL stripping)
- DNS infrastructure (DNS spoofing)

---
### Prevention
- use HTTPS websites and avoid HTTP connections
- avoid public Wi-Fi or use VPN
- verify network names before connecting
- use secure DNS services
- enable certificate warnings in browsers
---
### Key Points to Remember

- MITM attacks happen when attacker sits between user and server
- attacker can read, modify, or block data
- ARP spoofing works on local network level
- DNS spoofing redirects users to fake destinations
- evil twin targets users through fake Wi-Fi networks
- SSL stripping removes HTTPS protection
- public Wi-Fi is a common attack surface
- encryption (HTTPS) is key defense against these attacks