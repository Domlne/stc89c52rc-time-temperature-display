# STC89C52RC Time & Temperature Display

Bare-metal 8051 firmware for STC89C52RC.  
It reads temperature from a DS18B20 sensor and displays time and temperature on an LED matrix.

## Features

- STC89C52RC bare-metal C firmware
- DS18B20 1-Wire temperature measurement
- LED matrix dynamic scanning display
- Timer interrupt based time keeping
- Display time (HH:MM:SS) and temperature (°C)
- UART debug output and time setting
- EEPROM parameter storage
- Modular drivers: GPIO, timer, interrupt, UART, I2C, DS18B20, LED matrix

## Hardware

| Component | Model | Notes |
|---|---|---|
| MCU | STC89C52RC | 11.0592MHz crystal |
| Temperature sensor | DS18B20 | 1-Wire, 4.7k pull-up |
| Display | LED matrix | 8x8 / 16x16 |
| RTC | Timer software RTC / DS1302 | see Time Source |
| EEPROM | AT24C02 / STC IAP | optional |
| USB-UART | CH340 | 9600 8N1 |

## Pinout

| STC89C52RC Pin | Connected to | Note |
|---|---|---|
| P1.0 | DS18B20 DQ | 4.7k pull-up to VCC |
| P2 | LED matrix rows | current limit resistors |
| P0 | LED matrix columns | transistor driver |
| P3.0 | UART TX | to CH340 RX |
| P3.1 | UART RX | to CH340 TX |
| P3.2 | Key | set time / switch display |

> Replace this table with your actual wiring.

## Time Source

If no external RTC is used:

- Time is maintained by Timer0 interrupt.
- Time resets after power-off.
- Time can be set through UART or keys.

If DS1302 is used:

- Time is maintained by DS1302 with backup battery.
- MCU reads time from DS1302.

## Display Logic

- The LED matrix scans row by row in a timer interrupt.
- Display alternates between time and temperature.
- Example:
  - 0-3s: show time `12:30`
  - 3-6s: show temperature `25.6C`
  - Press key to switch manually.
- Avoid flicker by keeping the scan interval stable.
