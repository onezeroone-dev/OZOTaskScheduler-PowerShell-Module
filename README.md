# OZOTaskScheduler PowerShell Module

## Description
OZOTaskScheduler provides a lightweight PowerShell interface for managing Windows Task Scheduler tasks. It is designed to create, update, enable, disable, export, and remove scheduled tasks using simple function calls and JSON task definitions.

## Installation
This module is published to Microsoft's [PowerShell Gallery](https://learn.microsoft.com/en-us/powershell/scripting/gallery/overview?view=powershell-5.1). Run the following command in an _Administrator_ PowerShell session:

```powershell
Install-Module OZOTaskScheduler
```

## Usage
Import the module in your script or console:

```powershell
Import-Module OZOTaskScheduler
```

## Functions
- [Disable-OZOScheduledTask](Documentation/Disable-OZOScheduledTask.md)
- [Enable-OZOScheduledTask](Documentation/Enable-OZOScheduledTask.md)
- [Export-OZOScheduledTask](Documentation/Export-OZOScheduledTask.md)
- [Get-OZOScheduledTask](Documentation/Get-OZOScheduledTask.md)
- [New-OZOScheduledTask](Documentation/New-OZOScheduledTask.md)
- [Remove-OZOScheduledTask](Documentation/Remove-OZOScheduledTask.md)
- [Set-OZOScheduledTask](Documentation/Set-OZOScheduledTask.md)


## Classes
- [OZOJsonTask](Documentation/OZOJsonTask.md)
- [OZOTask](Documentation/OZOTask.md)
- [OZOOnceDateTime](Documentation/OZOOnceDateTime.md)
- [OZOSchedule](Documentation/OZOSchedule.md)

## JSON Definition
[_New-OZOScheduledTask_](Documentation/New-OZOScheduledTask.md) and [_Set-OZOScheduledTask_](Documentation/Set-OZOScheduledTask.md) expect a task expressed as a JSON dictionary. The following example shows a _Scheduled_ task with three schedule entries:

```json
{
    "Name":"Example Scheduled Task",
    "Script":"C:\\Temp\\OZOTaskScheduler-ScheduledTask-Example.ps1",
    "Parameters":"",
    "Directory":"C:\\Temp",
    "Disabled":true,
    "Settings":{
        "AllowDemandStart":true,
        "AllowHardTerminate":true,
        "AllowStartOnRemoteAppSession":true,
        "Compatibility":"Win8",
        "DeleteExpiredTaskAfter":"PT0S",
        "DisallowStartIfOnBatteries":false,
        "DontStopIfGoingOnBatteries":true,
        "ExecutionTimeLimit":"PT0S",
        "Hidden":false,
        "IdleSettings":{
            "StopOnIdleEnd":false,
            "RestartOnIdle":false
        },
        "MultipleInstances":"IgnoreNew",
        "Priority":"Normal",
        "RunOnlyIfNetworkAvailable":false,
        "WakeToRun":false
    },
    "AtLogon":false,
    "AtReboot":true,
    "Once":true,
    "OnceDateTime":{
        "DateTime":"2099-12-31T09:00:00",
        "RandomDelay":0
    },
    "Scheduled":true,
    "Schedules":[
        {
            "WeekDay":"Monday",
            "StartTime":"8:00 AM",
            "RandomDelay":0
        },
        {
            "WeekDay":"Wednesday",
            "StartTime":"8:00 AM",
            "RandomDelay":0
        },
        {
            "WeekDay":"Friday",
            "StartTime":"8:00 AM",
            "RandomDelay":0
        }
    ]
}
```

The following example shows an _AtLogon_ task:
```json
{
    "Name":"Example AtLogon Task",
    "Script":"C:\\Temp\\OZOTaskScheduler-AtLogonTask-Example.ps1",
    "Parameters":"",
    "Directory":"C:\\Temp",
    "Disabled":true,
    "Settings":{
        "AllowDemandStart":true,
        "AllowHardTerminate":true,
        "AllowStartOnRemoteAppSession":true,
        "Compatibility":"Win8",
        "DeleteExpiredTaskAfter":"PT0S",
        "DisallowStartIfOnBatteries":false,
        "DontStopIfGoingOnBatteries":true,
        "ExecutionTimeLimit":"PT0S",
        "Hidden":false,
        "IdleSettings":{
            "StopOnIdleEnd":false,
            "RestartOnIdle":false
        },
        "MultipleInstances":"IgnoreNew",
        "Priority":"Normal",
        "RunOnlyIfNetworkAvailable":false,
        "WakeToRun":false
    },
    "AtLogon":true,
    "AtReboot":false,
    "Once":false,
    "OnceDateTime":{},
    "Scheduled":false,
    "Schedules":[]
}
```

|Key|Description|
|---|-----------|
|`Name`|The name of the scheduled task.|
|`Script`|The full path to the script or program to run.|
|`Parameters`|Parameters for the script or program.|
|`Directory`|The working directory for the task.|
|`Disabled`|Determines whether the task is disabled when created. Allowed values are _true_ and _false_.|
|`Settings`|A dictionary of Task Scheduler settings. See _Settings_, below.|
|`Scheduled`|Determines whether the task runs on one or more weekly schedules. Allowed values are _true_ and _false_. May be combined with _Once_ and _AtReboot_. If combined with _AtLogon_, the _AtLogon_ trigger is ignored.|
|`Schedules`|The schedule definitions for _Scheduled_ tasks. See _Schedules_, below.|
|`Once`|Determines whether the task runs once at the date and time in _OnceDateTime_. Allowed values are _true_ and _false_. May be combined with _Scheduled_ and _AtReboot_. If combined with _AtLogon_, the _AtLogon_ trigger is ignored.|
|`OnceDateTime`|The one-time trigger definition. Required when _Once_ is _true_; use an empty object when _Once_ is _false_. See _OnceDateTime_, below.|
|`AtReboot`|Determines whether the task runs at startup/reboot. Allowed values are _true_ and _false_. May be combined with _Scheduled_ and _Once_. If combined with _AtLogon_, the _AtLogon_ trigger is ignored.|
|`AtLogon`|Determines whether the task runs at user logon. Allowed values are _true_ and _false_. The trigger is created only when _Scheduled_, _Once_, and _AtReboot_ are all _false_.|

_Settings_ is a dictionary containing Task Scheduler settings:
```json
{
    "AllowDemandStart":true,
    "AllowHardTerminate":true,
    "AllowStartOnRemoteAppSession":true,
    "Compatibility":"Win8",
    "DeleteExpiredTaskAfter":"PT0S",
    "DisallowStartIfOnBatteries":false,
    "DontStopIfGoingOnBatteries":true,
    "ExecutionTimeLimit":"PT0S",
    "Hidden":false,
    "IdleSettings":{
        "StopOnIdleEnd":false,
        "RestartOnIdle":false
    },
    "MultipleInstances":"IgnoreNew",
    "Priority":"Normal",
    "RunOnlyIfNetworkAvailable":false,
    "WakeToRun":false
}
```

|Key|Description|
|---|-----------|
|`AllowDemandStart`|Determines whether the task can be started on demand (manually or by another program). Allowed values are _true_ and _false_. Defaults to _true_.|
|`AllowHardTerminate`|Determines whether the task can be terminated by ending its process. Allowed values are _true_ and _false_. Defaults to _true_.|
|`AllowStartOnRemoteAppSession`|Determines whether the task can start when launched from a Remote Desktop/RemoteApp session. Allowed values are _true_ and _false_. Defaults to _true_.|
|`Compatibility`|Task compatibility mode. Allowed values are _At_, _V1_, _Vista_, _Win7_, and _Win8_. Defaults to _Win8_.|
|`DeleteExpiredTaskAfter`|The amount of time to wait after the task expires before Task Scheduler deletes it, expressed as an ISO 8601 duration (for example, _PT0S_ or _P30D_). Omit to never delete the task automatically.|
|`DisallowStartIfOnBatteries`|Determines whether the task is prevented from starting when the computer is running on battery power. Allowed values are _true_ and _false_. Defaults to _true_.|
|`DontStopIfGoingOnBatteries`|Determines whether a running task keeps running after the computer switches to battery power. Allowed values are _true_ and _false_. Defaults to _false_.|
|`ExecutionTimeLimit`|The maximum amount of time the task is allowed to run, expressed as an ISO 8601 duration (for example, _PT72H_, or _PT0S_ for no limit). Defaults to _PT72H_.|
|`Hidden`|Determines whether the task is hidden in the Task Scheduler UI. Allowed values are _true_ and _false_. Defaults to _false_.|
|`IdleSettings`|Idle-related settings for the task. See _IdleSettings_, below.|
|`MultipleInstances`|Determines how Task Scheduler handles multiple simultaneous instances of the task. Allowed values are _IgnoreNew_, _Parallel_, and _Queue_. Defaults to _IgnoreNew_.|
|`Priority`|The task's process priority. Accepts an integer from _0_ (highest) to _10_ (lowest), or the friendly value _Normal_ (equivalent to _7_). Defaults to _7_.|
|`RunOnlyIfNetworkAvailable`|Determines whether the task only runs when a network connection is available. Allowed values are _true_ and _false_. Defaults to _false_.|
|`WakeToRun`|Determines whether the computer is woken from sleep to run the task. Allowed values are _true_ and _false_. Defaults to _false_.|

_IdleSettings_ is a dictionary containing idle settings:
```
{
    "StopOnIdleEnd":false,
    "RestartOnIdle":false
}
```

|Key|Description|
|---|-----------|
|`StopOnIdleEnd`|Determines whether the task stops if the idle condition ends before the task completes. Allowed values are _true_ and _false_. Defaults to _true_.|
|`RestartOnIdle`|Determines whether the task restarts the next time the computer becomes idle, if it was stopped because the idle condition ended. Allowed values are _true_ and _false_. Defaults to _false_.|

_Schedules_ is a list of dictionaries. Each dictionary should contain a `WeekDay`, `StartTime`, and `RandomDelay` value in seconds. Example:
```json
[
    {
        "WeekDay":"Monday",
        "StartTime":"8:00 AM",
        "RandomDelay":0
    },
    {
        "WeekDay":"Wednesday",
        "StartTime":"8:00 AM",
        "RandomDelay":0
    },
    {
        "WeekDay":"Friday",
        "StartTime":"8:00 AM",
        "RandomDelay":0
    }
]
```

|Key|Description|
|---|-----------|
|`WeekDay`|The day of the week to run the task. Allowed values are _Sunday_, _Monday_, _Tuesday_, _Wednesday_, _Thursday_, _Friday_, and _Saturday_.|
|`StartTime`|The start time for the task in `HH:MM AM/PM` format.|
|`RandomDelay`|The number of seconds to randomize the start time. Allowed range is 0-3600 seconds.|

_OnceDateTime_ is a dictionary containing one date/time trigger definition:
```json
{
    "DateTime":"2099-12-31T09:00:00",
    "RandomDelay":0
}
```

|Key|Description|
|---|-----------|
|`DateTime`|The date and time for the one-time trigger. Use an ISO 8601 value. The value must not be in the past.|
|`RandomDelay`|The number of seconds to randomize the start time. Allowed range is 0-3600 seconds.|

### Generating a Compressed JSON String
You can define your JSON in any text editor and save it as a file, for example [`OZOTaskScheduler-ScheduledTask-Example.json`](OZOTaskScheduler-ScheduledTask-Example.json) and [`OZOTaskScheduler-AtLogonTask-Example.json`](OZOTaskScheduler-AtLogonTask-Example.json), then convert the file to a compressed JSON string with [_Convert-OZOJsonFileToString_](https://github.com/onezeroone-dev/OZOStrings-PowerShell-Module/blob/main/Documentation/Convert-OZOJsonFileToString.md):
```powershell
Convert-OZOJsonFileToString -Path C:\Temp\OZOTaskScheduler-ScheduledTask-Example.json
{"Name":"Example Scheduled Task","Script":"C:\\Temp\\OZOTaskScheduler-ScheduledTask-Example.ps1","Parameters":"","Directory":"C:\\Temp","Disabled":true,"Settings":{"AllowDemandStart":true,"AllowHardTerminate":true,"AllowStartOnRemoteAppSession":true,"Compatibility":"Win8","DeleteExpiredTaskAfter":"PT0S","DisallowStartIfOnBatteries":false,"DontStopIfGoingOnBatteries":true,"ExecutionTimeLimit":"PT0S","Hidden":false,"IdleSettings":{"StopOnIdleEnd":false,"RestartOnIdle":false},"MultipleInstances":"IgnoreNew","Priority":"Normal","RunOnlyIfNetworkAvailable":false,"WakeToRun":false},"AtLogon":false,"AtReboot":true,"Once":true,"OnceDateTime":{"DateTime":"2099-12-31T09:00:00","RandomDelay":0},"Scheduled":true,"Schedules":[{"WeekDay":"Monday","StartTime":"8:00 AM","RandomDelay":0},{"WeekDay":"Wednesday","StartTime":"8:00 AM","RandomDelay":0},{"WeekDay":"Friday","StartTime":"8:00 AM","RandomDelay":0}]}
```

Encapsulate the resulting compressed JSON string in single quotes (`'`) so it can be used as the value for _JsonString_:
```powershell
'{"Name":"Example Scheduled Task","Script":"C:\\Temp\\OZOTaskScheduler-ScheduledTask-Example.ps1","Parameters":"","Directory":"C:\\Temp","Disabled":true,"Settings":{"AllowDemandStart":true,"AllowHardTerminate":true,"AllowStartOnRemoteAppSession":true,"Compatibility":"Win8","DeleteExpiredTaskAfter":"PT0S","DisallowStartIfOnBatteries":false,"DontStopIfGoingOnBatteries":true,"ExecutionTimeLimit":"PT0S","Hidden":false,"IdleSettings":{"StopOnIdleEnd":false,"RestartOnIdle":false},"MultipleInstances":"IgnoreNew","Priority":"Normal","RunOnlyIfNetworkAvailable":false,"WakeToRun":false},"AtLogon":false,"AtReboot":true,"Once":true,"OnceDateTime":{"DateTime":"2099-12-31T09:00:00","RandomDelay":0},"Scheduled":true,"Schedules":[{"WeekDay":"Monday","StartTime":"8:00 AM","RandomDelay":0},{"WeekDay":"Wednesday","StartTime":"8:00 AM","RandomDelay":0},{"WeekDay":"Friday","StartTime":"8:00 AM","RandomDelay":0}]}'
```

## Logging
When available, messages are written to the [_One Zero One_ event provider](https://github.com/onezeroone-dev/OZOLogger-PowerShell-Module/blob/main/README.md). Otherwise, events are written to the _Microsoft-Windows-PowerShell_ provider as _Information_ events with event ID *4100*.

## Notes
This module requires _Administrator_ privileges.

## License
This module is licensed under the [GNU General Public License (GPL) version 2.0](LICENSE).

## Acknowledgements
Special thanks to my employer, [Sonic Healthcare USA](https://sonichealthcareusa.com), who supports the growth of my PowerShell skillset and enables me to contribute portions of my work product to the PowerShell community. Thanks also to GitHub Copilot (Claude Sonnet 5), a co-author of this module, for pairing on design reviews, the `Once`/`Settings` feature work, `GetExistingTask()`, and the integration test suite.
