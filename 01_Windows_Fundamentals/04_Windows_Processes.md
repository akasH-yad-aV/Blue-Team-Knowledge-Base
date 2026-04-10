# Windows Processes

## What is a Process
A process is a program under execution. When you 
run any program, Windows does not just run the 
.exe file directly ,it creates an isolated 
environment called a process, loads the .exe 
into it, and then executes it.

### What that environment includes
- Virtual memory space (each process gets its own)
- Threads for execution
- Handles (file, registry, network)
- Security context (which user is running it)

---

## Why Processes Matter
The main purpose of a process is isolation. One 
process cannot directly access or mess with 
another process's memory. If one process crashes, 
the rest of the system survives. The OS can 
start, stop, and monitor each process independently.

---

## PID (Process ID)
Every process gets a unique number called a PID 
assigned by the OS. Windows uses this to:
- Track which process is which
- Manage and control execution
- Kill or pause a specific process

---

## Threads
A thread is the smallest unit of execution inside 
a process. One process can have multiple threads 
running at the same time.

```
Process
|----> Thread 1
|----> Thread 2
|----> Thread 3
```
**SOC relevance:** Attackers use a technique 
called process injection — they inject malicious 
code into a legitimate process and run it as 
a thread inside that process. For example, 
malicious code injected into explorer.exe runs 
as a thread inside it, making it blend in with 
legitimate activity and evade detection.

---

## Parent Child Relationship
Every process is created by another process. 
The process that creates is called the parent 
and the one created is called the child.

```
explorer.exe (parent)
        |
        |
        V
 notepad.exe (child)
```
Every process stores its parent's PID, called 
the PPID (Parent Process ID). There is always 
a parent, no process appears out of nowhere.

**SOC relevance:** Unexpected parent child 
relationships are one of the strongest indicators 
of malicious activity. A normal process spawning 
an unexpected child is always worth investigating.


---

## Windows Core Processes
These are essential system processes that Windows 
runs automatically. They manage critical functions 
like booting, security, user sessions, and service 
control. They are the backbone of Windows stability 
and the first place attackers try to abuse or 
impersonate.

### 3. smss.exe (Session Manager Subsystem)

- First user mode process created by Windows
- Started by the System process (PID 4)
- Responsible for creating user sessions 
  and loading the Windows environment
- Starts csrss.exe and winlogon.exe then 
  exits after sessions are created
- Only one master instance runs at a time

**Normal behavior:**
- Parent is always System (PID 4)
- Located at C:\Windows\System32\smss.exe
- Runs as SYSTEM
- Only one instance (child instances 
  exit after session creation)

**Abnormal behaviour:** A second persistent instance 
of smss.exe or one running from any location 
other than System32 is a strong indicator of 
malware impersonating a legitimate process.

---

### wininit.exe (Windows Initialization Process)
- System process responsible for starting 
  critical services after boot
- Sets up the environment for system services
- Initiates other core processes
```
wininit.exe
|——→ services.exe
|——→ lsass.exe
|——→ lsm.exe
```

**Normal behavior:**
- Parent is always smss.exe
- Only one instance expected
- Runs as SYSTEM
- Located at C:\Windows\System32\wininit.exe

**Abnormal behavior:**
- A second instance or one running from any 
  location other than System32 is a strong 
  indicator of masquerading
- If it spawns any process other than 
  services.exe, lsass.exe, or lsm.exe that 
  is a strong indicator of system level 
  compromise and dangerous as it run as system

---

### services.exe (Service Control Manager)
- Core Windows process that manages starting, 
  stopping, and interacting with Windows services
- Acts as the parent for most svchost.exe 
  instances on the system

**Normal behavior:**
- Parent is always wininit.exe
- Runs as SYSTEM
- Only one instance expected
- Located at C:\Windows\System32\services.exe
- Should only spawn svchost.exe as children

**Abnormal behavior:**
- A second instance or one running from 
  any location other than System32 indicates 
  malware masquerading as a legitimate process
- If the parent is anything other than 
  wininit.exe then that is redflag as it never should happen 
- Spawning unexpected child processes other 
  than svchost.exe indicates malicious behavior

**Attacker Aim** 
-  Attackers abuse services 
by creating a fake service that points to their 
malware. When the system boots, the service 
starts automatically giving the attacker 
persistent access without needing to manually 
run anything.

---

### lsass.exe (Local Security Authority Subsystem)
- Core Windows process responsible for security 
  policy enforcement, user authentication, 
  and credential management
- Verifies every login attempt on the system
- Stores authentication data in memory including 
  Kerberos tickets and NTLM hashes
- Handles password changes and generates 
  security event logs

- Verifies user login credentials
- Enforces local security policies
Stores credentials in memory
- Generates Event ID 4624 (success)
and 4625 (failure) on every login attempt


**Normal behavior:**
- Parent is always wininit.exe
- Only one instance expected
- Runs as SYSTEM
- Located at C:\Windows\System32\lsass.exe
- Always running 

**Abnormal behavior:**
- A second instance of lsass.exe is strong indicator of suspicious activiry. Only one should 
  ever exist
- Running from any location other than 
  System32 is a strong indicator of 
  a masquerading attack
- Any unexpected child processes spawned 
  from lsass.exe indicate compromise

**Attackers Aim** lsass.exe is the most 
targeted process in Windows environments. 
Because it stores passwords, NTLM hashes, 
and Kerberos tickets in memory, attackers 
use tools like Mimikatz to dump credentials 
directly from it. If an attacker dumps lsass 
they can move laterally across the entire 
network without needing to crack a single 
password. This is why lsass access should 
always be monitored and why tools like 
Windows Credential Guard exist specifically 
to protect it.


