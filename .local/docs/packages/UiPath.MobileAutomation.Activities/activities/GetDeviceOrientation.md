# Get Device Orientation

`UiPath.MobileAutomation.Activities.GetDeviceOrientation`

Read the current device screen orientation.

**Package:** `UiPath.MobileAutomation.Activities`
**Category:** Mobile Device
**Required Scope:** `UiPath.MobileAutomation.Activities.MobileDeviceConnection`

## Properties

### Output

| Name | Display Name | Type | Description |
|------|-------------|------|-------------|
| `Orientation` | Orientation | [`ScreenOrientation`](#screen-orientation-enum) | Current device orientation (`Portrait` or `Landscape`). |

### Common

| Name | Display Name | Kind | Type | Default | Description |
|------|-------------|------|------|---------|-------------|
| `ContinueOnError` | Continue On Error | InArgument | `bool` | `false` | Continue executing the automation if this activity fails. |
| `DelayBefore` | Delay Before | InArgument | `double` | | Delay (in milliseconds) before the read. |
| `DelayAfter` | Delay After | InArgument | `double` | | Delay (in milliseconds) after the read. |

## Screen Orientation Enum

`UiPath.MobileAutomation.Enums.ScreenOrientation`

| Value | Description |
|-------|-------------|
| `Portrait` | Device is in portrait orientation. |
| `Landscape` | Device is in landscape orientation. |

## XAML Example

```xml
<uma:GetDeviceOrientation DisplayName="Get Device Orientation"
                          Orientation="[deviceOrientation]" />
```

Namespace required:
* `xmlns:uma="clr-namespace:UiPath.MobileAutomation.Activities;assembly=UiPath.MobileAutomation.Activities"`

## How to create a new Get Device Orientation

```bash
uip rpa get-default-activity-xaml --activity-class-name UiPath.MobileAutomation.Activities.GetDeviceOrientation
```

## Notes

- Must be placed inside a `MobileDeviceConnection` scope.
- Returns the **current** orientation regardless of whether the user, the system, or `SetDeviceOrientation` caused it.
- Companion write activity: `SetDeviceOrientation`.
