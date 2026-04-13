# Credentials Attacks 

## What are Credential Attacks 
- Credential attacks are cyber attacks where an attacker tries to gain unauthorized access to a system by exploiting user credentials such as usernames and passwords through guessing, reuse, or automated techniques.

---

### Purpose of Credential Attacks 
- Unauthorized access and impersonation: attackers gain access by pretending to be valid users, allowing them to bypass security controls  
- Data theft and espionage: stolen credentials are used to access sensitive data such as personal information, intellectual property, or internal records  
- Financial fraud: attackers use stolen credentials to access banking or e commerce accounts to make unauthorized transactions  
- Lateral movement: attackers move within a network using stolen credentials to access more systems and increase privileges  
- Selling credentials: stolen credentials are often sold on underground markets for further misuse  

---

### How This Works 
```
- attacker gets list of usernames (from leaks, emails, or guessing)
- creates a list of possible passwords or uses leaked credentials
- runs automated tools to try login combinations
- keeps sending login requests repeatedly
- successful login gives attacker access to the account
```

### Common Attacks 

### Brute Force Attack 
- brute force is a method where an attacker tries multiple password combinations repeatedly on a single account until the correct password is found  

---

### Password Spraying 
- password spraying is a method where an attacker tries a few commonly used passwords across many user accounts to avoid account lockouts  

---

### Credential Stuffing 
- credential stuffing is a method where an attacker uses previously leaked or stolen username and password combinations to gain access to accounts on different systems  

---

### Prevention 
- Avoid password reuse: use different passwords for different accounts to reduce risk  
- Create strong passwords: use a mix of letters, numbers, and symbols  
- Change passwords regularly: update passwords periodically and use a password manager if needed  

- Multi factor authentication:  
MFA adds an extra layer of security by requiring additional verification such as a code, device, or biometric check. This makes access harder even if credentials are stolen  