# Open Deep Link

`UiPath.MobileAutomation.Activities.OpenDeepLink`

Navigate into a mobile app via a deep-link URL (e.g. `myapp://product/42`).

**Package:** `UiPath.MobileAutomation.Activities`
**Category:** Mobile Application
**Required Scope:** `UiPath.MobileAutomation.Activities.MobileDeviceConnection`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Url` | Url | InArgument | `string` | Yes | | The deep link URL. Format is defined by the target app (e.g. `myapp://products/1234`, `mybank://transfer?amount=50`). |
| `App` | App | InArgument | `string` | | | Optional app identifier — Android package name (e.g. `"com.example.myapp"`) or iOS bundle ID (e.g. `"com.example.myapp"`). When set, the deep link is dispatched directly to this app. When omitted, the OS decides which app handles the link. |

### Common

| Name | Display Name | Kind | Type | Default | Description |
|------|-------------|------|------|---------|-------------|
| `ContinueOnError` | Continue On Error | InArgument | `bool` | `false` | Continue executing the automation if this activity fails. |
| `DelayBefore` | Delay Before | InArgument | `double` | | Delay (in milliseconds) before opening the deep link. |
| `DelayAfter` | Delay After | InArgument | `double` | | Delay (in milliseconds) after opening the deep link. |

## XAML Example

Open a deep link in a specific app:

```xml
<uma:OpenDeepLink DisplayName="Open DeepLink"
                  Url="myapp://products/1234"
                  App="com.example.myapp" />
```

Without specifying the app (OS resolves the handler):

```xml
<uma:OpenDeepLink DisplayName="Open DeepLink"
                  Url="myapp://products/1234" />
```

Namespace required:
* `xmlns:uma="clr-namespace:UiPath.MobileAutomation.Activities;assembly=UiPath.MobileAutomation.Activities"`

## How to create a new Open Deep Link

```bash
uip rpa get-default-activity-xaml --activity-class-name UiPath.MobileAutomation.Activities.OpenDeepLink
```

## Notes

- Must be placed inside a `MobileDeviceConnection` scope.
- The deep link scheme (`myapp://...`) must be registered by the target app. If no app handles the scheme, the activity throws.
- For HTTPS URLs that open in the web browser, use `OpenUrl` instead.
- Setting `App` is the difference between "OS picks a handler" (omitted) and "dispatch to this specific app even if others claim the scheme" (set). Set it when more than one app on the device handles the same scheme.
