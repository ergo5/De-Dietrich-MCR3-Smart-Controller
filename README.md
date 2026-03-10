# 🔥 De Dietrich MCR3 Smart Controller

**ESPHome + OpenTherm integration for De Dietrich MCR3 boiler**

[![ESPHome](https://img.shields.io/badge/ESPHome-2026.2.x-blue)](https://esphome.io/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

Unlock the full potential of your De Dietrich MCR3 boiler with local, smart control. This project provides a complete ESPHome configuration integrated into Home Assistant.

## ✨ Features

| Feature | Description |
|---------|-------------|
| 🔄 **Hybrid Control** | Switch between Autonomous (boiler curve) and Software (ESP curve) modes |
| 🌡️ **Dual Outdoor Sensors** | DS18B20 + boiler's analog sensor (OpenTherm ID 27) |
| 📈 **Software Heating Curve** | Weather compensation with EMA smoothing (anti-oscillation) |
| 📊 **Efficiency Monitoring** | Delta T, Modulation %, Burner Hours, Flame Cycles |
| ⚡ **Power Limiter** | Cap maximum boiler power for shoulder seasons |
| 🔥 **Fireplace Override** | Auto-reduce CH setpoint when fireplace is active (hysteresis) |
| 🛡️ **Failsafe** | Auto-fallback to autonomous mode on sensor failure |
| 💾 **Flash Persistence** | All settings survive power outages (ESP8266 `restore_from_flash`) |

## 🛠️ Hardware Requirements

| Component | Recommendation |
|-----------|----------------|
| **MCU** | Wemos D1 Mini (ESP-12F) |
| **OpenTherm Adapter** | [Ihor Melnyk Shield](http://ihormelnyk.com/opentherm_adapter) or DIYLESS |
| **Outdoor Sensor** | DS18B20 (waterproof) |
| **Power** | USB 5V (recommended for stable WiFi) |

### Pin Configuration

| Function | Pin | GPIO |
|----------|-----|------|
| OpenTherm IN | D2 | GPIO4 |
| OpenTherm OUT | D1 | GPIO5 |
| DS18B20 | D5 | GPIO14 |

### Wiring Diagram

```
MCR3 Boiler                    Wemos D1 Mini
┌────────────┐                 ┌────────────┐
│   X9/X10   │                 │            │
│  OT+  OT-  │──────┬──────────│ D2 (IN)    │
│            │      │          │ D1 (OUT)   │
└────────────┘      │          │            │
                    │          │ D5 ────────│──── DS18B20
              [OpenTherm       │            │
               Adapter]        │ 5V/GND ────│──── USB Power
                               └────────────┘
```

> ⚠️ **Warning**: OpenTherm bus is 24-30V. Power off boiler before wiring!

## 📦 Installation

1. **Clone repository**
   ```bash
   git clone https://github.com/ergo5/De-Dietrich-MCR3-Smart-Controller.git
   ```

2. **Create `secrets.yaml`**
   ```yaml
   wifi_ssid: "YourNetwork"
   wifi_password: "YourPassword"
   ota_password: "your_ota_password"
   ap_password: "fallback_ap_password"
   ```

3. **Configure `piec-co-mcr3.yaml`**
   - Adjust `static_ip` for your network
   - Set `heating_curve_slope` for your house insulation (default: 0.8)

4. **Flash via ESPHome**
   ```bash
   esphome run piec-co-mcr3.yaml
   ```

5. **Connect to boiler** (terminals X9 or similar)

## 🎛️ Control Modes

### Mode A: Autonomous (Default)
- Boiler uses its internal heating curve
- ESP monitors only, doesn't send setpoint
- Safe fallback mode

### Mode B: Software
- ESP calculates heating curve from DS18B20
- Sends CH setpoint to boiler via OpenTherm
- Requires working DS18B20 sensor

**Formula:** `T_water = 20 + Slope × (20 - T_outdoor) + Offset`

## 📊 Home Assistant Entities

### Sensors
| Entity | Description |
|--------|-------------|
| `sensor.mcr3_temperatura_co` | Boiler flow temperature |
| `sensor.mcr3_temperatura_powrotu` | Return temperature |
| `sensor.mcr3_delta_t` | Efficiency indicator (Flow - Return) |
| `sensor.mcr3_temp_zewnetrzna_ds18b20` | Outdoor temp (ESP sensor) |
| `sensor.mcr3_temp_zewnetrzna_kociol` | Outdoor temp (boiler sensor) |
| `sensor.mcr3_modulacja_palnika` | Burner modulation % |
| `sensor.mcr3_godziny_palnika` | Total burner hours |
| `sensor.mcr3_boot_count` | Number of reboots (diagnostic) |
| `sensor.mcr3_flame_cycles` | Flame ignitions per session (diagnostic) |
| `text_sensor.mcr3_uptime_readable` | Human-readable uptime |

### Controls
| Entity | Description |
|--------|-------------|
| `switch.mcr3_tryb_software` | Toggle Software/Autonomous mode |
| `switch.mcr3_ogrzewanie_co` | CH enable |
| `switch.mcr3_podgrzewanie_cwu` | DHW enable |
| `switch.mcr3_kominek_override_enable` | Fireplace override toggle |
| `number.mcr3_setpoint_co` | CH setpoint (°C) |
| `number.mcr3_setpoint_cwu` | DHW setpoint (°C) |
| `number.mcr3_krzywa_grzewcza_nachylenie` | Heating curve slope |
| `number.mcr3_kominek_override_setpoint` | Fireplace override setpoint (°C) |
| `number.mcr3_max_moc_palnika` | Max burner power (%) |

## 🔧 OpenTherm Data-IDs

Verified working on MCR3:

| ID | Name | Status |
|----|------|--------|
| 0 | Status | ✅ Read |
| 1 | CH Setpoint | ✅ Write |
| 14 | Max Modulation | ✅ Write |
| 17 | Rel. Modulation | ✅ Read |
| 25 | Boiler Temp | ✅ Read |
| 26 | DHW Temp | ✅ Read |
| 27 | **Outdoor Temp** | ✅ Read |
| 28 | Return Temp | ✅ Read |
| 56 | DHW Setpoint | ✅ Write |
| 120 | Burner Hours | ✅ Read |

**Not supported by MCR3:**
- ID 18 (CH Pressure) - returns `DATA_INVALID`
- ID 116 (Burner Starts) - returns `0xFFFF`

## ⚠️ Known Issues

### Boiler Outdoor Sensor Offset
The MCR3's analog outdoor sensor may show temperature ~3-4°C lower than actual. Solutions:
1. Use **Mode B** with DS18B20 (recommended)
2. Adjust boiler's P.09 parameter (service mode) with +4°C offset
3. Check sensor wiring/placement

### Modulation Shows 0%
MCR3 reports 0% modulation even when flame is on. A filter adds minimum 5% when flame detected.

## 📚 Resources

- [OpenTherm Protocol v2.2](docs/PROTOCOL_NOTES.md)
- [What is OpenTherm?](https://www.opentherm.eu/opentherm-protocol/what-is-opentherm/)
- [ESPHome OpenTherm Component](https://esphome.io/components/opentherm.html)
- [Ihor Melnyk Adapter](http://ihormelnyk.com/opentherm_adapter)

## 📜 License

MIT License - Use at your own risk.

> **Disclaimer**: Modifying heating equipment can be dangerous. Verified on De Dietrich MCR3. Should work with similar boilers (Remeha Tzerra, Baxi) with minimal changes.

---

Made with ❤️ for the Home Assistant community
