# Fractal G-Wing AIO Flight Controller

This target is for the Fractal G-Wing AIO (`FRENGWINGAIO`) STM32F405 board. Its
pin assignments are derived from the [pinned Betaflight target configuration](https://github.com/FractalEngineer/config/blob/884132d1d440eeec796c44d0c04bdae60fcabd52/configs/FREN/FRENGWINGAIO/config.h).

## Default interface assignment

| ArduPilot port | MCU peripheral | Target default |
| --- | --- | --- |
| SERIAL2 | USART2 TX (PA2) | SmartAudio |
| SERIAL3 | USART3 (PC10/PC11) | MSP DisplayPort |
| SERIAL4 | UART4 (PA0/PA1) | CRSF receiver |
| SERIAL5 | UART5 (PC12/PD2) | GPS |

The I2C1 bus (PB8/PB9) is external for a compass. The I2C2 bus (PB10/PB11)
contains the onboard BMP388 barometer. PWM outputs 1--7 map to the two motor
outputs followed by the five servo outputs in the Betaflight target.

Battery monitoring is enabled by default. The 10k/1k divider maps to
`BATT_VOLT_MULT = 11.0`. Betaflight's verified current-meter scale of 195 is
19.5mV/A, so the matching ArduPilot value is `BATT_AMP_PERVLT = 51.28205`.

Build Plane firmware with:

```sh
./waf configure --board FRACTAL_GWING_AIO
./waf plane
```

The target expects the ArduPilot bootloader at a 64 KiB offset. Install the
matching bootloader through a recovery/DFU method before using normal firmware
updates, and confirm the VTX and camera-switch idle levels on hardware.
