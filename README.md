# Anenji 11kW Inverter Native ESPHome Integration

This project provides a native ESPHome configuration for the **Anenji 11kW Hybrid Inverter**. Unlike external plug-and-play solutions, this implementation involves replacing the original internal Wi-Fi controller with a **Seeed Studio XIAO ESP32-C6**, powered directly by the inverter's internal board.

## 🚀 Key Features & Findings
- **Protocol:** Modbus RTU (9600 bps).
- **Registers:** Uses **Holding Registers (03H)** for most real-time data.
- **Hardware Mod:** The XIAO ESP32-C6 is soldered/connected to the original controller pins, drawing power from the main board.
- **Port Availability:** This mod leaves the external **RS485 port completely free** for parallel communication or Battery BMS.

### Verified Register Map (Runtime Data)
Based on the **2024 v1.0 Modbus Protocol**:

| Parameter | Modbus Address | Type | Unit | Divider |
|-----------|----------------|------|------|---------|
| **AC Mains Voltage** | 338 | Holding | V | 0.1 |
| **AC Mains Frequency** | 203 | Holding | Hz | 0.01 |
| **Output Voltage** | 606 | Holding | V | 0.1 |
| **Active Output Power** | 254 | Holding | W | 1 |
| **Battery Voltage** | 277 | Holding | V | 0.1 |
| **Battery Current** | 278 | Holding | A | 0.1 |
| **Battery SOC** | 280 | Holding | % | 1 |
| **PV1 Voltage** | 351 | Holding | V | 0.1 |
| **PV Total Power** | 302 | Holding | W | 1 |
| **Inverter Temp** | 231 | Holding | °C | 1 |

### Device Operation Modes (Reg 201)
- `2`: Mains Mode
- `3`: Off-Grid Mode
- `4`: Bypass Mode
- `6`: Fault Mode

## 🔧 Hardware Configuration
The original Wi-Fi plug controller was removed. The XIAO ESP32-C6 is integrated into the original board using:
- **TX:** GPIO17 (D6)
- **RX:** GPIO16 (D7)
- **Power:** Sourced directly from the inverter's internal header.

## ⚠️ Build Notes
For ESP32-C6 on Home Assistant/ESPHome, you **must** limit compilation threads to prevent crashes:
```yaml
esphome:
  compile_process_limit: 1

## 🤝 Credits & Acknowledgments
This integration was made possible by the collective knowledge shared in the solar community. Special thanks to the following resources:

- **[DIY Solar Forum](https://diysolarforum.com/threads/a-hack-to-connect-newer-anenji-aios-w-built-in-wifi-to-solarassistant.119887/)**: For the initial insights on hacking newer Anenji AIOs with built-in Wi-Fi.
- **[ArturHome.pl Forum](https://forum.arturhome.pl/t/esphome-falownik-inwerter-anenji/13229)**: For the detailed discussions on ESPHome configurations for Anenji inverters.
- Additional community contributors and forum threads that helped decode the Modbus registers for these hybrid units.
