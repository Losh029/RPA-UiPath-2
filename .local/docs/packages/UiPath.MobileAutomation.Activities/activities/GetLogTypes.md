# Get Log Types

`UiPath.MobileAutomation.Activities.GetLogTypes`

List the log channels available in the current Appium session.

**Package:** `UiPath.MobileAutomation.Activities`
**Category:** Logging & Debugging
**Required Scope:** `UiPath.MobileAutomation.Activities.MobileDeviceConnection`

## Properties

### Output

| Name | Display Name | Type | Description |
|------|-------------|------|-------------|
| `LogTypes` | Log Types | `string[]` | Array of available log type names (e.g. `["logcat", "bugreport", "server"]` on Android, `["syslog", "crashlog", "performance", "server"]` on iOS). |

### Common

| Name | Display Name | Kind | Type | Default | Description |
|------|-------------|------|------|---------|-------------|
| `ContinueOnError` | Continue On Error | InArgument | `bool` | `false` | Continue executing the automation if this activity fails. |
| `DelayBefore` | Delay Before | InArgument | `double` | | Delay (in milliseconds) before the read. |
| `DelayAfter` | Delay After | InArgument | `double` | | Delay (in milliseconds) after the read. |

## XAML Example

```xml
<uma:GetLogTypes DisplayName="Get Log Types"
                 LogTypes="[logTypesArray]" />
```

Namespace required:
* `xmlns:uma="clr-namespace:UiPath.MobileAutomation.Activities;assembly=UiPath.MobileAutomation.Activities"`

## How to create a new Get Log Types

```bash
uip rpa get-default-activity-xaml --activity-class-name UiPath.MobileAutomation.Activities.GetLogTypes
```

## Notes

- Must be placed inside a `MobileDeviceConnection` scope.
- The set of returned types depends on the platform and driver — Android UIAutomator2 typically returns `logcat`, `bugreport`, `server`; iOS XCUITest typically returns `syslog`, `crashlog`, `performance`, `server`.
- Use the returned values as input to `GetLogs.LogType`.
