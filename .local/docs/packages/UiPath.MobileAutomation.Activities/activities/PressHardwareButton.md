# Press Hardware Button

`UiPath.MobileAutomation.Activities.PressHardwareButton`

Simulate pressing one of the device's hardware (or soft) navigation buttons.

**Package:** `UiPath.MobileAutomation.Activities`
**Category:** Hardware Interaction
**Required Scope:** `UiPath.MobileAutomation.Activities.MobileDeviceConnection`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `HardwareButton` | Hardware Button | InArgument | [`HardwareButton`](#hardware-button-enum) | Yes | `HomeButton` | Which button to press. |

### Common

| Name | Display Name | Kind | Type | Default | Description |
|------|-------------|------|------|---------|-------------|
| `ContinueOnError` | Continue On Error | InArgument | `bool` | `false` | Continue executing the automation if this activity fails. |
| `DelayBefore` | Delay Before | InArgument | `double` | | Delay (in milliseconds) before the press. |
| `DelayAfter` | Delay After | InArgument | `double` | | Delay (in milliseconds) after the press. |

The property is named `HardwareButton` (not `Button`).

## Hardware Button Enum

`UiPath.MobileAutomation.Enums.HardwareButton`

| Value | Description | Platforms |
|-------|-------------|-----------|
| `HomeButton` | Home / go to launcher. Default. | Android + iOS |
| `BackButton` | System back. | Android only |
| `SwitchAppButton` | Recents / app-switcher. | Android only |

Only these three values exist. The enum does **not** include `Menu`, `VolumeUp`, `VolumeDown`, `Power`, `Camera`, or `Search`. For volume / power / camera, use `ExecuteCommand` with the appropriate Appium mobile command (e.g. `mobile: pressKey` on Android).

## XAML Example

Go back to the previous screen on Android:

```xml
<uma:PressHardwareButton DisplayName="Press Hardware Button"
                         HardwareButton="BackButton" />
```

Namespace required:
* `xmlns:uma="clr-namespace:UiPath.MobileAutomation.Activities;assembly=UiPath.MobileAutomation.Activities"`

## How to create a new Press Hardware Button

```bash
uip rpa get-default-activity-xaml --activity-class-name UiPath.MobileAutomation.Activities.PressHardwareButton
```

## Notes

- Must be placed inside a `MobileDeviceConnection` scope.
- `BackButton` and `SwitchAppButton` are Android-only. On iOS they will throw at runtime.
- For keys not in the enum (volume, power, camera, search, function keys), use `ExecuteCommand` with a driver-specific command such as `mobile: pressKey` (Android UIAutomator2) or `mobile: pressButton` (iOS XCUITest).
