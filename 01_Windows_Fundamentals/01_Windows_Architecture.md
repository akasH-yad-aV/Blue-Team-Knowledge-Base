# Windows Architecture

## What is it?
Windows OS divides into layers (User Mode and Kernel Mode) to ensure applications don't crash the entire system and maintain security.

---

## Core Components
- **User Mode** - Where applications run (Chrome, Notepad, etc.)
- **Kernel Mode** - Where OS runs with full system control
- **System Calls** - Interface between user mode and kernel mode
- **Device Drivers** - Software to manage hardware devices

---

## User Mode

**Characteristics:**
- Restricted execution space for applications
- Limited memory access (only own memory)
- Cannot directly access hardware

**Why Isolation?**
- If Chrome crashes, only Chrome stops (not entire system)
- Prevents malware from accessing entire system
- Protects system stability

---

## Kernel Mode

**Characteristics:**
- Privileged execution space for OS core
- Full memory and hardware access
- Direct control over all resources

**Core Functions:**
- Thread scheduling - allocate CPU time to processes
- Interrupt handling - respond to hardware events
- Process management - create/terminate processes
- Memory management - allocate and protect memory
- Context switching - switch between processes

---

## System Calls

**Definition:** Controlled mechanism for applications to request kernel services

**Key Point:** User mode cannot directly access kernel. System calls are the only legal entry point.

**Common System Calls:**
- OpenFile() - open a file
- ReadFile() / WriteFile() - read/write from disk
- CreateProcess() - start a new process
- AllocateMemory() - request RAM
- SendNetworkPacket() - send network data

**Flow:**
```
Application (user mode) 
    ↓
Calls System Call
    ↓
Kernel validates permissions
    ↓
Kernel executes action
    ↓
Returns result to application
```

---

## Device Drivers

**Definition:** Software that enables OS to communicate with hardware devices

**Key Points:**
- Run in kernel mode (require direct hardware access)
- Examples: GPU driver, keyboard driver, network driver
- Vulnerable driver = system compromise or crash
- Only signed drivers are trusted (Windows signature verification)

---

## Ring Levels (Privilege Hierarchy)

| Ring | Mode | Privilege | Examples |
|------|------|-----------|----------|
| **Ring 0** | Kernel Mode | Maximum | OS kernel, drivers, system services |
| **Ring 3** | User Mode | Restricted | All user applications |

---

## How It Works Together

```
User Application (Ring 3)
        ↓
Needs resource (file, network, memory)
        ↓
Makes System Call
        ↓
Kernel (Ring 0) receives request
        ↓
Validates: Is application allowed?
        ↓
Device Driver (if needed) executes on hardware
        ↓
Kernel returns result
```

---

## Security: Privilege Escalation

**Attacker Goal:** Move from Ring 3 (user mode) to Ring 0 (kernel mode)

**Why?** At Ring 0, attacker owns the entire system - can install rootkit, disable antivirus, steal data, hide malware.

**How Attack Works:**
1. Attacker runs malware in user mode
2. Finds kernel vulnerability (0-day exploit)
3. Exploits via system call to escape user mode
4. Gains kernel mode access
5. Full system compromise

**Defense:**
- Keep Windows updated (patches vulnerabilities)
- Don't run untrusted programs
- Use antivirus/antimalware
- Monitor system calls via SIEM

---

## Important Windows Event IDs

| Event ID | Meaning | Indicates |
|----------|---------|-----------|
| **4688** | Process created | New process launched |
| **4672** | Admin privileges assigned | Privilege escalation |
| **4625** | Failed login | Brute force attempt |
| **7045** | New service installed | Backdoor/malware as service |

---

## Key Points Summary

| Aspect | User Mode | Kernel Mode |
|--------|-----------|------------|
| Trust | Low (untrusted) | High (trusted) |
| Hardware Access | No | Yes |
| Memory Access | Limited | Full |
| Can Crash System | Only itself | Entire system |
| Security Risk | Moderate | Critical |

---

## For SIEM/Security

**Threats to Monitor:**
- Privilege escalation attempts (4672 events)
- Suspicious process creation (4688 events)
- Unsigned driver installation
- New service creation by unusual processes
- Unusual system call patterns

**What to Protect:**
- Kernel from exploitation
- Drivers from vulnerability
- System calls from abuse
