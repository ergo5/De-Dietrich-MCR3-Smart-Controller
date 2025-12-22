# OpenTherm Protocol v2.2 - Data-ID Overview Map

Based on OpenTherm Protocol Specification v2.2.

| ID | Msg | Data Object | Type | Description |
| :--- | :--- | :--- | :--- | :--- |
| 0 | R/W | Status | flag8 / flag8 | Master and Slave Status flags. |
| 1 | W | TSet | f8.8 | Control setpoint ie CH water temperature setpoint (°C) |
| 2 | W | M-Config / M-MemberIDcode | flag8 / u8 | Master Configuration Flags / Master MemberID Code |
| 3 | R | S-Config / S-MemberIDcode | flag8 / u8 | Slave Configuration Flags / Slave MemberID Code |
| 4 | W | Command | u8 / u8 | Remote Command |
| 5 | R | ASF-flags / OEM-fault-code | flag8 / u8 | Application-specific fault flags and OEM fault code |
| 6 | R | RBP-flags | flag8 / flag8 | Remote boiler parameter transfer-enable & read/write flags |
| 7 | W | Cooling-control | f8.8 | Cooling control signal (%) |
| 8 | W | TsetCH2 | f8.8 | Control setpoint for 2e CH circuit (°C) |
| 9 | R | TrOverride | f8.8 | Remote override room setpoint |
| 10 | R | TSP | u8 / u8 | Number of Transparent-Slave-Parameters supported by slave |
| 11 | R/W | TSP-index / TSP-value | u8 / u8 | Index number / Value of referred-to transparent slave parameter. |
| 12 | R | FHB-size | u8 / u8 | Size of Fault-History-Buffer supported by slave |
| 13 | R | FHB-index / FHB-value | u8 / u8 | Index number / Value of referred-to fault-history buffer entry. |
| 14 | W | Max-rel-mod-level-setting | f8.8 | Maximum relative modulation level setting (%) |
| 15 | R | Max-Capacity / Min-Mod-Level | u8 / u8 | Maximum boiler capacity (kW) / Minimum boiler modulation level(%) |
| 16 | W | TrSet | f8.8 | Room Setpoint (°C) |
| 17 | R | Rel.-mod-level | f8.8 | Relative Modulation Level (%) |
| 18 | R | CH-pressure | f8.8 | Water pressure in CH circuit (bar) |
| 19 | R | DHW-flow-rate | f8.8 | Water flow rate in DHW circuit. (litres/minute) |
| 20 | R/W | Day-Time | special / u8 | Day of Week and Time of Day |
| 21 | R/W | Date | u8 / u8 | Calendar date |
| 22 | R/W | Year | u16 | Calendar year |
| 23 | W | TrSetCH2 | f8.8 | Room Setpoint for 2nd CH circuit (°C) |
| 24 | W | Tr | f8.8 | Room temperature (°C) |
| 25 | R | Tboiler | f8.8 | Boiler flow water temperature (°C) |
| 26 | R | Tdhw | f8.8 | DHW temperature (°C) |
| 27 | R | Toutside | f8.8 | Outside temperature (°C) |
| 28 | R | Tret | f8.8 | Return water temperature (°C) |
| 29 | R | Tstorage | f8.8 | Solar storage temperature (°C) |
| 30 | R | Tcollector | f8.8 | Solar collector temperature (°C) |
| 31 | R | TflowCH2 | f8.8 | Flow water temperature CH2 circuit (°C) |
| 32 | R | Tdhw2 | f8.8 | Domestic hot water temperature 2 (°C) |
| 33 | R | Texhaust | s16 | Boiler exhaust temperature (°C) |
| 48 | R | TdhwSet-UB / TdhwSet-LB | s8 / s8 | DHW setpoint upper & lower bounds for adjustment (°C) |
| 49 | R | MaxTSet-UB / MaxTSet-LB | s8 / s8 | Max CH water setpoint upper & lower bounds for adjustment (°C) |
| 56 | R/W | TdhwSet | f8.8 | DHW setpoint (°C) (Remote parameter 1) |
| 57 | R/W | MaxTSet | f8.8 | Max CH water setpoint (°C) (Remote parameters 2) |
| 100 | R | Remote override function | flag8 / - | Function of manual and program changes in master and remote room setpoint. |
| 115 | R | OEM diagnostic code | u16 | OEM-specific diagnostic/service code |
| 116 | R/W | Burner starts | u16 | Number of starts burner |
| 117 | R/W | CH pump starts | u16 | Number of starts CH pump |
| 118 | R/W | DHW pump/valve starts | u16 | Number of starts DHW pump/valve |
| 119 | R/W | DHW burner starts | u16 | Number of starts burner during DHW mode |
| 120 | R/W | Burner operation hours | u16 | Number of hours that burner is in operation (i.e. flame on) |
| 121 | R/W | CH pump operation hours | u16 | Number of hours that CH pump has been running |
| 122 | R/W | DHW pump/valve operation hours | u16 | Number of hours that DHW pump has been running |
| 123 | R/W | DHW burner operation hours | u16 | Number of hours that burner is in operation during DHW mode |
| 124 | W | OpenTherm version Master | f8.8 | The implemented version of the OpenTherm Protocol Specification in the master. |
| 125 | R | OpenTherm version Slave | f8.8 | The implemented version of the OpenTherm Protocol Specification in the slave. |
