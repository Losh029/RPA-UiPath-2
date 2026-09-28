# Get Page Source

`UiPath.MobileAutomation.Activities.GetPageSource`

Dump the current UI hierarchy as XML to a file.

**Package:** `UiPath.MobileAutomation.Activities`
**Category:** Logging & Debugging
**Required Scope:** `UiPath.MobileAutomation.Activities.MobileDeviceConnection`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `PageSourceFilePath` | Page Source File Path | InArgument | `string` | Yes | | Absolute or relative path where the XML page source will be written. |

### Common

| Name | Display Name | Kind | Type | Default | Description |
|------|-------------|------|------|---------|-------------|
| `ContinueOnError` | Continue On Error | InArgument | `bool` | `false` | Continue executing the automation if this activity fails. |
| `DelayBefore` | Delay Before | InArgument | `double` | | Delay (in milliseconds) before the dump. |
| `DelayAfter` | Delay After | InArgument | `double` | | Delay (in milliseconds) after the dump. |

## XAML Example

```xml
<uma:GetPageSource DisplayName="Get Page Source"
                   PageSourceFilePath="diagnostics/page_source.xml" />
```

Namespace required:
* `xmlns:uma="clr-namespace:UiPath.MobileAutomation.Activities;assembly=UiPath.MobileAutomation.Activities"`

## How to create a new Get Page Source

```bash
uip rpa get-default-activity-xaml --activity-class-name UiPath.MobileAutomation.Activities.GetPageSource
```

## Notes

- Must be placed inside a `MobileDeviceConnection` scope.
- The file is written directly by the activity — no extra "save" step required.
- Output XML starts with `<?xml ...>` followed by `<hierarchy>` (Android UIAutomator2) or `<AppiumAUT>` (iOS XCUITest). Each element node carries attributes like `class`, `resource-id`, `content-desc`, `text`, `bounds`.
- For workflows debugging selector drift (an element that used to resolve no longer does), capturing the page source on failure is the standard diagnostic.
- The `MobileDeviceConnection` scope's `CollectPageSource = AfterActivity` setting will auto-dump the page source after every activity — handy for batch debugging.
