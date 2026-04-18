# Common Registry Keys With Thier Abuse


## Startup / Persistence 
- Windows reads these registry keys at startup and executes all the entries inside  

### Keys

```
HKCU\Software\Microsoft\Windows\CurrentVersion\Run
HKLM\Software\Microsoft\Windows\CurrentVersion\Run
HKCU\Software\Microsoft\Windows\CurrentVersion\RunOnce
HKLM\Software\Microsoft\Windows\CurrentVersion\RunOnce
```

- HKCU executes entries for the current user, while HKLM executes for the whole system  

### Why they matter
- Windows and software need a mechanism to run essential processes after boot without any user interaction, such as system components or legitimate software updates. Hence, Windows stores configuration for these processes in these keys  

### Security Context:-
- These keys are most targeted by attackers as they run at startup, giving them persistence  
- Attackers modify these registry keys to point to their malware, resulting in persistence  
- If an attacker sets their malware in `RunOnce`, it will execute only once and then remove itself. This is dangerous because attackers can download payloads stealthily  

---

## Services Key 

- This key stores the configuration of every service and driver in the Windows system  
- When Windows boots, the Service Control Manager reads this key and decides what to start, when to start, and how to run it  


```
HKLM\SYSTEM\CurrentControlSet\Services\
    ├── Spooler          ← Print Spooler service
    ├── wuauserv         ← Windows Update
    ├── Dnscache         ← DNS Client
    ├── MaliciousSvc     ← could be anything an attacker planted
    └── ...
```

- *Note:-* every subkey under `Services\` represents one service or driver, and each subkey has its own values that define the service's behavior  

- `ImagePath` :- the actual executable or DLL that runs  
- `Start` :- when it starts (2 = auto, 3 = manual, 4 = disabled)  
- `Type` :- service type (10 = standalone EXE, 20 = shared process, 1 = kernel driver)  
- `ObjectName` :- the account it runs as  
- `Description` :- human readable description  
- `DisplayName` :- the friendly name shown in services.msc  

- **Note:-** when Windows SCM reads `HKLM\SYSTEM\CurrentControlSet\Services`, for every service where `Start = 2`, SCM launches the `ImagePath`. The executable runs without user interaction or login  

**Security Context / How attackers abuse this**
- An attacker with admin privileges can create a new subkey under `Services\` with proper values and set `ImagePath` to their malicious service. On the next reboot, it runs as SYSTEM and can download payloads or execute malware, giving persistence  
- Instead of creating something new, an attacker may take a stealthy approach by modifying the `ImagePath` of an existing but disabled service and setting `Start = 2`, making it run after boot without interaction  
- Some services do not have their own `.exe`; they are DLLs that run inside `svchost.exe` using the path:
  `HKLM\SYSTEM\CurrentControlSet\Services\wuauserv\Parameters\ServiceDll`  
  An attacker can change this `ServiceDll` path to a malicious DLL. The process list still shows `svchost.exe`, making it look normal. This is stealthy and relates to DLL hijacking  

  ---

## BootExecute

**key:-**
```
HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\BootExecute
```

- BootExecute runs after the kernel loads but before the Session Manager finishes, which means it starts before services, before antivirus, before EDR agents, and before almost any security component is running  
- `smss.exe` is the first user mode process Windows creates, and one of its tasks is to read this key and execute whatever is listed here  

### Default Value 
- `autocheck autochk *`  
- This value is used by the Windows disk checking utility. `chkdsk` runs if Windows detects the file system was not properly shut down, and `*` means check all drives. This is completely normal  

### How this can be abused 
- An attacker can add their payload as a second entry. Windows will see multiple entries and execute all of them  
- **Why this is dangerous:-** most security tools load as services, and services start after BootExecute. This gives malware a window where no monitoring or defense is active. Malware can modify files, patch memory structures, and establish deeper persistence. This technique is often associated with rootkits and bootkits  
- An attacker can also abuse this by replacing `autochk.exe` in `System32` with a malicious binary  

- **Note:** this key should normally contain only one value: `autocheck autochk *`. Anything extra is suspicious  

