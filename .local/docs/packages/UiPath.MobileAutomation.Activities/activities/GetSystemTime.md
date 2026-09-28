# Get System Time

`UiPath.MobileAutomation.Activities.GetSystemTime`

Read the current system time from the connected mobile device.

**Package:** `UiPath.MobileAutomation.Activities`
**Category:** Mobile Device
**Required Scope:** `UiPath.MobileAutomation.Activities.MobileDeviceConnection`

## Properties

### Output

| Name | Display Name | Type | Description |
|------|-------------|------|-------------|
| `SystemTime` | System Time | `DateTime` | Current system time on the device, in the device's configured time zone. |

### Common

| Name | Display Name | Kind | Type | Default | Description |
|------|-------------|------|------|---------|-------------|
| `ContinueOnError` | Continue On Error | InArgument | `bool` | `false` | Continue executing the automation if this activity fails. |
| `DelayBefore` | Delay Before | InArgument | `double` | | Delay (in milliseconds) before the read. |
| `DelayAfter` | Delay After | InArgument | `double` | | Delay (in milliseconds) after the read. |

## XAML Example

```xml
<uma:GetSystemTime DisplayName="Get System Time"
                   SystemTime="[deviceTime]" />
```

Namespace required:
* `xmlns:uma="clr-namespace:UiPath.MobileAutomation.Activities;assembly=UiPath.MobileAutomation.Activities"`

## How to create a new Get System Time

```bash
uip rpa get-default-activity-xaml --activity-class-name UiPath.MobileAutomation.Activities.GetSystemTime
```

## Notes

- Must be placed inside a `MobileDeviceConnection` scope.
- The returned `DateTime` reflects the device's configured time zone, not UTC. Convert with `.ToUniversalTime()` if you need UTC.
- Useful for time-sensitive validations (e.g. asserting that a log entry was emitted after a particular wall-clock moment on the device) or for diagnosing time-drift issues between the test host and the device.
