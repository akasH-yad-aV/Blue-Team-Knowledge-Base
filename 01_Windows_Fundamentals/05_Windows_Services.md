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

---

### What SCM Does
- Starts services in the correct order based on their configuration
- Tracks the current state of each service
- Sends control commands such as start, stop, pause, and continue
- Handles failure recovery (e.g., restarting a crashed service)