## Persistence

### T1546.008 Accessibility Features
Atomics: [T1546.008](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1546.008/T1546.008.md)

Detections addition of a debugger process to executables using Image File Execution Options.

```
(RegistryKeyPath ContainsCIS "CurrentVersion\Image File Execution Options" AND RegistryKeyPath ContainsCIS ".exe\Debugger") AND (EventType = "Registry Value Create" OR EventType = "Registry Key Create")
```

### T1098 Account Manipulation
Atomics: [T1098](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1098/T1098.md)


### T1546.010 Application Shimming
Atomics: [T1546.010](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1546.011/T1546.010.md) , 
[T1546.011](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1546.011/T1546.011.md)

Detects application shimming through sdbinst or registry modification.

```
(SrcProcName = "sdbinst.exe" and ProcessCmd ContainsCIS ".sdb") OR ((RegistryKeyPath ContainsCIS "AppInit_DLLs" OR RegistryPath  ContainsCIS "AppCompatFlags") AND (EventType = "Registry Value Create" OR EventType = "Registry Value Modified"))
```

### T1053.002 AT Scheduled Task
Atomics: [T1053.002](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1053.002/T1053.002.md)

Detect interactive process execution scheduled by AT command.

```
TgtProcName = "at.exe" AND TgtProcCmdLine ContainsCIS "/interactive "
```

### T1197 BITS Jobs
Atomics: [T1197](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1197/T1197.md)


### T1176 Browser Extensions
Atomics: [T1176](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1176/T1176.md)


### T1574.012 COR Profiler
Atomics: [T1574.012](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1574.012/T1574.012.md)

Detection of unmanaged COR profiler hooking of .NET CLR through registry or process command.

```
(SrcProcCmdScript Contains "COR_" AND SrcProcCmdScript Contains "\Environment") OR RegistryKeyPath Contains "COR_PROFILER_PATH" OR SrcProcCmdScript Contains "$env:COR_"
```

### T1546.001 Change Default File Association
Atomics: [T1546.001](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1546.001/T1546.001.md)


###  T1574.001 DLL Search Order Hijacking
Atomics: [T1574.001](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1574.001/T1574.001.md)

Detection of DLL search order hijack for AMSI bypass. Search order bypasses can target more than AMSI, so this can be expanded upon greatly by switching the `ContainsCIS` to `In Contains Anycase(dll list)`.

```
(FileFullName ContainsCIS "amsi.dll" AND FileFullName Does Not ContainCIS "System32") AND EventType = "File Creation"
```

### T1574.002 DLL Side-Loading of Notepad++ GUP.exe
Atomics: [T1574.002](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1574.002/T1574.002.md)

Detection for GUP.exe side-loading a dll, where executable has a display name of "WinGup for Notepad++" and has non-standard source process. Keep an eye on Cross Process events or add `AND EventType = "Open Remote Process Handle"` to the query to narrow down target (child) process.

```
TgtProcDisplayName ContainsCIS "WinGup" and SrcProcName Not In ("notepad++.exe","explorer.exe","lsass.exe","csrss.exe","svchost.exe","WerFault.exe")
```

### T1078.001 Enable Guest account with RDP and Admin
Atomics: [T1078.001](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1078.001/T1078.001.md)

Detects enabling of Guest account, adding Guest account to groups, as well as changing of Deny/Allow of Terminal Server connections through Registry changes.

```
(SrcProcCmdLine ContainsCIS "net localgroup" AND SrcProcCmdLine ContainsCIS "guest /add") OR (SrcProcCmdLine ContainsCIS "net user" AND SrcProcCmdLine ContainsCIS "/active:yes") OR (RegistryKeyPath In Contains ("Terminal Server\AllowTSConnections","Terminal Server\DenyTSConnections") AND EventType In ("Registry Value Create","Registry Value Modified"))
```

### T1546.012 Image File Execution Options Injection
Atomics: [T1546.012](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1546.012/T1546.012.md)

Detection of Image File Execution Options tampering for persistence through Registry monitoring.

```
RegistryKeyPath In Contains Anycase ("CurrentVersion\Image File Execution Options","CurrentVersion\SilentProcessExit") AND RegistryKeyPath In Contains Anycase ("GlobalFlag","ReportingMode","MonitorProcess")
```

### T1136.001 Local Account
Atomics: [T1136.001](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1136.001/T1136.001.md)


### T1037.001 Logon Scripts (Windows)
Atomics: [T1037.001](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1037.001/T1037.001.md)

Detects addition of logon scripts through command line or registry methods.

```
SrcProcCmdLine ContainsCIS "UserInitMprLogonScript" OR (RegistryKeyPath ContainsCIS "UserInitMprLogonScript" AND EventType = "Registry Value Create")
```

### T1546.007 Netsh Helper DLL
Atomics: [T1546.007](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1546.007/T1546.007.md)

Detection of "helper" dlls with network command shell, through command arguments or registry modification.

```
(TgtProcName = "netsh.exe" AND TgtProcCmdLine ContainsCIS "add helper") OR (RegistryPath ContainsCIS "SOFTWARE\Microsoft\NetSh" AND EventType = "Registry Value Create")
```

### T1574.009 Unquoted Service Path for program.exe
Atomics: [T1574.009](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1574.009/T1574.009.md)

Detects creation or modification of the file at `C:\program.exe` for exploiting unquoted services paths of Program Files folder.

```
(FileFullName = "C:\program.exe" AND EventType In ("File Creation","File Modification")) OR TgtProcImagePath = "C:\program.exe"
```

### T1546.013 Malicious Process Start Added to Powershell Profile
Atomics: [T1546.013](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1546.013/T1546.013.md)

Detects the addition of process execution strings (`TgtProcCmdLine In Contains Anycase (list)`)to the powershell profile, through CommandLine and CommandScript indicators.

```
(SrcProcCmdScript ContainsCIS "Add-Content $profile -Value" AND SrcProcCmdScript ContainsCIS "Start-Process") OR (TgtProcCmdLine ContainsCIS "Add-Content $profile" AND TgtProcCmdLine In Contains Anycase ("Start-Process","& ","cmd.exe /c"))
```

### T1547.001 Registry Run Keys / Startup Folder
Atomics: [T1547.001](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1547.001/T1547.001.md)


### T1053.005 Scheduled Task
Atomics: [T1053.005](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1053.005/T1053.005.md)


### T1546.002 Screensaver
Atomics: [T1546.002](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1546.002/T1546.002.md)

Detects malicious changes to screensaver through Registry changes, filtering expected processes.

```
RegistryKeyPath ContainsCIS "Control Panel\Desktop\SCRNSAVE.EXE" AND (EventType In ("Registry Value Create","Registry Value Modified") AND SrcProcName Not In ("svchost.exe","SetupHost.exe","CcmExec.exe"))
```

### T1547.005 Security Support Provider
Atomics: [T1547.005](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1547.005/T1547.005.md)

Detection of changes to Security Support Provider through Registry modification. Filters most standard system changes with `SrcProcName Not In (list)` but there will be some noise from installers.

```
RegistryKeyPath ContainsCIS "\Control\Lsa\Security Packages" AND (SrcProcName Not In ("services.exe","SetupHost.exe","svchost.exe") AND SrcProcCmdLine Does Not ContainCIS "system32\wsauth.dll")
```

### T1574.010 Services File Permissions Weakness
Atomics: [T1574.010](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1574.010/T1574.010.md)


### T1574.011 Services Registry Permissions Weakness
Atomics: [T1574.011](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1574.011/T1574.011.md)


### T1547.009 Startup Shortcuts
Atomics: [T1547.009](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1547.009/T1547.009.md)

Focuses on Test 2: Detection .lnk or .url files written to Startup folders. Filters noise with `SrcProcName Not In (list)` but you can remove noise from 3rd party update services updating their links by adding `SrcProcParentName != "userinit.exe"` to the query.

```
(FileFullName ContainsCIS "Microsoft\Windows\Start Menu\Programs\Startup" AND TgtFileExtension In Contains ("lnk","url") AND EventType = "File Creation") AND SrcProcName Not In ("ONENOTE.EXE","msiexec.exe")
```

### T1505.002 Transport Agent
Atomics: [T1505.002](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1505.002/T1505.002.md)


### T1505.003 Web Shell
Atomics: [T1505.003](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1505.003/T1505.003.md)


### T1546.003 Windows Management Instrumentation Event Subscription
Atomics: [T1546.003](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1546.003/T1546.003.md)

Detect WMI Event Subs using the New-CimInstance cmdlet, through CommandLine and CommandScript indicators.

```
SrcProcCmdLine ContainsCIS "New-CimInstance -Namespace root/subscription" OR SrcProcCmdScript ContainsCIS "New-CimInstance -Namespace root/subscription"
```

### T1543.003 Windows Service
Atomics: [T1543.003](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1543.003/T1543.003.md)

Detects creation and modification of windows services through binPath argument to sc.exe.

```
TgtProcName = "sc.exe" AND TgtProcCmdLine Contains "binPath="
```

### T1547.004 Winlogon Helper DLL
Atomics: [T1547.004](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1547.004/T1547.004.md)

Detects Winlogon Helper Dll changes through Registry MetadataIndicator item, as it holds the full registry change info but will only return data of the Indicators object type.

```
IndicatorMetadata In Contains Anycase ("Microsoft\Windows NT\CurrentVersion\Winlogon","Microsoft\Windows NT\CurrentVersion\Winlogon\Notify") AND IndicatorMetadata In Contains Anycase ("logon","Userinit","Shell") AND IndicatorMetadata Does Not ContainCIS "WINDOWS\system32\userinit.exe"
```