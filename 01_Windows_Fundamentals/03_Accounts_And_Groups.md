# Windows Accounts and Groups

## What is a User Account
A user account is a unique identity that allows 
an individual to access and interact with a Windows 
system. It acts as a container for user specific 
settings, permissions, and access rights that define what a user can and cannot do on the system.

---

## Types of Accounts

### Standard User
- Limited privileges
- Cannot modify system files or install software
- Cannot make changes that affect other users
- Default account type for regular employees 
  in an organization

### Administrator
- Elevated privileges
- Can install software, modify system files, 
  and manage other user accounts
- SOC relevance: if an administrator account 
  is compromised, the attacker can install 
  payloads, create malicious processes, modify 
  system files, and create new accounts to 
  maintain persistence

### SYSTEM
- Not a human user account
- Highest privileged account in Windows
- Used by the operating system itself to run 
  background processes and core services
- No password, cannot be logged into directly
- SOC relevance: malware that escalates to 
  SYSTEM level has unrestricted access to 
  everything on that machine ,this is the 
  worst case scenario in an investigation

---

## Local Accounts vs Domain Accounts

### Local Accounts
- Exist only on the specific machine they 
  were created on
- Credentials stored locally in SAM 
  (Security Account Manager) database
- Authentication handled by NTLM protocol
- No access to network resources by default
- Example: the Administrator account on 
  a standalone PC

### Domain Accounts
- Created and managed centrally in 
  Active Directory
- Work across all machines in the domain
- Credentials stored on Domain Controller
- Authentication handled by Kerberos protocol
- Example: company employee accounts managed 
  by IT department

---

## Authentication Protocols

### NTLM (NT LAN Manager)
- Older authentication protocol
- Used for local account authentication
- Works by challenge and response method ,
  server sends a challenge, client responds 
  with a hashed version of the password
- SOC relevance: attackers exploit NTLM 
  using Pass the Hash attack — they steal 
  the password hash and use it directly 
  without knowing the actual password

### Kerberos
- Modern and more secure protocol
- Default authentication for domain accounts
- Uses tickets instead of passwords ,
  user authenticates once and receives 
  a ticket that grants access to resources
- SOC relevance: attackers exploit Kerberos 
  using techniques like Pass the Ticket, 
  Golden Ticket, and Kerberoasting to 
  move laterally across the domain

---

## What Happens if a Domain Account is Compromised

This is the most critical scenario in enterprise 
security. Unlike a local account compromise which 
is limited to one machine, a domain account 
compromise can affect the entire organization.

**Standard domain account compromised:**
- Attacker gains access to all resources 
  that account has permission to access
- Can move laterally to other systems 
  in the network
- Can access shared drives and internal services

**Domain Admin account compromised:**
- Complete control over the entire domain
- Can create new accounts and add them 
  to any group
- Can access every machine in the organization
- Can dump credentials of all domain users
- Essentially game over for the organization
  until the domain is rebuilt

---

