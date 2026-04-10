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

**SOC relevance:** A second persistent instance 
of smss.exe or one running from any location 
other than System32 is a strong indicator of 
malware impersonating a legitimate process.