# Hardware

## Components

| Component | Purpose |
|-----------|---------|
| ESP32-CAM | Main camera controller |
| OV3660 | Captures fabric images |
| ESP32-WROOM development board | Development and programming support |
| 0.96-inch OLED SSD1306 | Displays the inspection result |
| Wi-Fi | Communicates with the laptop |
| Power supply | Powers the hardware |

## OLED wiring

These are the pins configured in the current FabricGuard code.

| OLED | ESP32-CAM |
|------|-----------|
| VCC | 3.3V |
| GND | GND |
| SDA | GPIO 14 |
| SCL | GPIO 15 |

**OLED test:** the display successfully shows the FabricGuard startup message.

## Camera

The camera is the OV3660. Its pin mapping must match the actual camera board in use. It is not documented here yet.

Current camera settings are listed in [../docs/architecture.md](../docs/architecture.md).

## Still to add

- Circuit diagram
- Component list with exact models and part numbers
