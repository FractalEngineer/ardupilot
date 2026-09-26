# Fractal D-Wing AIO Flight Controller

This target is for the Fractal D-Wing AIO (`FRENH7WINGAIO`) STM32H743 board. Its
pin assignments are derived from the [pinned Betaflight target configuration](https://github.com/FractalEngineer/config/blob/91fbaf69fe90584723a9a976d31ad75a0bafdfb6/configs/FREN/FRENH7WINGAIO/config.h).

## Default interface assignment

| ArduPilot port | MCU peripheral | Target default |
| --- | --- | --- |
| SERIAL2 | USART2 (PA2/PA3) | SmartAudio |
| SERIAL3 | USART3 (PD8/PD9) | GPS |
| SERIAL5 | USART6 (PC6/PC7) | CRSF receiver |
| SERIAL6 | UART8 (PE0/PE1) | MSP DisplayPort OSD |

The I2C1 bus (PB8/PB9) is external for a compass. The I2C2 bus (PB10/PB11)
contains the onboard BMP388 barometer. PWM outputs 1--11 map to the two motor
outputs, the eight servo outputs and the WS2812 LED strip in the Betaflight
target.

GPIO 81 (`PINIO1`), GPIO 82 (`CAM_SW`) and GPIO 83 (`VTX_PWR`) are mapped to
Relay1, Relay2 and Relay3 respectively. They default to off, off and on.
Assign `RCx_OPTION` 28, 34 or 35 to control Relay1, Relay2 or Relay3 from an
RC switch.

Battery monitoring is enabled by default. Betaflight's voltage scale of 110
gives `BATT_VOLT_MULT = 11.0`, and its current scale of 250 is 40.0 A/V in
ArduPilot's `BATT_AMP_PERVLT` convention.

The onboard MT29F QSPI NAND flash is not yet usable as a dataflash: ArduPilot
support is pending upstream PR
[#32146](https://github.com/ArduPilot/ardupilot/pull/32146). Until then the
board has no logging filesystem, and parameters are stored in internal flash.

Build Plane firmware with:

```sh
./waf configure --board FRACTAL_DWING_AIO
./waf plane
```

The target expects the ArduPilot bootloader at a 128 KiB offset. Install the
matching bootloader through a recovery/DFU method before using normal firmware
updates, and confirm the VTX and camera-switch idle levels on hardware.
