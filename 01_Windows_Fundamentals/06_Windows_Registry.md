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
