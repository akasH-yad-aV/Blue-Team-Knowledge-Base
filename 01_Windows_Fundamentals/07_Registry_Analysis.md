# Registry Analysis 

## How to Read and Understand Registry Path 

- the registry path is not random, it is structured and meaningful  
- every part of the path gives a clue about what it does  


### Path Structure  
```
HKLM \ SOFTWARE \ Microsoft \ Windows \ CurrentVersion \ Run
 │        │           │          │            │            │
 │        │           │          │            │            │
Who       What        Who        What         Where      Doing What
owns it   category   made it    it's for     in version   action
```


### Reading Framework 
- this framework helps understand the purpose of a registry path  

```
STEP 1: Read the Root Key
         HKLM = machine-wide, persists for all users
         HKCU = personal to one user
         HKU  = all users, identified by SID

STEP 2: Read the second-level hive
         SOFTWARE = installed application configs
         SYSTEM   = boot-level, OS internals, services
         SECURITY = policies and permissions (restricted)
         SAM      = local accounts and credentials (restricted)

STEP 3: Read the vendor/owner key
         Microsoft = Windows built-in
         Adobe     = Adobe product
         Unknown name = could be legitimate app or malware

STEP 4: Read the functional key name
         Services   = service definitions
         Run        = auto-execute on startup
         Policies   = security and behavioral settings
         Enum       = hardware device records
         NetworkList= network connection history

STEP 5: Read the values
         Name = what this setting is called
         Type = what kind of data it is
         Data = the actual configuration value
```

### Example :- The Run Key 

- the key :-

```
Key: HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run

Values inside:
┌──────────────────┬────────────┬─────────────────────────────────────┐
│ Name             │ Type       │ Data                                │
├──────────────────┼────────────┼─────────────────────────────────────┤
│ SecurityHealth   │ REG_SZ     │ C:\Windows\System32\SecurityHealth  │
│ OneDrive         │ REG_SZ     │ C:\Program Files\OneDrive\OneDrive  │
│ Zoom             │ REG_SZ     │ C:\Program Files\Zoom\Zoom.exe      │
└──────────────────┴────────────┴─────────────────────────────────────┘
```
### Step by Step Interpretation

```
HKLM           - HKEY_LOCAL_MACHINE
               - This setting applies to the ENTIRE machine
               - Every user who logs in is affected
               -  Does NOT matter who is logged in

SOFTWARE       - We are in the SOFTWARE hive
               - This section deals with installed applications
                 and their configurations

Microsoft      - This key was created by Microsoft
               - (Vendors create their own named folder here)

Windows        - Specifically the Windows OS product

CurrentVersion  - The currently installed version of Windows
                - (avoids hardcoding version numbers like "10.0")

Run            - THIS is the action key
               - Whatever VALUE exists inside this key
               - WILL BE EXECUTED when Windows starts

Value          - Value name :- the label or indetifier
               - Value Type :- REG_SZ = Plain Text Path
               - Value Data :- the actual executables that will 

```

### Summary 
this registry key is read at startup and executes:
- SecurityHealth.exe  
- OneDrive  
- Zoom.exe  
---

