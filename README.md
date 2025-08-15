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

## Build

**Prereqs:** Windows 10/11, x64, MSVC (Visual Studio Developer Command Prompt)

```bat
cl /nologo /W4 /EHsc /DUNICODE /D_UNICODE SigScope.cpp ^
  /link Crypt32.lib Wintrust.lib Psapi.lib
