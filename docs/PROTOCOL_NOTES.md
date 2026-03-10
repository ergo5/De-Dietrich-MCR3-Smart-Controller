# OpenTherm Protocol v2.2 - MCR3 Compatibility Notes

## Verified Data-IDs on De Dietrich MCR3

### ✅ Working (Read)

| ID | Name | Type | Description |
|----|------|------|-------------|
| 0 | Status | Read | Master/Slave status flags |
| 17 | Rel. Modulation | Read | Relative modulation level (%) |
| 25 | Tboiler | Read | Boiler water temperature |
| 26 | Tdhw | Read | DHW temperature |
| 27 | Toutside | Read | **Outdoor temperature from boiler's analog sensor** |
| 28 | Tret | Read | Return water temperature |
| 120 | Burner Hours | Read | Total burner operation hours |

### ✅ Working (Write)

| ID | Name | Type | Description |
|----|------|------|-------------|
| 1 | TSet | Write | CH setpoint (°C) |
| 14 | MaxRelMod | Write | Maximum relative modulation (%) |
| 56 | TdhwSet | Write | DHW setpoint (°C) |

### ❌ Not Supported by MCR3

| ID | Name | Response | Notes |
|----|------|----------|-------|
| 18 | CH Pressure | DATA_INVALID | Boiler has no pressure sensor connected to OT |
| 116 | Burner Starts | 0xFFFF | Returns max u16, likely not implemented |
| 57 | MaxTSet | DATA_INVALID | Cannot read max setpoint |

## OpenTherm Message Types

```
READ-DATA:   Master requests data from Slave
READ-ACK:    Slave responds with requested data
WRITE-DATA:  Master sends data to Slave
WRITE-ACK:   Slave acknowledges write
DATA-INVALID: Slave doesn't support this Data-ID
```

## Message Format (32 bits)

```
Bit 31-28: Message Type (4 bits)
Bit 27-24: Spare (4 bits)
Bit 23-16: Data-ID (8 bits)
Bit 15-0:  Data Value (16 bits)
```

## Data Value Formats

- **f8.8**: Signed fixed-point (8.8) - ID 1, 14, 25, 26, 27, 28, 56
- **u16**: Unsigned 16-bit integer - ID 116, 120
- **flag8/flag8**: Two 8-bit flag fields - ID 0

## ID 27 (Toutside) Details

The MCR3 with an external analog NTC sensor connected reports outdoor temperature via ID 27.

**Important**: The analog sensor on MCR3 may show temperature ~3-4°C lower than actual due to:
- Sensor placement (cold wall, shade)
- Cable resistance
- A/D converter drift

**Recommendation**: Use DS18B20 digital sensor for accurate outdoor temperature.

## References

- [OpenTherm Protocol Specification v2.2](https://www.opentherm.eu/)
- [ESPHome OpenTherm Component](https://esphome.io/components/opentherm.html)
