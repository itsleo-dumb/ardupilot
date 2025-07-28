# LeoH743 Custom ArduPilot Board

This document describes the creation of a custom ArduPilot board called "LeoH743" based on the MatekH743 board design.

## Files Created

### Hardware Definition Files
- `/home/orangepi/ardupilot/libraries/AP_HAL_ChibiOS/hwdef/LeoH743/hwdef.dat` - Main hardware definition
- `/home/orangepi/ardupilot/libraries/AP_HAL_ChibiOS/hwdef/LeoH743/hwdef-bl.dat` - Bootloader hardware definition

### Board Registration
- Added `AP_HW_LEOH743 2025` to `/home/orangepi/ardupilot/Tools/AP_Bootloader/board_types.txt`

### Generated Files
- `/home/orangepi/ardupilot/Tools/bootloaders/LeoH743_bl.bin` - Bootloader binary
- `/home/orangepi/ardupilot/Tools/bootloaders/LeoH743_bl.hex` - Bootloader hex file
- `/home/orangepi/ardupilot/Tools/bootloaders/LeoH743_bl.elf` - Bootloader ELF file

## Board Specifications

- **MCU**: STM32H743xx (ARM Cortex-M7)
- **Board ID**: 2025 (AP_HW_LEOH743)
- **Flash Size**: 2048KB
- **Crystal Frequency**: 8MHz
- **Bootloader Size**: 128KB

## Hardware Features

### IMUs
- Primary: ICM42688P on SPI1
- Secondary: ICM20602 on SPI4

### Communication Interfaces
- **UART7**: Telemetry 1 with hardware flow control
- **USART1**: Telemetry 2  
- **USART2**: GPS1
- **USART3**: GPS2
- **UART4**: Spare UART
- **UART8**: Spare UART
- **USART6**: RC Input (with alternative bi-directional UART mode)

### Connectivity
- **USB**: OTG1 Full Speed USB
- **CAN**: Single CAN bus interface
- **I2C**: Two I2C buses (I2C1, I2C2)
- **SPI**: External SPI3 bus available

### Power Management
- Dual battery monitoring (voltage and current sensing)
- Multiple ADC channels for voltage/current sensing
- RSSI analog input
- Airspeed sensor input

### Motor Outputs
- 13 PWM outputs using various timers (TIM4, TIM5, TIM8, TIM15)
- WS2812 LED output on PWM13

### Storage
- microSD card support via SDMMC1
- Internal flash storage (32KB parameter storage)

### Other Features
- LED indicators (Blue ACT, Green B/E)
- Buzzer/Beeper support
- External sensor CS pins
- GPIO pins for various functions

## Build Process

1. **Create board directory structure**:
   ```bash
   mkdir -p /home/orangepi/ardupilot/libraries/AP_HAL_ChibiOS/hwdef/LeoH743
   ```

2. **Add board ID to board_types.txt**:
   Added `AP_HW_LEOH743 2025` entry

3. **Build bootloader**:
   ```bash
   cd /home/orangepi/ardupilot
   Tools/scripts/build_bootloaders.py LeoH743
   ```

4. **Configure for board**:
   ```bash
   ./waf configure --board LeoH743
   ```

5. **Build firmware** (example for copter):
   ```bash
   ./waf copter
   ```

## Board Verification

The board has been successfully:
- ✅ Recognized by the build system (`./waf list_boards` shows LeoH743)
- ✅ Bootloader builds without errors
- ✅ Main firmware configuration completes successfully
- ✅ Hardware definition parsing works correctly

## Usage

This board can be used as a drop-in replacement for MatekH743 boards or as a base for further customization. All standard ArduPilot vehicle types (Copter, Plane, Rover, etc.) should work with this board definition.

To use this board:
1. Build firmware with `--board LeoH743`
2. Flash the bootloader to the target hardware
3. Flash the main firmware (.apj file) via bootloader or programming interface

## Customization Notes

The board definition is based on MatekH743 and inherits all its features. You can customize:
- Pin assignments in hwdef.dat
- Default parameters
- IMU configurations
- Additional sensors
- Custom defines and features

## Board ID

The board uses ID 2025 (AP_HW_LEOH743) which should be unique and not conflict with other boards.
