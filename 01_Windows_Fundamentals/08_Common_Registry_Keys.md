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
- *Note:-* every subkey under Services\ is one service or driver and each subkeys has its own value that defines the service entire behaviour

- `imagePath` :- the actual executable or dll that runs 
- `Start` :- when it starts (2 = auto,3 = manual , 4=disabled)
- `Type` :- Service type (10= standalone exe , 20= shared process , 1 = kernel driver)
- `ObjectName`:- the account it run as
- `description`:- human readable description 
- `displayName` :-  the friendly name shown in services.msc

- **note**:- when window scm reads ` HKLM\SYSTEM\CurrentControlSet\Services` ,for every service where start = 2 SCM launches imagepath , executable runs no user interaction needed , no login needed

### Security Context/How Attacker Abused this
- attacker with admin privileges can create new subkey under `Services\` will all right and imagepath to theri malicous services , on next reboot it run as system and can dowload payload or execute malware , this gives attacker persistence
- instead of creating something new ,attacker may choose a stealthy approach like modifies imagpath of existing legitimate but currently disabled service to their malicious service or process and start = 2 which make it run at after boot without any interaction 
- there is also another way , many services dont have thier own .exe , they are actually dlls  that run inside svchost.exe inssdie parameter subkey `HKLM\SYSTEM\CurrentControlSet\Services\wuauserv\Parameters\ServiceDll` , attacker can change this serviedll path to their malicous dll , the process list still show svchost.exe looking completly normal. this is dangerous as they give attacker to execute malware without noise with stealth and this whole thing refer to dll hijacking