# Set Device GeoLocation

`UiPath.MobileAutomation.Activities.SetDeviceGeoLocation`

Mock the device's GPS coordinates. Useful for testing location-based features without physical movement.

**Package:** `UiPath.MobileAutomation.Activities`
**Category:** Mobile Device
**Required Scope:** `UiPath.MobileAutomation.Activities.MobileDeviceConnection`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Latitude` | Latitude | InArgument | `double` | Yes | | Latitude in decimal degrees. Valid range: -90 to 90. |
| `Longitude` | Longitude | InArgument | `double` | Yes | | Longitude in decimal degrees. Valid range: -180 to 180. |
| `Altitude` | Altitude | InArgument | `double` | | `0` | Altitude in meters above sea level. |

### Common

| Name | Display Name | Kind | Type | Default | Description |
|------|-------------|------|------|---------|-------------|
| `ContinueOnError` | Continue On Error | InArgument | `bool` | `false` | Continue executing the automation if this activity fails. |
| `DelayBefore` | Delay Before | InArgument | `double` | | Delay (in milliseconds) before setting the location. |
| `DelayAfter` | Delay After | InArgument | `double` | | Delay (in milliseconds) after setting the location. |

## XAML Example

Mock the device location to San Francisco:

```xml
<uma:SetDeviceGeoLocation DisplayName="Set Device GeoLocation"
                          Latitude="37.7749"
                          Longitude="-122.4194"
                          Altitude="10.0" />
```

Namespace required:
* `xmlns:uma="clr-namespace:UiPath.MobileAutomation.Activities;assembly=UiPath.MobileAutomation.Activities"`

## How to create a new Set Device GeoLocation

```bash
uip rpa get-default-activity-xaml --activity-class-name UiPath.MobileAutomation.Activities.SetDeviceGeoLocation
```

## Notes

- Must be placed inside a `MobileDeviceConnection` scope.
- The device / emulator must support location mocking. Android emulators support it natively. Real devices on Android require developer-mode "Mock location" permission; iOS requires the test app to be signed with appropriate entitlements.
- Some apps cache the last location aggressively — after `SetDeviceGeoLocation`, you may need to trigger a refresh in the app under test (re-open the map, pull to refresh, etc.) for the new value to be visible.
- Latitude / longitude are decimal degrees, **not** degrees-minutes-seconds. Verify that lat/long aren't swapped — a common bug is passing `(longitude, latitude)`.
