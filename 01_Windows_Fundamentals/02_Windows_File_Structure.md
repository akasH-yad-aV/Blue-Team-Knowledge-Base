# Windows File Structure 

## What is it?
The Windows file structure is a hierarchical, tree-like system that organizes files and directories on storage devices.

---

## Important Windows Directories

### C:\ (Root Directory)
- Main starting point of the file system  
- Contains all major system folders  

---

### C:\Windows
- Contains core OS files  
- Includes system executables, drivers, and DLL files  

---

### C:\Windows\System32
- Critical system directory  
- Contains essential executables and DLLs  
- Used by the OS for core operations  
- Despite the name, stores 64-bit files (on 64-bit systems)  

---

### C:\Windows\SysWOW64
- Contains 32-bit system files  
- Used for compatibility with 32-bit applications on 64-bit Windows  

---

### C:\Program Files
- Stores installed 64-bit applications  

---

### C:\Program Files (x86)
- Stores installed 32-bit applications  

---

### C:\Users
- Contains user profiles  
- Stores personal files like Desktop, Documents, Downloads  

---


## AppData Directory

### Path
C:\Users\<username>\AppData

### What is it?
- Stores application-specific data and settings  
- Hidden by default 
- 

### Subfolders

#### Local
- Stores machine-specific data  
- Not synced across systems  
- Example: cache, temp files  

#### Roaming
- Stores user-specific data  
- Can be synced in domain environments  
- Example: browser settings, profiles  
- Attacker abuse: malware commonly drops persistence scripts here because user writable location (common entry point for malware)

#### LocalLow
- Used by low-permission applications  
- Example: sandboxed apps  

---

## Temp Directory

### Common Paths
- C:\Windows\Temp  
- C:\Users\<username>\AppData\Local\Temp  

### Purpose
- Stores temporary files created by system and applications  

### Key Points
- Safe to clean (most of the time)  
- Often abused by malware to store payloads  

---

## Window file structure
```
C:
├── Windows
│ ├── System32
│ ├── SysWOW64
│ ├── Temp
│ └── Logs
│
├── Program Files
│ ├── Common Files
│ └── WindowsApps
│
├── Program Files (x86)
│ ├── Common Files
│ └── Internet Explorer
│
├── Users
│ ├── Public
│ └── YourUsername
│ └── AppData
│       ├── Local
│       ├── LocalLow
│       └── Roaming
│  
|
├── PerfLogs
├── Recovery
└── System Volume Information\

```

## Important Windows File Types

### .exe
- Executable files  
- Runs programs  

### .dll
- Dynamic Link Libraries  
- Shared code used by multiple programs 
- Loaded by process , provides functions
- Instead of process writing thier own code(eg. for writing ,    opening file etc) window provide it via dlls


### .sys
- System/driver files  
- Used by kernel and hardware drivers  

### .bat
- Batch scripts  
- Executes command-line instructions 
- Soc relevance: commonly used in malware for persistence and lateral movement 

### .cmd
- Similar to .bat, but newer scripting format  

### .tmp
- Temporary files  

### .log
- Log files used for debugging and monitoring  
- Soc relevance: primary source of evidence during any investigation 
-

---

## File System Basics (NTFS)

- Default file system in Windows  
- Supports permissions and access control  
- Uses journaling for reliability 
- SOC relevance: NTFS log file creation , modification , and access time , critical for building attack timelines during incident response 

### Alternate Data Streams (ADS)
- NTFS allows files to have hidden streams 
  attached to them
- A file called invoice.pdf can secretly 
  carry malicious code inside it
- Attackers use this to hide payloads 
  inside legitimate looking files
- Command to check: dir /r

### File Permissions (ACLs)
- Every file and folder has an Access 
  Control List defining who can read, 
  write, or execute it
  - **READ**: allow user to view content of files or folder .
  - **WRITE** grants permission to add new data or make changes to an existing file.For folders it allow to create new files or subfolders .
  - **EXECUTE**: permits user to run executables file or applications , for folder it allow navigating into the folder
  - **MODIFY**: combines read , write and delete permissions 
  - **FULL CONTROL**: provides complete access to folder or file , including all above rights 
- SOC relevance: misconfigured permissions 
  are one of the most common ways attackers 
  escalate privileges

### Key Features
- File permissions (ACLs)  
- Encryption (EFS)  
- Compression  
- Disk quotas  

---

## How It All Connects

1. OS files stored in C:\Windows  
2. Applications installed in Program Files  
3. User data stored in C:\Users  
4. Apps store configs in AppData  
5. Temporary operations use Temp folders  

---

## Key Points
- System files → C:\Windows  
- Apps → Program Files  
- User data → Users  
- App settings → AppData  
- Temp data → Temp folders  

---

## Security Insight

- Attackers often use Temp and AppData to hide files  
- Malicious executables usually appear as .exe or .dll  
- Abusing legitimate directories helps malware avoid detection  