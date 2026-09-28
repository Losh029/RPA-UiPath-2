# Open URL

`UiPath.MobileAutomation.Activities.OpenUrl`

Open a URL in the device's active web browser.

**Package:** `UiPath.MobileAutomation.Activities`
**Category:** Mobile Application
**Required Scope:** `UiPath.MobileAutomation.Activities.MobileDeviceConnection`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Url` | Url | InArgument | `string` | Yes | | The URL to open (e.g. `"https://www.example.com"`). |

### Common

| Name | Display Name | Kind | Type | Default | Description |
|------|-------------|------|------|---------|-------------|
| `ContinueOnError` | Continue On Error | InArgument | `bool` | `false` | Continue executing the automation if this activity fails. |
| `DelayBefore` | Delay Before | InArgument | `double` | | Delay (in milliseconds) before opening the URL. |
| `DelayAfter` | Delay After | InArgument | `double` | | Delay (in milliseconds) after opening the URL. |

## XAML Example

```xml
<uma:OpenUrl DisplayName="Open URL"
             Url="https://www.uipath.com" />
```

Namespace required:
* `xmlns:uma="clr-namespace:UiPath.MobileAutomation.Activities;assembly=UiPath.MobileAutomation.Activities"`

## How to create a new Open URL

```bash
uip rpa get-default-activity-xaml --activity-class-name UiPath.MobileAutomation.Activities.OpenUrl
```

## Notes

- Must be placed inside a `MobileDeviceConnection` scope.
- The URL is opened in the device's **currently active web browser**. The session must already be in a web context (set the `browserName` capability on `MobileDeviceConnection` or switch contexts with `ExecuteCommand`).
- For an in-app deep link (e.g. `myapp://product/42`), use `OpenDeepLink` instead.
- For native-app HTTP automation where no browser is involved, use the regular `UiPath.WebAPI.Activities` package — not `OpenUrl`.
