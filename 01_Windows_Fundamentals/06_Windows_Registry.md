# Windows Registry 

## What is the Registry
- The registry is a hierarchical database that stores system and application configuration.
- It stores:
  - system configuration 
  - user settings 
  - software configurations 
  - startup behaviour 

**Analogy:-**
- The registry is structured exactly like a file system, but instead of storing files and folders, it stores *configuration data*.  

---

### Purpose of Registry 
- Windows consists of programs, multiple user preferences, hardware drivers, and services — all of which need a place to store configuration.
- Before the registry (in early Windows versions), programs used `.ini` files scattered across the disk, making the system insecure, slow, and unorganized.
- This problem was solved by creating a centralized, hierarchical database called the registry.
- The registry is also a critical target for attackers because it provides persistence and configuration control.

---

### Where the Registry is Located on Disk 
- The registry is a collection of binary files called *hives* stored on disk.

| Hive Name     | Path                                                                 |
|--------------|----------------------------------------------------------------------|
| SYSTEM       | C:\Windows\System32\config\SYSTEM                                    |
| SOFTWARE     | C:\Windows\System32\config\SOFTWARE                                  |
| SECURITY     | C:\Windows\System32\config\SECURITY                                  |
| SAM          | C:\Windows\System32\config\SAM                                       |
| DEFAULT      | C:\Windows\System32\config\DEFAULT                                   |
| NTUSER.DAT   | C:\Users\<username>\NTUSER.DAT                                       |
| UsrClass.dat | C:\Users\<username>\AppData\Local\Microsoft\Windows\UsrClass.dat     |

*Note:* These files are locked while Windows is running, but forensic tools (like Autopsy, FTK, or reg.exe with shadow copies) can read them offline.

---

## The Logical Structure 
- The registry is a tree, exactly like a filesystem.



```
Root Key (Hive)
    └── Key (like a folder)
            └── Subkey (subfolder)
                    └── Value (like a file — holds actual data)

```
---
### The Root Keys 
- "HKEY" means *handle to a key*.
- Programs use this handle to interact with registry keys programmatically.
---
### Key 
- A key is the registry equivalent of a folder.
- It is purely a container and does not hold data directly. Its job is to organize items inside it.

---

### Subkey
- A subkey is simply a key that lives inside another key.
- There is no technical difference between a key and a subkey.

---

### Properties of Keys 
- *Name:* Label of the key, like a folder name (e.g., Run, CurrentVersion, Services)

- *LastWriteTime:* A 64-bit FILETIME timestamp  
  - Records the last time the key or any value inside it was modified  
  - This is the only timestamp available in the registry  

- *Content:*  
   - more subkeys  
   - values (actual data)  
   - both can exist inside the same key  

---

### The Path Notation 
- Just like a filesystem path uses backslashes, the registry does too:


```
HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run
 │        │         │        │         │        │
Root    Subkey   Subkey   Subkey    Subkey    Subkey

```
### Values
- values are the equivalent of files , they hold actual data.
- a value has three components:

```
VALUE

Name ---> "MyProgram"
Type ---> REG_SZ
Data ---> "C:\Users\John\AppData\malware.exe"

```

---

### Value Types

- **REG_SZ** (simple strings): stores plain text  
  - *Security Context:* Most Run key entries use this. If a REG_SZ value in a persistence location points to Temp, AppData, or an unusual path, it is suspicious. Legitimate software usually points to Program Files.

---

- **REG_EXPAND_SZ** (expandable strings): similar to REG_SZ but contains environment variable placeholders that Windows expands at runtime  
  - Example: `%SystemRoot%` → `C:\Windows`  
  - *Security Context:* Attackers use this for evasion. Instead of hardcoding paths like `C:\Users\Bob\evil.exe`, they use `%APPDATA%\evil.exe`. SIEM tools may miss this because the raw value does not contain the full expanded path.

---

- **REG_DWORD**: stores a 32-bit integer (displayed as hex in regedit)  
  - Used like an on/off switch  

 ```
  0x00000000  =  OFF / Disabled / False / No
  0x00000001  =  ON  / Enabled  / True  / Yes
  
  ```

  - *Security Context:* Changing a REG_DWORD value (e.g., from 1 to 0) can silently disable security features.

---

- **REG_QWORD**: similar to REG_DWORD but stores a 64-bit integer (less common)  
  - *Security Context:* Often used for timestamps (FILETIME format), useful in forensic timeline analysis  

---

- **REG_BINARY**: stores raw bytes; meaning depends on the application  
  - *Security Context:* Common hiding place for malware. Attackers may store encrypted shellcode, configuration data, or encoded command-and-control addresses. Data appears meaningless in regedit but can be decoded by malware.

---

- **REG_MULTI_SZ** (array of strings):  
- Stores multiple strings in a single value, separated by null terminators, ending with a double null  
  - *Security Context:* Used in the Services hive for dependencies. Malware may abuse this to inject itself into a dependency chain  

---

- **REG_NONE** (no defined type):  
- Rarely used; indicates no defined data type and may suggest unusual or legacy entries  

---

### Analogy with window 
```

Filesystem          |    Registry
--------------------|--------------------
Drive (C:\)         |    Root Hive (HKLM)
Folder              |    Key
Subfolder           |    Subkey
File                |    Value
File Content        |    Value Data

```
---

## Registry Hives 
- there are total 5 root keys or hives 
- they are at the absolute top of the hierarchy 

**Not All Keys Are Real**

- some of these root keys are virtual, they do not have their own hive file on disk. they are pointers or merged views of other real hives


```
HKEY_LOCAL_MACHINE     Real          (HKLM)
HKEY_CURRENT_USER      Virtual       (HKCU)
HKEY_USERS             Real          (HKU)
HKEY_CLASSES_ROOT      Virtual       (HKCR)
HKEY_CURRENT_CONFIG    Virtual       (HKCC)
```
 
 **Why does this matter**
 - when doing forensic on a dead disc or offline hives HKCU ,HKCR and HKCC do not exist as file . we have to know where their data actually lives to find it .



 
**Why does this matter**
- when doing forensics on a dead disk or offline hives, HKCU, HKCR and HKCC do not exist as files. we need to know where their actual data lives to find them

---

### HKEY_LOCAL_MACHINE (HKLM)
- machine wide settings, applies to all users
- contains hardware configs, installed software, services, drivers
- targeted by attackers for persistence 
 
### HKEY_CURRENT_USER (HKCU)
- settings for currently logged in user only 
- maps directly to that user's NTUSER.DAT file 
- user level persistence and per user malware config lives here

### HKEY_USERS 
- contains subkeys for every user profile on the system by SID
- HKCU is just a shortcut pointing into this hive

### HKEY_CLASSES_ROOT
- file associations and COM object registration 
- a merge of HKLM\SOFTWARE\Classes and HKCU\SOFTWARE\Classes
- COM hijacking lives here

### HKEY_CURRENT_CONFIG (HKCC)
- current hardware profile 
- mostly a pointer into HKLM\SYSTEM\CurrentControlSet\Hardware Profiles

---

## How Windows Uses the Registry (Boot to Login)
- windows actively reads the registry at every stage of startup  to know:
  - what hardware exists
  - what drivers to load
  - what services to start
  - who is logging in
  - what programs to run
  - what policies to enforce
- without the registry, Windows does not know how to start itself

---

### The Full Sequence 

**STAGE 1 :- the very first registry read (bootexecute)**
```
Location:
HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\BootExecute
```
- when: this is the earliest point. the windows kernel has just loaded, no user session, no desktop, no services. almost nothing is running yet 
- what happens here: windows reads the BootExecute value and runs whatever is listed there before anything else happens

*Security context:* this is one of the most powerful persistence locations in the registry. anything placed here runs:
   - before security software loads
   - before antivirus loads
   - before any user logs in 
   - with very high privileges

---

**STAGE 2 :- kernel loads boot drivers (Start = 0)**
```
Location:
HKLM\SYSTEM\CurrentControlSet\Services\
```
- when: immediately after BootExecute. the kernel is initializing and needs drivers to communicate with hardware 
- what happens here: windows reads every subkey under Services and loads entries where Start = REG_DWORD = 0x00000000

*Security context:* a malicious driver here loads before endpoint security solutions. this is rootkit level territory

---

**STAGE 3 :- system drivers load (Start = 1)**

```
Location:
HKLM\SYSTEM\CurrentControlSet\Services\
```
- when: still early. kernel is running and basic hardware is already initialized 
- what happens here: windows loads entries where Start = 1 

*Security context:* still loads before most user space security tools. malicious drivers here are still dangerous

---

**STAGE 4 :- automatic services start (Start = 2)**


```
Location:
HKLM\SYSTEM\CurrentControlSet\Services\
```
- when: boot is complete and the desktop environment is starting 
- what happens here: windows reads service subkeys with Start = 2 and starts them 

*Security context:* very common persistence method. attackers can create malicious services here

---

**STAGE 5 :- user logs in (NTUSER.DAT loads)**


```
Location:
C:\Users\<username>\NTUSER.DAT
         ↓
Loaded into memory as:
HKU\<user SID>
         ↓
Mapped as shortcut to:
HKCU\
```

- when: user logs in and windows loads user specific configuration

- what happens:
  - step 1: windows finds the user's SID (unique identifier)
  - step 2: locates NTUSER.DAT on disk
  - step 3: loads it into HKU\<SID> and maps it to HKCU

```
User's personal software settings
User's desktop preferences
User's recent documents history
User's network drive mappings
User's personal Run keys
User's personal policies
```

*Security context:* each user has their own NTUSER.DAT. this file shows what the user did, what ran, and possible user level persistence

---

**Stage 6 :- Run Keys Execute (Programs Auto-Start)**
```
Locations (in order of execution):

HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run
HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce
HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Run
HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce
```

- when: after login, when desktop is loading

- what happens:
windows reads values inside these keys:

```
HKLM\...\Run

Values:
SecurityHealth  →  C:\Windows\System32\SecurityHealth.exe
OneDrive        →  C:\Program Files\OneDrive\OneDrive.exe
Zoom            →  C:\Program Files\Zoom\Zoom.exe
```

- each entry is executed automatically

- this is why apps like OneDrive or Zoom start at login

*Security context:* one of the most abused persistence locations. simple and reliable

---

### the complete picture


```
POWER ON
    │
    ▼
Stage 1 — BootExecute
    │       Runs disk check and anything else here
    │       Registry: HKLM\...\Session Manager\BootExecute
    │       Timing: Before almost everything
    ▼
Stage 2 — Boot Drivers (Start = 0)
    │       Kernel loads hardware drivers
    │       Registry: HKLM\...\Services\ (Start=0)
    │       Timing: Kernel initialization
    ▼
Stage 3 — System Drivers (Start = 1)
    │       More drivers load
    │       Registry: HKLM\...\Services\ (Start=1)
    │       Timing: Kernel execution
    ▼
Stage 4 — Automatic Services (Start = 2)
    │       All major Windows services start
    │       Registry: HKLM\...\Services\ (Start=2)
    │       Timing: After boot, before login
    ▼
Stage 5 — User Logs In
    │       NTUSER.DAT loaded → becomes HKCU
    │       Registry: C:\Users\<name>\NTUSER.DAT
    │       Timing: At login
    ▼
Stage 6 — Run Keys Execute
            Auto-start programs launch
            Registry: HKLM\...\Run and HKCU\...\Run
            Timing: Desktop appearing
```
