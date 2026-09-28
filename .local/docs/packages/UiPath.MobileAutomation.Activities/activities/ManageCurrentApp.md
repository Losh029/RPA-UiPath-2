# Manage Current App

`UiPath.MobileAutomation.Activities.ManageCurrentApp`

Launch or reset the application currently configured in the `MobileDeviceConnection` (the app identified by the connection's `app` / `appPackage` / `bundleId` capability).

**Package:** `UiPath.MobileAutomation.Activities`
**Category:** Mobile Application
**Required Scope:** `UiPath.MobileAutomation.Activities.MobileDeviceConnection`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Command` | Command | InArgument | [`ManageCurrentAppCommandEnum`](#manage-current-app-command-enum) | Yes | `Launch` | Lifecycle command to apply to the current app. |

### Output

| Name | Display Name | Type | Description |
|------|-------------|------|-------------|
| `Result` | Result | `bool` | `True` if the command succeeded. |

### Common

| Name | Display Name | Kind | Type | Default | Description |
|------|-------------|------|------|---------|-------------|
| `ContinueOnError` | Continue On Error | InArgument | `bool` | `false` | Continue executing the automation if this activity fails. |
| `DelayBefore` | Delay Before | InArgument | `double` | | Delay (in milliseconds) before the command runs. |
| `DelayAfter` | Delay After | InArgument | `double` | | Delay (in milliseconds) after the command runs. |

## Manage Current App Command Enum

`UiPath.MobileAutomation.Enums.ManageCurrentAppCommandEnum`

| Value | Description |
|-------|-------------|
| `Launch` | Start the app if it is not already running. Default. |
| `Reset` | Reset the app to a clean state (clears app data on Android, equivalent of `mobile: resetApp`). |

Only these two values exist. There is **no** `Close`, `Activate`, `Terminate`, or `GetState` command on this activity. For:
* terminating an arbitrary app → `ManageOtherApp` with `Command = Terminate`
* bringing an arbitrary app to the foreground → `ManageOtherApp` with `Command = Activate`
* closing the current session → let the `MobileDeviceConnection` scope exit (with default `Close = Always`)

## XAML Example

Launch the configured app:

```xml
<uma:ManageCurrentApp DisplayName="Manage Current App"
                      Command="Launch"
                      Result="[launchOk]" />
```

Reset app state between test cases:

```xml
<uma:ManageCurrentApp DisplayName="Manage Current App"
                      Command="Reset"
                      Result="[resetOk]" />
```

Namespace required:
* `xmlns:uma="clr-namespace:UiPath.MobileAutomation.Activities;assembly=UiPath.MobileAutomation.Activities"`

## How to create a new Manage Current App

```bash
uip rpa get-default-activity-xaml --activity-class-name UiPath.MobileAutomation.Activities.ManageCurrentApp
```

## Notes

- Must be placed inside a `MobileDeviceConnection` scope.
- `Reset` is the most common usage between test cases — it clears app state without tearing down the Appium session.
- When the `MobileDeviceConnection.Arguments` dictionary specifies an `app` / `appPackage`, the app is launched automatically at scope entry. An explicit `ManageCurrentApp` call with `Launch` is typically only needed after a `Reset` or `Terminate`.
- For lifecycle operations against apps **other** than the current one (install, uninstall, check-installed, activate, terminate), use `ManageOtherApp`.
