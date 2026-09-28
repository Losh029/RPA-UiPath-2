# Set Selected Item

`UiPath.MobileAutomation.Activities.SetSelectedItem`

Set the selected item(s) on a Spinner, picker, or dropdown.

**Package:** `UiPath.MobileAutomation.Activities`
**Category:** UI Automation
**Required Scope:** `UiPath.MobileAutomation.Activities.MobileDeviceConnection`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Target` | Target | Property | [`Target`](../common/Target.md) | Yes | | The Spinner / picker / dropdown to set. |
| `SelectedItem` | Selected Item(s) | InArgument | `string` | | | The item text to select. For multi-select controls, pass multiple items separated by newlines (`\n`). |

### Common

| Name | Display Name | Kind | Type | Default | Description |
|------|-------------|------|------|---------|-------------|
| `ContinueOnError` | Continue On Error | InArgument | `bool` | `false` | Continue executing the automation if this activity fails. |
| `Timeout` | Timeout | InArgument | `double` | | Maximum time (in milliseconds) to wait for the target element. |
| `DelayBefore` | Delay Before | InArgument | `double` | | Delay (in milliseconds) before the selection. |
| `DelayAfter` | Delay After | InArgument | `double` | | Delay (in milliseconds) after the selection. |

## XAML Example

Single-select Spinner:

```xml
<uma:SetSelectedItem DisplayName="Set Selected Item"
                     SelectedItem="String 2">
  <uma:SetSelectedItem.Target>
    <umm:Target FullSelector="&lt;mbl android:className='android.widget.Spinner' id='com.example.basicapp:id/dropdownSpinner' /&gt;" />
  </uma:SetSelectedItem.Target>
</uma:SetSelectedItem>
```

Multi-select (newline-separated items):

```xml
<uma:SetSelectedItem DisplayName="Set Selected Item"
                     SelectedItem="[String.Join(Environment.NewLine, New() {&quot;Option A&quot;, &quot;Option B&quot;})]">
  <uma:SetSelectedItem.Target>
    <umm:Target FullSelector="&lt;mbl android:className='android.widget.ListView' id='com.example.app:id/multiSelect' /&gt;" />
  </uma:SetSelectedItem.Target>
</uma:SetSelectedItem>
```

Namespaces required:
* `xmlns:uma="clr-namespace:UiPath.MobileAutomation.Activities;assembly=UiPath.MobileAutomation.Activities"`
* `xmlns:umm="clr-namespace:UiPath.MobileAutomation.Models;assembly=UiPath.MobileAutomation"`

## How to create a new Set Selected Item

```bash
uip rpa get-default-activity-xaml --activity-class-name UiPath.MobileAutomation.Activities.SetSelectedItem
```

## Notes

- Must be placed inside a `MobileDeviceConnection` scope.
- The `SelectedItem` value is **split on newline characters (`\n`)**. A single value selects one item; multiple newline-separated values are used for multi-select controls (rare on mobile).
- Match against the item's visible text, not its index. If the item with that exact text is not in the list, the activity throws.
- For Spinner / picker controls, `Set Selected Item` opens the popup, taps the matching item, and confirms the selection — you do not need to wrap it in a `Tap` of the Spinner first.
- Companion activity for reading: `GetSelectedItem`.
