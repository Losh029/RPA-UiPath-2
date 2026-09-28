# Set Device Orientation

`UiPath.MobileAutomation.Activities.SetDeviceOrientation`

Set the device's screen orientation to Portrait, Landscape, or toggle the current value.

**Package:** `UiPath.MobileAutomation.Activities`
**Category:** Mobile Device
**Required Scope:** `UiPath.MobileAutomation.Activities.MobileDeviceConnection`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `DesiredOrientation` | Desired Orientation | InArgument | [`DeviceOrientationEnum`](#device-orientation-enum) | Yes | `Toggle` | Target orientation to rotate to. |

### Output

| Name | Display Name | Type | Description |
|------|-------------|------|-------------|
| `Orientation` | Orientation | [`ScreenOrientation`](#screen-orientation-enum) | The orientation the device is in after the operation completes (`Portrait` or `Landscape`). Useful when `DesiredOrientation = Toggle` to learn which side it landed on. |

### Common

| Name | Display Name | Kind | Type | Default | Description |
|------|-------------|------|------|---------|-------------|
| `ContinueOnError` | Continue On Error | InArgument | `bool` | `false` | Continue executing the automation if this activity fails. |
| `DelayBefore` | Delay Before | InArgument | `double` | | Delay (in milliseconds) before the rotation. |
| `DelayAfter` | Delay After | InArgument | `double` | | Delay (in milliseconds) after the rotation. |

The input property is named `DesiredOrientation`, and its type is `DeviceOrientationEnum` — not `Orientation` / `ScreenOrientation` as earlier docs suggested. The **output** property is named `Orientation`, of type `ScreenOrientation`.

## Device Orientation Enum

`UiPath.MobileAutomation.Activities.Enums.DeviceOrientationEnum`

| Value | Description |
|-------|-------------|
| `Portrait` | Rotate to portrait. |
| `Landscape` | Rotate to landscape. |
| `Toggle` | Flip to the opposite of the current orientation. Default. |

## Screen Orientation Enum

`UiPath.MobileAutomation.Enums.ScreenOrientation` — the type of the `Orientation` output. Has fewer values than `DeviceOrientationEnum` because it represents a current state, not a desired action.

| Value | Description |
|-------|-------------|
| `Portrait` | Device is in portrait. |
| `Landscape` | Device is in landscape. |

## XAML Example

Rotate to landscape and capture the resulting orientation:

```xml
<uma:SetDeviceOrientation DisplayName="Set Device Orientation"
                          DesiredOrientation="Landscape"
                          Orientation="[currentOrientation]" />
```

Toggle the orientation (useful when you don't know the current state):

```xml
<uma:SetDeviceOrientation DisplayName="Set Device Orientation"
                          DesiredOrientation="Toggle"
                          Orientation="[currentOrientation]" />
```

Namespace required:
* `xmlns:uma="clr-namespace:UiPath.MobileAutomation.Activities;assembly=UiPath.MobileAutomation.Activities"`

## How to create a new Set Device Orientation

```bash
uip rpa get-default-activity-xaml --activity-class-name UiPath.MobileAutomation.Activities.SetDeviceOrientation
```

## Notes

- Must be placed inside a `MobileDeviceConnection` scope.
- `Toggle` is the safest default when you don't care about absolute orientation, just want to flip. Bind the `Orientation` output if you need to know which way it ended up.
- The device must support orientation changes — some apps lock their orientation regardless of the system setting; in that case the activity may report success but the visible orientation will not change.
- Companion read-only activity: `GetDeviceOrientation`.
