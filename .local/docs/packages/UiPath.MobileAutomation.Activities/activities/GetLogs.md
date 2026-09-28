# Get Logs

`UiPath.MobileAutomation.Activities.GetLogs`

Capture device logs of a specific channel (e.g. `logcat` on Android) to a file.

**Package:** `UiPath.MobileAutomation.Activities`
**Category:** Logging & Debugging
**Required Scope:** `UiPath.MobileAutomation.Activities.MobileDeviceConnection`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `LogType` | Log Type | InArgument | `string` | Yes | | Log channel name. Driver-specific — common values: `"logcat"`, `"bugreport"`, `"server"` (Android); `"syslog"`, `"crashlog"`, `"performance"`, `"server"` (iOS). Use `GetLogTypes` to enumerate available channels. |
| `LogFilePath` | Log File Path | InArgument | `string` | Yes | | Absolute or relative path where the captured logs will be written. |
| `AppendToLogFile` | Append To Log File | InArgument | `bool` | Yes | `true` | When `true`, append new logs to existing file content. When `false`, overwrite the file. |

### Common

| Name | Display Name | Kind | Type | Default | Description |
|------|-------------|------|------|---------|-------------|
| `ContinueOnError` | Continue On Error | InArgument | `bool` | `false` | Continue executing the automation if this activity fails. |
| `DelayBefore` | Delay Before | InArgument | `double` | | Delay (in milliseconds) before the capture. |
| `DelayAfter` | Delay After | InArgument | `double` | | Delay (in milliseconds) after the capture. |

## XAML Example

Capture Android logcat to a file in the project's diagnostics folder:

```xml
<uma:GetLogs DisplayName="Get Logs"
             LogType="logcat"
             LogFilePath="diagnostics/logcat.txt"
             AppendToLogFile="True" />
```

Capture iOS syslog (overwrite previous):

```xml
<uma:GetLogs DisplayName="Get Logs"
             LogType="syslog"
             LogFilePath="diagnostics/ios_syslog.txt"
             AppendToLogFile="False" />
```

Namespace required:
* `xmlns:uma="clr-namespace:UiPath.MobileAutomation.Activities;assembly=UiPath.MobileAutomation.Activities"`

## How to create a new Get Logs

```bash
uip rpa get-default-activity-xaml --activity-class-name UiPath.MobileAutomation.Activities.GetLogs
```

## Notes

- Must be placed inside a `MobileDeviceConnection` scope.
- **Pick `LogType` based on the connected platform**:
  - Android (UIAutomator2 driver): `logcat` is the most useful — app-level + system messages.
  - iOS (XCUITest driver): `syslog` for general device logs, `crashlog` for crash reports.
- Use `GetLogTypes` if you're not sure which log channels are available — it returns the driver's actual list at runtime.
- The file is written directly by the activity — no extra "save" step required.
- Note that on Android the `logcat` buffer is finite (a few MB); long-running tests should call `GetLogs` periodically rather than waiting until the very end, to avoid buffer rollover.
