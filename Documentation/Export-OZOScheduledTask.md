# Export-OZOScheduledTask
This function is part of the [OZOTaskScheduler PowerShell Module](../README.md).

## Description
Exports a task to JSON, if found.

## Syntax
```
Export-OZOScheduledTask
    -OutFile  <String>
    -TaskName <String>
```

## Parameters
|Parameter|Description|
|---------|-----------|
|`OutFile`|The path for the output JSON file.|
|`TaskName`|The name of the task to export.|

## Example
```powershell
Export-OZOScheduledTask -OutFile "C:\Temp\update-ozo-powershell-module-task.json" -TaskName "Update OZO PowerShell Module"
```

## Notes
This function requires _Administrator_ privileges.
