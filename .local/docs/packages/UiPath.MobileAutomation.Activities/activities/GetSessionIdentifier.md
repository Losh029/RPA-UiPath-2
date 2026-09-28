# Get Session Identifier

`UiPath.MobileAutomation.Activities.GetSessionIdentifier`

Retrieve the unique session ID of the active Appium session.

**Package:** `UiPath.MobileAutomation.Activities`
**Category:** Mobile Device
**Required Scope:** `UiPath.MobileAutomation.Activities.MobileDeviceConnection`

## Properties

### Output

| Name | Display Name | Type | Description |
|------|-------------|------|-------------|
| `SessionId` | Session Id | `string` | The Appium session identifier (a UUID-shaped string). |

### Common

| Name | Display Name | Kind | Type | Default | Description |
|------|-------------|------|------|---------|-------------|
| `ContinueOnError` | Continue On Error | InArgument | `bool` | `false` | Continue executing the automation if this activity fails. |
| `DelayBefore` | Delay Before | InArgument | `double` | | Delay (in milliseconds) before the read. |
| `DelayAfter` | Delay After | InArgument | `double` | | Delay (in milliseconds) after the read. |

## XAML Example

```xml
<uma:GetSessionIdentifier DisplayName="Get Session Identifier"
                          SessionId="[sessionIdentifier]" />
```

Namespace required:
* `xmlns:uma="clr-namespace:UiPath.MobileAutomation.Activities;assembly=UiPath.MobileAutomation.Activities"`

## How to create a new Get Session Identifier

```bash
uip rpa get-default-activity-xaml --activity-class-name UiPath.MobileAutomation.Activities.GetSessionIdentifier
```

## Notes

- Must be placed inside a `MobileDeviceConnection` scope.
- Useful when integrating with cloud device farms (Sauce Labs, BrowserStack) that index sessions by ID — pass `SessionId` to their API to fetch session video / logs.
- Also useful for low-level driver calls: combine with `ExecuteCommand` or direct HTTP calls to `{AppiumUrl}/session/{SessionId}/...`.
