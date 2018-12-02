# STM32F103
Notes on using Arduino_Core_STM332 with "Bluepill"

Arduino-STM32F103

This page entry is merely some personal jottings/observations/experiences using STM32F103, mostly as "Bluepill" 64k flash 'F103C8T6. The 'F103 is ST's take on the arm M3 CPU.

Current "Core" (that code the Arduino's Board Manager option can add) = ST's __Arduino-Core_STM32__,  See https://github.com/stm32duino/Arduino_Core_STM32 .

(For my notes on the smaller STM32F030F4P6, see https://github.com/BLavery/STM32F030F4P6-Arduino .)

The LeafLabs Maple and Maple Mini boards (using the 128k flash version of the 'F103) generated a valiant software industry since about 2011, trying to make the 'F103 work easily with the Arduino IDE. Approach has been to use a flash-resident bootloader to make the M3's native USB ability to give a functioning USB port on the board. That USB port then can allow the Arduino IDE to program a sketch or to use a serial terminal. In time-honoured Arduino UNO fashion. The maple "core" use the "libmaple" library derived from original maple board years. The current version of the old maple-style arduino "core" is "Arduino-STM32" here, still maintained by Roger Clark: https://github.com/rogerclarkmelbourne/Arduino_STM32. 

Note that the Maple or Bluepill boards have no separate USB chip (like FTDI on the UNO or NANO) to do USB. But USB is possible from 2 pins of the M3 itself, if given some driver code (in the maple-derived bootloader).

The newer Arduino_Core_STM32 version of Arduino support, from the ST company, does NOT use a flash-loaded bootloader. So USB is not a possibility for uploading your sketch. Instead, uploading is done either by STLINK adapter, or a 3 volt TTL serial adapter, and these two modes are built into the chip by manufacturer ST. STLINK mode can upload virtually any time. Uploading by serial requires a reset with "boot0" set high, to get to upload mode.

The assignment of MCU pins to traditional Arduino D0 D1 D2 numbering or simply 0 1 2 3 numbering is dependent on configuration files in whichever core you install to your IDE. There are many (very nice) pin assignment images out there in search-space that do not agree with ST's core. I find that the easiest solution is to always in sketches use the PA0 PA1 notation, as that should always be correct no matter your hardware. (That doesn't help for still-available maple mini boards, as they are marked 0 1 2 !)

I have found that these codes/libraries work fine on the ST core and bluepill board:
 - Serial, Serial2 and Serial3 simultaneously
 - I2C native Wire, and native Wire2
 - Softwire I2C
 - Acrobotic OLED: native I2C, or modified slightly to use Softwire.
 - miniOled (see https://github.com/BLavery/miniOled), with native I2C or Softwire
 - UCGlib for ILI9341 SPI TFT display: slight fix for "arm"
 - UCGlib for ILI9163 SPI TFT display: on my display, needed slight fix for that 32-pixel offset devil.
 - FreeRTOS
 - NRFlite (compiles, hardware not tested yet)
 - SD - SD card in a slot
 - SerialFlash on a 16x256 flash chip
 - ADXL345
 - ENC28j60 ethernet: with a bit of tweaking. Flaky. Not really happy. Awaiting a W5500 adapter instead.

__Summary:__
Stuff "just works"
Uploading "just works" - I use STLINK
I'm a happy camper.