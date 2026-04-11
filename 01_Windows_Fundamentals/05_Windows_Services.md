# Windows Service

## What is a Windows Service?
- A Windows service is a program designed to run in the background without user interaction.
- It does not require a user to be logged in.
- It can start automatically, continue running in the background, and is managed by the operating system.

---

## Why Services Exist
- Some tasks need to run continuously regardless of user login, such as antivirus scanning, network listeners, print spoolers, and Windows Update.
- User-level applications depend on an active user session, so they cannot handle these tasks reliably.
- Services solve this problem by running independently of user sessions.

- The key difference between a service and a normal background process is its relationship with the Service Control Manager (SCM).
- A service must follow a specific communication model with the SCM to function properly.
- Any program can run in the background, but only programs that interact correctly with the SCM are considered true services.

---

## Service Control Manager (SCM)
- The SCM is responsible for managing all services on a Windows system.
- It runs as services.exe under the SYSTEM account.
- It is started early in the boot process by wininit.exe.

---

### RPC Communication
- The SCM communicates using RPC (Remote Procedure Call).
- Tools like services.msc or commands like sc stop <service> interact with the SCM through RPC.
- All service management operations go through the SCM.



### What SCM Does
- Starts services in the correct order based on their configuration
- Tracks the current state of each service
- Sends control commands such as start, stop, pause, and continue
- Handles failure recovery (e.g., restarting a crashed service)

---

## The SCM and Service Handshake 
- When the SCM launches a service, it does not just start the process . It expects the service to register itself almost immediately, within about 30 seconds by default.
- The sequence looks like this:
     - SCM launches the service executable
     - the service calls `StartServiceCtrlDispatcher()` to connect back to the SCM
     - the service registers its main function `ServiceMain`, where its actual logic runs
     - the service registers a control handler (`HandlerEx`) that the SCM calls when sending commands like stop or pause
     - the service reports its current state back using `SetServiceStatus` throughout its lifecycle

*NOTE:* 
- If a service fails to register in time, the SCM marks it as failed and terminates it. This matters from a detection standpoint because malware trying to impersonate a legitimate service sometimes fails at this stage.
- This timeout behavior can be abused or monitored for detecting poorly implemented or malicious services



---

## Service Types
- Service type refers to the physical structure of the service, not what it does.

### Own Process (SERVICE_WIN32_OWN_PROCESS)
- The service runs as its own dedicated process. One service, one executable, one process. Clean isolation. 
- If something goes wrong with this service, it does not directly affect others.
- Example: Windows Defender's engine (`MsMpEng.exe`) runs as its own process.

### Shared Process (SERVICE_WIN32_SHARE_PROCESS)
- Multiple services share a single host process. This is what `svchost.exe` is used for.
- If you open Task Manager, you will see multiple instances of `svchost.exe` running at the same time, each one hosting one or more services and each instance is associated with a specific service group . 

### Kernel Driver (SERVICE_KERNEL_DRIVER)
- This is a driver running in kernel mode (ring 0). The service infrastructure manages it, but it operates at the lowest level of the OS. 
- Examples include disk drivers, network drivers, and antivirus kernel components.

### File System Driver (SERVICE_FILE_SYSTEM_DRIVER)
- A specialized kernel driver that handles filesystem operations. The same kernel mode rules apply as above.

---
## Service Lifecycle
- A service moves through different states managed by the SCM:
  - Start Pending → Running
  - Running → Stop Pending → Stopped
- Services must regularly report their state using `SetServiceStatus`
- Failure to report state properly can cause the SCM to terminate the service


---

## Service Configuration
- Services are defined in the Windows Registry under:
  `HKLM\SYSTEM\CurrentControlSet\Services`
- Important configuration fields include:
  - ImagePath (executable path)
  - Start type (Auto, Manual, Disabled)
  - Service account (SYSTEM, LocalService, NetworkService)


  ---
  ## Service Start Types
- Automatic → starts at boot
- Manual → starts on demand
- Disabled → cannot be started


---

## Service Security Context
- Services run under specific accounts:
  - SYSTEM → highest privilege
  - LocalService → limited privileges
  - NetworkService → limited but can access network
- The privilege level determines the impact if a service is compromised


---
## Service Abuse & Persistence
- Attackers can create new services to maintain persistence
- Modify existing services to execute malicious binaries
- Change `ImagePath` in registry to point to malicious code
- Use services to gain SYSTEM-level execution

## Key Points to Remember

- A Windows service runs in the background and does not require user interaction
- All services are managed by the Service Control Manager (`services.exe`)
- Services must communicate with the SCM using a defined handshake (`StartServiceCtrlDispatcher`)
- Services must report their state using `SetServiceStatus`, or they may be terminated
- Service configuration is stored in the registry at `HKLM\SYSTEM\CurrentControlSet\Services`
- `ImagePath` defines what executable the service runs
- Services can run under different accounts (SYSTEM, LocalService, NetworkService)
- `svchost.exe` is used to host multiple services in shared processes
- Start types (Automatic, Manual, Disabled) control when a service runs
- Attackers commonly abuse services for persistence or privilege escalation
- Modifying or creating services can lead to SYSTEM-level execution