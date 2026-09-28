# Install App

`UiPath.MobileAutomation.Activities.InstallApp`

Install a mobile application from a local file or remote URL onto the connected device.

**Package:** `UiPath.MobileAutomation.Activities`
**Category:** Mobile Application
**Required Scope:** `UiPath.MobileAutomation.Activities.MobileDeviceConnection`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `AppPath` | App Path | InArgument | `string` | Yes | | Path or URL to the installer. APK on Android, IPA / APP on iOS. Local file paths and HTTPS URLs are both supported. |

### Output

| Name | Display Name | Type | Description |
|------|-------------|------|-------------|
| `Result` | Result | `bool` | `True` if the install succeeded. |

### Common

| Name | Display Name | Kind | Type | Default | Description |
|------|-------------|------|------|---------|-------------|
| `ContinueOnError` | Continue On Error | InArgument | `bool` | `false` | Continue executing the automation if this activity fails. |
| `DelayBefore` | Delay Before | InArgument | `double` | | Delay (in milliseconds) before the install. |
| `DelayAfter` | Delay After | InArgument | `double` | | Delay (in milliseconds) after the install. |

## XAML Example

Install a local APK:

```xml
<uma:InstallApp DisplayName="Install App"
                AppPath="C:\Apps\BasicAppAndroid.apk"
                Result="[installOk]" />
```

Install from a URL:

```xml
<uma:InstallApp DisplayName="Install App"
                AppPath="https://uipathtestappsto.blob.core.windows.net/testapps/BasicAppAndroid.apk"
                Result="[installOk]" />
```

Namespace required:
* `xmlns:uma="clr-namespace:UiPath.MobileAutomation.Activities;assembly=UiPath.MobileAutomation.Activities"`

## How to create a new Install App

```bash
uip rpa get-default-activity-xaml --activity-class-name UiPath.MobileAutomation.Activities.InstallApp
```

## Notes

- Must be placed inside a `MobileDeviceConnection` scope.
- For Android, the device must allow installation from external sources (the `appium:autoGrantPermissions` capability handles most permission prompts; for `INSTALL_UNKNOWN_SOURCES` you may need to enable it manually on the device).
- For iOS, the IPA must be signed for the connected device's provisioning profile.
- If the app is already installed, behaviour depends on the platform and the `noReset` / `fullReset` capabilities. To force a clean reinstall, uninstall first with `ManageOtherApp` (`Command = Remove`).
- The activity does **not** launch the app after installation — chain a `ManageCurrentApp` (`Command = Launch`) or `ManageOtherApp` (`Command = Activate`) if you want the app started.
- Companion activities: `ManageOtherApp` (uninstall / activate / terminate), `ManageCurrentApp` (launch / reset the app currently configured on the connection).
