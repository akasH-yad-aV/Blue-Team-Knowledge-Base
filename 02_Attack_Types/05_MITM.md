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




