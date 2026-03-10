# Changelog

## [5.4.0] - 2026-03-10

### Added
- **EMA Setpoint Smoothing** (alpha=0.3): Prevents oscillations from sudden outdoor temperature changes
- **Boot Counter**: Persistent `Boot Count` sensor to track power outage frequency
- **Flame Cycle Counter**: Per-session `Flame Cycles` sensor to detect short-cycling
- **Human-readable Uptime**: Text sensor showing "Xd Yh Zm" instead of raw seconds
- **Fault Debouncing**: `delayed_on: 5s` on fault indicator to filter transient false alarms
- **ESP8266 Flash Persistence**: `restore_from_flash: true` for reliable state retention across power outages
- **Boot NaN Guard**: Prevents DS18B20 NaN from disabling Software Mode on boot (15s delay + fallback)
- **Switch UI Sync**: Publishes restored `software_mode` state to switch UI on boot

### Changed
- Default heating curve slope: 0.7 → **0.8** (matches production tuning)
- Sensor filters: Added `throttle` + `delta` to all OpenTherm sensors (~90% recorder reduction)
- Sensor precision: `accuracy_decimals: 1` for temperatures, `0` for modulation
- Delta T update interval: 5s → 30s (synchronized with throttled temperature inputs)
- Filter order: `throttle → delta` for CPU efficiency

### Fixed
- EMA stale value after Kominek Override exit (reset via `was_kominek_override` flag)
- Defensive clamp [25°C, 75°C] after EMA smoothing
- `kominek_override_state` now persisted across power outages (`restore_value: true`)

## [5.3.0] - 2026-02-01

### Added
- **Kominek Override**: When fireplace temp >35°C, reduces CH setpoint to configurable value (default 30°C)
- New sensor: `Kominek Temp` - pulls fireplace temperature from Home Assistant
- New switch: `Kominek Override Enable` - toggle feature on/off (default: ON)
- New number: `Kominek Override Setpoint` - configurable setpoint during override (25-40°C)
- New binary_sensor: `Kominek Override Active` - shows when override is active
- Hysteresis: activates >35°C, deactivates <30°C

### Changed
- Reverted safety limits to 25-75°C (mixing valve protects underfloor)
- Default heating curve slope set to 0.7

## [5.2.2] - 2026-01-24

### Fixed
- CH Setpoint NaN issue - added `initial_value: 40` and `restore_value: true`
- Removed `burner_starts` sensor (returns 0xFFFF on MCR3 - not supported)
- Simplified `friendly_name` to "MCR3"

### Changed
- English entity names for international compatibility

## [5.2.1] - 2026-01-24

### Fixed
- Template switch circular reference causing device freeze
- Changed `control_mode` to use `optimistic: true` with global variable

### Added
- `web_server` component for local debugging

## [5.2.0] - 2026-01-24

### Added
- **Hybrid A/B Control Architecture**
  - Mode A (Autonomous): Boiler controls using internal curve
  - Mode B (Software): ESP calculates and sends setpoint
- Outdoor temperature from boiler (OpenTherm ID 27)
- Mode switch with DS18B20 validation
- Auto-fallback to Mode A on sensor failure

## [5.1.0] - 2026-01-24

### Fixed
- Delta T showing NaN - now returns 0 when sensors unavailable
- Modulation showing 0% when flame on - added 5% minimum workaround

### Added
- Max Burner Power control
- Restart button
- Human-readable Uptime display
- Flame status text sensor

### Removed
- CH Pressure sensor (ID 18 not supported by MCR3)

## [5.0.0] - 2026-01-20

### Changed
- Migrated from custom OpenTherm component to **native ESPHome OpenTherm**
- Complete rewrite for ESPHome 2026.1.x compatibility

### Added
- Software heating curve calculation
- DS18B20 outdoor temperature sensor
- Full OpenTherm sensor suite
