# SigScope

Lightweight Windows **process & module signature scanner** with basic **token privilege** dump.  
Fundamentals-level, single EXE, no drivers.

## Features
- Enumerates running processes (PID, parent, thread count)
- Lists loaded modules (DLL/EXE path)
- Verifies Authenticode signatures via `WinVerifyTrust` (OK/NO)  
- Dumps process token privileges (e.g., `SeDebugPrivilege`)
- Graceful handling for access-denied/PPL processes

> ⚠️ Designed for learning and triage. It **does not** modify the system.


## Run

```bat
SigScope.exe > scan.txt
```

Run from an **elevated** console to enumerate more processes/modules.  
You can also `Tee-Object` in PowerShell to save and view live


```
[+] Process Name: winlogon.exe
  PID = 1712
  Parent Process Name = (n/a)
  Thread Count = 5
  [+] FileName
    [OK ] C:\Windows\System32\KERNEL32.DLL is signed and valid.
    [NO ] C:\Windows\System32\FirewallAPI.dll is not signed or signature format unknown.

  Privileges:
    SeDebugPrivilege
    SeBackupPrivilege
    ...
```

## Limitations
- Some Windows components are **catalog-signed**; basic `WinVerifyTrust` may mark them as “NO”.
- Access to protected processes (PPL) is restricted by design; output may show “Access is denied.”
- Parent process name may be `(n/a)` if the parent handle isn’t openable with limited info.

## Roadmap (nice-to-have)
- Optional **catalog** verification (`CryptCATAdmin*`) to reduce false “unsigned”
- `[UNK]` bucket for indeterminate trust (policy/offline) instead of calling “unsigned”
- `--csv` or `--json` output
- `EnumProcessModulesEx(..., LIST_MODULES_ALL)` and WOW64 awareness
- One-line per-process summary: `OK:x NO:y`


