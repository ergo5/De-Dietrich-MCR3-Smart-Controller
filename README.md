# De Dietrich MCR3 Smart Controller (ESPHome + OpenTherm)

**Unlock the full potential of your De Dietrich MCR3 boiler with local, smart control.**

This project provides a complete ESPHome configuration to replace the standard thermostat with a smart, Wi-Fi connected controller integrated into Home Assistant. It acts as a "man-in-the-middle" between your boiler and your smart home, offering advanced features often hidden by the manufacturer.

## 🌟 Key Features

*   **Full OpenTherm Integration**: Read status, flame, pressures, and control setpoints directly.
*   **Software Heating Curve**: Advanced weather compensation logic calculated on the ESP (independent of cloud).
*   **Anti-Cycling Protection**: (Optional) Logic to prevent short cycles and extend boiler life.
*   **Efficiency Monitoring**:
    *   **Delta T** (Supply - Return) monitoring to ensure condensing efficiency.
    *   **Modulation Level**: See exactly how hard the burner is working.
    *   **Burner Stats**: Track starts and operation hours.
*   **Max Power Limiter**: Cap the maximum boiler power (e.g., 50%) for shoulder seasons without affecting hot water comfort.

## 🛠️ Hardware Requirements

*   **ESP8266/ESP32 Board**: Wemos D1 Mini is recommended.
*   **OpenTherm Adapter**: Ihor Melnyk's OpenTherm Adapter (or DIYLESS Shield).
*   **DS18B20 Sensors**: For independent Boiler Room and Outdoor temperature readings.

### Wiring (Ihor Melnyk Adapter)

| Wemos D1 Mini | Adapter Pin | Function |
| :--- | :--- | :--- |
| **5V** | **+** | Power (if powered by boiler*) |
| **GND** | **-** | Ground |
| **D2 (GPIO4)** | **OT In** | Data Input (from Boiler) |
| **D1 (GPIO5)** | **OT Out** | Data Output (to Boiler) |

*> **Note:** Ideally, power the Wemos via USB to ensure stable WiFi and clean power.*

## 🚀 Installation

1.  **Clone this repository** (or copy the YAML).
2.  **Edit `mcr3_controller.yaml`**:
    *   Set your `wifi_ssid` and `password`.
    *   Adjust `heating_curve_slope` initial value for your house insulation (default 1.4).
3.  **Flash to ESP8266** using ESPHome Dashboard or CLI.
4.  **Connect to Boiler**:
    *   Connect the OpenTherm adapter wires to the **Showroom/Thermostat** terminals on the MCR3 PCB (consult your manual, usually X9 or similar).
    *   **Warning**: Boiler bus voltage is 24V-30V. Ensure everything is powered off before wiring!

## 📊 Home Assistant Integration

Once flashed and connected, the device will appear in Home Assistant via the ESPHome integration. You will get:
*   **Controls**: Sliders for Heating Curve Slope/Offset, Switches for CH/DHW.
*   **Sensors**: Temperatures, Modulation, Pressure (if supported), Fault Codes.
*   **Diagnostics**: WiFi Signal, Uptime, Burner Stats.

## 🤝 Contributing

This configuration is based on the [esphome-opentherm](https://github.com/arthurrump/esphome-opentherm) component. Feel free to fork and improve!

## 📚 Resources
*   **[OpenTherm Protocol v2.2 Notes](docs/PROTOCOL_NOTES.md)** - extracted Data-ID Overview Map.
*   **[What is OpenTherm?](https://www.opentherm.eu/opentherm-protocol/what-is-opentherm/)** - Official protocol documentation explaining the standard used by this project.

---
*Disclaimer: Use at your own risk. Modifying heating equipment can be dangerous. Verified on De Dietrich MCR3, but should work with Remeha Tzerra/Baxi with minimal changes.*
