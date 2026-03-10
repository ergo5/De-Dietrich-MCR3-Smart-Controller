# MCR3 Remote UART Bridge

WiFi-to-Serial bridge for remote access to De Dietrich MCR3 diagnostic port.

## Purpose

Access the MCR3 "PC" diagnostic port remotely via WiFi, allowing:
- Running Recom software from anywhere on the network
- Reading boiler parameters and diagnostics
- Modifying service settings (with proper access level)

## Hardware Requirements

| Component | Description |
|-----------|-------------|
| Wemos D1 Mini | ESP8266 board |
| Logic Level Converter | 5V ↔ 3.3V bidirectional |
| RJ9 Cable (4P4C) | Phone handset cable |
| USB Power | 5V for Wemos |

## MCR3 "PC" Port Pinout

The "PC" port uses RJ9 (4P4C) connector:

```
     ┌─────────┐
     │ 1 2 3 4 │  (looking at socket on PCB)
     └────┬────┘
          │
Pin 1: VCC (+5V) - DO NOT USE
Pin 2: TX  (MCR3 output)
Pin 3: RX  (MCR3 input)
Pin 4: GND
```

## Wiring Diagram

```
MCR3 "PC" Port          Level Converter         Wemos D1 Mini
┌───────────┐           ┌───────────┐           ┌───────────┐
│           │           │  HV   LV  │           │           │
│ Pin 2 TX ─┼──────────→│ HV1  LV1 │──────────→│ D8 (RX)   │
│ Pin 3 RX ─┼──────────←│ HV2  LV2 │←──────────│ D7 (TX)   │
│ Pin 4 GND─┼───────────│ GND  GND │───────────│ GND       │
│           │           │ HV   LV  │           │           │
│ Pin 1 5V ─┼──────────→│ +5V +3.3V│←──────────│ 3.3V      │
└───────────┘           └───────────┘           └───────────┘
```

> ⚠️ **Warning**: MCR3 uses 5V TTL logic. A level converter is REQUIRED to protect the Wemos!

## Software Setup

### 1. Flash ESPHome

```bash
esphome run mcr3-uart-bridge.yaml
```

### 2. Find Device IP

After boot, device will be available at `mcr3-uart-bridge.local` or check your router.

### 3. Connect via TCP

**Option A: PuTTY (Windows)**
1. Open PuTTY
2. Choose "Raw" connection type
3. Enter IP address and port `6638`
4. Connect

**Option B: com0com + Recom (Windows)**

To use with Recom software, create a virtual COM port:

1. Install [com0com](https://sourceforge.net/projects/com0com/) (virtual COM port driver)
2. Create a COM pair (e.g., COM10 ↔ COM11)
3. Install [com2tcp](http://com0com.sourceforge.net/) or use `socat`
4. Bridge: `com2tcp --baud 9600 \\.\COM10 <ESP_IP>:6638`
5. In Recom, select COM11

**Option C: Linux/macOS**

```bash
# Using socat to create virtual port
socat pty,link=/dev/ttyVirtual,raw tcp:<ESP_IP>:6638

# Or direct terminal access
nc <ESP_IP> 6638
```

## Baud Rate

The default baud rate is **9600**. If communication fails, try:
- 19200
- 38400

Change in `mcr3-uart-bridge.yaml`:
```yaml
uart:
  baud_rate: 19200  # Adjust as needed
```

## Troubleshooting

### No response from MCR3
1. Check wiring (TX/RX might be swapped)
2. Verify level converter is working
3. Try different baud rates
4. Enable UART debug in ESPHome config

### Connection drops
1. Check WiFi signal strength
2. Verify power supply is stable
3. Check for interference

## Integration with Home Assistant

This device appears in Home Assistant but only provides:
- Connection status
- WiFi signal
- Restart button

The actual UART communication is transparent - data flows between TCP socket and MCR3.

## Protocol Notes

The MCR3 uses a proprietary protocol (Recom/Remeha). Known information:
- Detected as "Tzerra Export" in Recom software
- Protocol documentation is not publicly available
- Some reverse-engineering attempts exist (see references)

## References

- [skyboo.net - Connecting MCR3 to PC](https://skyboo.net/2017/03/connecting-dedietrich-mcr3-to-pc-via-serial-connection/)
- [Robert Hekkers - Remeha Interface](http://blog.hekkers.net/tag/remeha/)
- [oxan/esphome-stream-server](https://github.com/oxan/esphome-stream-server)
