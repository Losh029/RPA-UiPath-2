# Target

`UiPath.MobileAutomation.Models.Target`

The complex model that every target-based Mobile Automation activity uses to locate a UI element on the device. A `Target` describes the element through one or more resolution strategies (selector, fuzzy selector, image, CV) and optionally tweaks the tap point via `TapOffset`.

**Assembly:** `UiPath.MobileAutomation`
**XAML namespace prefix:** `umm:` → `clr-namespace:UiPath.MobileAutomation.Models;assembly=UiPath.MobileAutomation`

## Where it's used

Every activity below takes a `Target` as a required input (inherited from `TargetBaseActivity` or `TargetOffsetableActivity`):

- [Tap](../activities/Tap.md), [Swipe](../activities/Swipe.md)
- [SetText](../activities/SetText.md), [GetText](../activities/GetText.md)
- [GetAttribute](../activities/GetAttribute.md), [ElementExists](../activities/ElementExists.md)
- [GetSelectedItem](../activities/GetSelectedItem.md), [SetSelectedItem](../activities/SetSelectedItem.md)

`TypeText`, `DirectionalSwipe`, `PositionalSwipe`, `DrawPattern`, `MobileDeviceConnection`, `ExecuteCommand`, `OpenUrl`, `OpenDeepLink`, `PressHardwareButton`, `InstallApp`, `ManageCurrentApp`, `ManageOtherApp`, `GetDeviceOrientation`, `SetDeviceOrientation`, `GetDeviceGeoLocation`, `SetDeviceGeoLocation`, `GetSystemTime`, `TakeScreenshot`, `TakeScreenshotPart`, `GetPageSource`, `GetLogs`, `GetLogTypes`, `GetSessionIdentifier` do **not** take a `Target`.

## Properties

The properties below are the ones a XAML author / coding agent should set. The model also exposes internal book-keeping fields (`Id`, `Reference`, `ContentHash`, `Anchors`, `InformativeScreenshot`, `DesignTimeRectangle`, `ImageBase64`, `FriendlyName`, `Occurence`, `Accuracy`, `FuzzyAccuracy`, `CvElementArea`, `CvTextArea`) that Studio fills in during recording — leave them unset in hand-authored XAML.

### Selector resolution

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `FullSelector` | Full Selector | Property | `string` | Literal selector value, e.g. `<mbl android:className='android.widget.Button' id='com.example.basicapp:id/button1' />`. The primary way to identify an element. Hand-authored XAML usually sets this directly. |
| `FullSelectorArgument` | Full Selector | InArgument | `InArgument<string>` | Same purpose as `FullSelector`, but as a runtime expression. Use when the selector must be built from a variable (e.g. `"<mbl android:className='" + cls + "' />"`). |
| `FuzzySelector` | Fuzzy Selector | Property | `string` | Looser, attribute-rich selector used as a fallback when `FullSelector` doesn't resolve. Studio emits this alongside `FullSelector` when recording. |
| `FuzzySelectorArgument` | Fuzzy Selector | InArgument | `InArgument<string>` | Same as `FuzzySelector`, as a runtime expression. |
| `CvText` | Cv Text | Property | `string` | The text to find on the screen using Computer Vision (the `Target` resolves to the bounding rectangle around that text). |
| `CvTextArgument` | Cv Text | InArgument | `InArgument<string>` | Same as `CvText`, as a runtime expression. |
| `SearchSteps` | Search Steps | Property | [`TargetSearchSteps`](#target-search-steps-enum) (Flags) | Ordered list of resolution strategies to try. Default `Selector` (FullSelector only). Combine with `|` for fallbacks, e.g. `Selector, FuzzySelector` or `Selector, CV`. |

### Tap offset (optional, for `Tap` only)

| Name | Display Name | Kind | Type | Default | Description |
|------|-------------|------|------|---------|-------------|
| `TapOffset` | Tap Offset | Property | [`TapOffset`](#tapoffset-sub-model) | `null` | Sub-element that nudges the tap point inside the resolved element's bounds. Useful when a parent element resolves but the hit-zone is offset (e.g. an icon inside a list row). Only honoured by the `Tap` activity. |

## FullSelector grammar

A Mobile Automation selector is a self-closing `<mbl …attributes… />` tag. In XAML it must be HTML-escaped (`<` → `&lt;`, `>` → `&gt;`, `"` → `&quot;`) when written as an attribute value.

```
<mbl <namespace>:<key>='<value>' <namespace>:<key>='<value>' ... />
```

**Android** (UIAutomator2 driver) — common keys, all under the `android:` namespace:

| Key | Maps to | Example |
|-----|---------|---------|
| `android:className` | Java class | `'android.widget.Button'` |
| `id` | Resource id (full or short) | `'com.example.basicapp:id/button1'` |
| `text` | Visible text | `'Submit'` |
| `accessibilityId` | content-desc | `'Submit button'` |
| `package` | App package | `'com.example.basicapp'` |
| `checkable` / `checked` / `enabled` | State attributes | `'true'` / `'false'` |

**iOS** (XCUITest driver) — keys are under the `ios:` namespace, e.g. `ios:type='XCUIElementTypeButton'`, `ios:label='Submit'`, `ios:name='submitButton'`.

Use single quotes around values inside the selector (since the outer attribute is already double-quoted in XAML).

## TapOffset sub-model

`UiPath.MobileAutomation.Models.TapOffset`

| Name | Display Name | Kind | Type | Default | Description |
|------|-------------|------|------|---------|-------------|
| `Position` | Position | Property | [`Position`](#position-enum) | `Center` | Anchor point inside the element from which `OffsetX` / `OffsetY` are measured. |
| `OffsetX` | Offset X | Property | `int` | `0` | Horizontal offset, in the unit set by `TapOffsetType`. |
| `OffsetY` | Offset Y | Property | `int` | `0` | Vertical offset, in the unit set by `TapOffsetType`. |
| `OffsetXArgument` | Offset X | InArgument | `InArgument<int>` | | Same as `OffsetX`, as a runtime expression. |
| `OffsetYArgument` | Offset Y | InArgument | `InArgument<int>` | | Same as `OffsetY`, as a runtime expression. |
| `TapOffsetType` | Coordinates Type | InArgument | `InArgument<CoordinatesTypeEnum>` | `DeviceIndependentPixels` | Whether `OffsetX` / `OffsetY` are in device-independent pixels (DIP) or physical pixels. Required. |

## Target Search Steps enum

`UiPath.MobileAutomation.Enums.TargetSearchSteps` — `[Flags]`. Combine values with `|` (or with a comma when written as a XAML attribute, e.g. `SearchSteps="Selector, FuzzySelector"`).

| Value | Description |
|-------|-------------|
| `None` | Disable all resolution. Rarely useful. |
| `Selector` | Resolve via `FullSelector`. Default. |
| `FuzzySelector` | Fall back to `FuzzySelector` if `FullSelector` does not match. |
| `Image` | Fall back to image-based matching (`ImageBase64`). Recording-time only — agents should not author this. |
| `CV` | Fall back to Computer Vision (`CvText` / `CvType`). Requires `CvServer` configured on the `MobileDeviceConnection` scope. |

When `Selector` matches, the other steps are skipped. The first matching step wins.

## Position enum

`UiPath.MobileAutomation.Enums.Position`

| Value | Description |
|-------|-------------|
| `Center` | Default. Tap at the centre of the element; offset (if any) is measured from the centre. |
| `TopLeft` | Anchor at the element's top-left corner. |
| `TopRight` | Anchor at the element's top-right corner. |
| `BottomLeft` | Anchor at the element's bottom-left corner. |
| `BottomRight` | Anchor at the element's bottom-right corner. |

## XAML Examples

### 1. Static `FullSelector` (most common)

```xml
<uma:Tap DisplayName="Tap Button" TapType="Single">
  <uma:Tap.Target>
    <umm:Target FullSelector="&lt;mbl android:className='android.widget.Button' id='com.example.basicapp:id/button1' /&gt;" />
  </uma:Tap.Target>
</uma:Tap>
```

### 2. `FullSelector` + `FuzzySelector` fallback

```xml
<uma:SetText DisplayName="Set Text"
             Text="user@test.com"
             ClearText="True">
  <uma:SetText.Target>
    <umm:Target FullSelector="&lt;mbl android:className='android.widget.EditText' id='com.example.basicapp:id/editText' /&gt;"
                FuzzySelector="&lt;mbl android:className='android.widget.EditText' id='com.example.basicapp:id/editText' accessibilityId='' text='Add Text' /&gt;"
                SearchSteps="Selector, FuzzySelector" />
  </uma:SetText.Target>
</uma:SetText>
```

### 3. Dynamic selector via `FullSelectorArgument`

Use when the selector value comes from a variable / expression rather than a literal string:

```xml
<uma:Tap DisplayName="Tap Dynamic" TapType="Single">
  <uma:Tap.Target>
    <umm:Target>
      <umm:Target.FullSelectorArgument>
        <InArgument x:TypeArguments="x:String">
          [&quot;&lt;mbl android:className='android.widget.Button' id='&quot; + buttonId + &quot;' /&gt;&quot;]
        </InArgument>
      </umm:Target.FullSelectorArgument>
    </umm:Target>
  </uma:Tap.Target>
</uma:Tap>
```

### 4. `TapOffset` to nudge the tap point

Tap 20 DIP right of the element's centre:

```xml
<uma:Tap DisplayName="Tap with Offset" TapType="Single">
  <uma:Tap.Target>
    <umm:Target FullSelector="&lt;mbl android:className='android.widget.LinearLayout' id='com.example.basicapp:id/row' /&gt;">
      <umm:Target.TapOffset>
        <umm:TapOffset Position="Center"
                       OffsetX="20"
                       OffsetY="0"
                       TapOffsetType="DeviceIndependentPixels" />
      </umm:Target.TapOffset>
    </umm:Target>
  </uma:Tap.Target>
</uma:Tap>
```

### 5. Computer Vision target (`CvText`)

```xml
<uma:Tap DisplayName="Tap by Text" TapType="Single">
  <uma:Tap.Target>
    <umm:Target CvText="Submit"
                SearchSteps="CV" />
  </uma:Tap.Target>
</uma:Tap>
```

Requires `CvServer` / `CvApiKey` set on the parent `MobileDeviceConnection` (or in `project.json`).

## Namespaces required

```xml
xmlns:uma="clr-namespace:UiPath.MobileAutomation.Activities;assembly=UiPath.MobileAutomation.Activities"
xmlns:umm="clr-namespace:UiPath.MobileAutomation.Models;assembly=UiPath.MobileAutomation"
xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
```

Add `x:` only when you use `FullSelectorArgument` / `FuzzySelectorArgument` / `CvTextArgument` / `OffsetXArgument` / `OffsetYArgument` (i.e. the dynamic-expression variants that need `<InArgument x:TypeArguments="...">`).

## Notes

- **`Target` is a model, not a string.** It cannot be set as a single string attribute on the activity — it must be a nested `<umm:Target>` element under `<uma:Activity.Target>`.
- **`FullSelector` is the simplest authoring path.** Set it as a string attribute on `<umm:Target>` and leave `SearchSteps` at its default (`Selector`). Add `FuzzySelector` + `SearchSteps="Selector, FuzzySelector"` only when the strict selector is brittle.
- **`FullSelector` vs `FullSelectorArgument`** — both exist for the same purpose. Use `FullSelector` (the literal string property) for static selectors; use `FullSelectorArgument` (the `InArgument<string>` expression form) only when the selector value must be computed at runtime. If both are set, the argument wins.
- **`Occurence`, `Anchors`, `ImageBase64`, `InformativeScreenshot`, `DesignTimeRectangle`** are populated by Studio during interactive recording and have no useful authoring value — omit them from hand-written XAML.
- **`TapOffset` is only honoured by `Tap`** (and the offsetable inheritance chain). Setting it on a `Target` passed to `SetText` / `GetText` / etc. is silently ignored.
- **iOS vs Android selectors** are not portable: the same logical element has different `<mbl ios:.../>` vs `<mbl android:.../>` selectors. Author per platform.
