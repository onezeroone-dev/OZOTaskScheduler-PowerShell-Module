# Get-OZOScheduledTask
This function is part of the [OZOTaskScheduler PowerShell Module](../README.md).

## Description
Get an object representing an existing task, if found.

## Syntax
```
Get-OZOScheduledTask
    -TaskName <String>
```

## Parameters
|Parameter|Description|
|---------|-----------|
|`TaskName`|The name of the task to get.|

## Example
```powershell
$ozoGetScheduledTask = (Get-OZOScheduledTask -TaskName "Update OZO PowerShell Module")
```

## Notes
This function requires _Administrator_ privileges.
