# Get Selected Item

`UiPath.MobileAutomation.Activities.GetSelectedItem`

Retrieve the currently selected item text from a Spinner, picker, or dropdown.

**Package:** `UiPath.MobileAutomation.Activities`
**Category:** UI Automation
**Required Scope:** `UiPath.MobileAutomation.Activities.MobileDeviceConnection`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Target` | Target | Property | [`Target`](../common/Target.md) | Yes | | The Spinner / picker / dropdown to read from. |

### Output

| Name | Display Name | Type | Description |
|------|-------------|------|-------------|
| `SelectedItem` | Selected Item | `string` | The text of the currently selected item. |

### Common

| Name | Display Name | Kind | Type | Default | Description |
|------|-------------|------|------|---------|-------------|
| `ContinueOnError` | Continue On Error | InArgument | `bool` | `false` | Continue executing the automation if this activity fails. |
| `Timeout` | Timeout | InArgument | `double` | | Maximum time (in milliseconds) to wait for the target element. |
| `DelayBefore` | Delay Before | InArgument | `double` | | Delay (in milliseconds) before reading. |
| `DelayAfter` | Delay After | InArgument | `double` | | Delay (in milliseconds) after reading. |

## XAML Example

```xml
<uma:GetSelectedItem DisplayName="Get Selected Item"
                     SelectedItem="[selectedValue]">
  <uma:GetSelectedItem.Target>
    <umm:Target FullSelector="&lt;mbl android:className='android.widget.Spinner' id='com.example.basicapp:id/dropdownSpinner' /&gt;" />
  </uma:GetSelectedItem.Target>
</uma:GetSelectedItem>
```

Namespaces required:
* `xmlns:uma="clr-namespace:UiPath.MobileAutomation.Activities;assembly=UiPath.MobileAutomation.Activities"`
* `xmlns:umm="clr-namespace:UiPath.MobileAutomation.Models;assembly=UiPath.MobileAutomation"`

## How to create a new Get Selected Item

```bash
uip rpa get-default-activity-xaml --activity-class-name UiPath.MobileAutomation.Activities.GetSelectedItem
```

## Notes

- Must be placed inside a `MobileDeviceConnection` scope.
- Target a Spinner / picker element directly — not the popup list that appears after tapping it.
- For multi-select controls, only the first selected item is returned. To enumerate all selected items, use `GetAttribute` with `AttributeName = "text"` against each option element instead.
- Companion activity for writing: `SetSelectedItem`.
