# Set-OZOScheduledTask
This function is part of the [OZOTaskScheduler PowerShell Module](../README.md).

## Description
Updates an existing scheduled task. The module uses `powershell.exe` to run `.ps1` scripts and `cmd.exe` to run other executable files. Tasks may run at logon for the logged-in user with the `AtLogon` setting, or may run as the _SYSTEM_ account with `Scheduled`, `Once`, and `AtReboot` triggers. `Once` creates a single date/time trigger from `OnceDateTime`. `AtLogon` is ignored when `Scheduled`, `Once`, or `AtReboot` is enabled.

If the task already exists, it will be removed and recreated.

> **Note:** Tasks created with `Scheduled`, `Once`, or `AtReboot` always run as the _SYSTEM_ account; there is currently no JSON option to specify a different account. If a task needs to run as a different user, create or update it with this module first, then change the task's principal (and supply credentials) directly in Task Scheduler or with `Set-ScheduledTask -User -Password`.

## Prerequisites
This script requires _Administrator_ privileges.

## Syntax
This function supports two parameter sets: one for tasks defined in a JSON file and one for tasks defined as a compressed JSON string.

```
Set-OZOScheduledTask
    -JsonFile <string>
    [-PassThru]

Set-OZOScheduledTask
    -JsonString <String>
    [-PassThru]
```

## Parameters
|Parameter|Description|
|---------|-----------|
|`JsonFile`|The path to a JSON file that defines the task configuration.|
|`JsonString`|A compressed JSON string that defines the task configuration. See _Generating a Compressed JSON String_, below.|
|`PassThru`|Return the updated task.|

## JSON Definition
See [README.md](..\README.md).

## Examples
### Example 1
```powershell
Set-OZOScheduledTask -JsonFile "C:\Temp\OZOTaskScheduler-ScheduledTask-Example.json"
```
### Example 2
```powershell
Set-OZOScheduledTask -JsonString '{"Name":"Example Scheduled Task","Script":"C:\\Temp\\OZOTaskScheduler-ScheduledTask-Example.ps1","Parameters":"","Directory":"C:\\Temp","Disabled":true,"Settings":{"AllowDemandStart":true,"AllowHardTerminate":true,"AllowStartOnRemoteAppSession":true,"Compatibility":"Win8","DeleteExpiredTaskAfter":"PT0S","DisallowStartIfOnBatteries":false,"DontStopIfGoingOnBatteries":true,"ExecutionTimeLimit":"PT0S","Hidden":false,"IdleSettings":{"StopOnIdleEnd":false,"RestartOnIdle":false},"MultipleInstances":"IgnoreNew","Priority":"Normal","RunOnlyIfNetworkAvailable":false,"WakeToRun":false},"AtLogon":false,"AtReboot":true,"Once":true,"OnceDateTime":{"DateTime":"2099-12-31T09:00:00","RandomDelay":0},"Scheduled":true,"Schedules":[{"WeekDay":"Monday","StartTime":"8:00 AM","RandomDelay":0},{"WeekDay":"Wednesday","StartTime":"8:00 AM","RandomDelay":0},{"WeekDay":"Friday","StartTime":"8:00 AM","RandomDelay":0}]}'
```

## See Also
* [`OZOTaskScheduler-ScheduledTask-Example.json`](OZOTaskScheduler-ScheduledTask-Example.json)
* [`OZOTaskScheduler-AtLogonTask-Example.json`](OZOTaskScheduler-AtLogonTask-Example.json)
