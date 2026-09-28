# Manage Other App

`UiPath.MobileAutomation.Activities.ManageOtherApp`

Check / activate / terminate / uninstall an app identified by package name (Android) or bundle id (iOS), independent of the current app set on the `MobileDeviceConnection`.

**Package:** `UiPath.MobileAutomation.Activities`
**Category:** Mobile Application
**Required Scope:** `UiPath.MobileAutomation.Activities.MobileDeviceConnection`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `App` | App | InArgument | `string` | Yes | | App identifier — Android package name (e.g. `"com.android.settings"`) or iOS bundle ID (e.g. `"com.apple.mobilesafari"`). |
| `Command` | Command | InArgument | [`ManageOtherAppCommandEnum`](#manage-other-app-command-enum) | Yes | | Lifecycle command to apply to the named app. |

### Output

| Name | Display Name | Type | Description |
|------|-------------|------|-------------|
| `Result` | Result | `bool` | For `IsInstalled`: `True` if the app is installed. For other commands: `True` if the command succeeded. |

### Common

| Name | Display Name | Kind | Type | Default | Description |
|------|-------------|------|------|---------|-------------|
| `ContinueOnError` | Continue On Error | InArgument | `bool` | `false` | Continue executing the automation if this activity fails. |
| `DelayBefore` | Delay Before | InArgument | `double` | | Delay (in milliseconds) before the command runs. |
| `DelayAfter` | Delay After | InArgument | `double` | | Delay (in milliseconds) after the command runs. |

## Manage Other App Command Enum

`UiPath.MobileAutomation.Enums.ManageOtherAppCommandEnum`

| Value | Description |
|-------|-------------|
| `IsInstalled` | Check whether the app is installed. Result is returned in `Result` (`True` = installed). |
| `Remove` | Uninstall the app from the device. |
| `Activate` | Bring the app to the foreground (launch if not running). |
| `Terminate` | Force-stop the app. |

These four are the complete set. The enum does **not** include `Install` or `GetState`. To install an app from a local file path, use `InstallApp`.

## XAML Examples

Check whether the calculator app is installed:

```xml
<uma:ManageOtherApp DisplayName="Manage Other App"
                    App="com.google.android.calculator"
                    Command="IsInstalled"
                    Result="[isInstalled]" />
```

Terminate a specific background app:

```xml
<uma:ManageOtherApp DisplayName="Manage Other App"
                    App="com.example.basicapp"
                    Command="Terminate"
                    Result="[terminated]" />
```

Namespace required:
* `xmlns:uma="clr-namespace:UiPath.MobileAutomation.Activities;assembly=UiPath.MobileAutomation.Activities"`

## How to create a new Manage Other App

```bash
uip rpa get-default-activity-xaml --activity-class-name UiPath.MobileAutomation.Activities.ManageOtherApp
```

## Notes

- Must be placed inside a `MobileDeviceConnection` scope.
- `Command = Activate` will launch the app from cold if it is not currently running. Useful for switching to a side app (Settings, Email) and back.
- Use the **package name** on Android (lowercase, dot-separated, e.g. `"com.android.settings"`) and the **bundle ID** on iOS (reverse-DNS, e.g. `"com.apple.mobilesafari"`). The activity does not normalise across platforms.
- To install a fresh apk/ipa, use `InstallApp` (which takes a file path) — `ManageOtherApp` cannot install apps; it can only manipulate ones already on the device.
- For the app currently set on the connection (`appPackage` / `bundleId` capability), prefer `ManageCurrentApp` — it avoids duplicating the identifier.
