# CJMCU-1038

This is a minimal design board I got from AliExpress (https://pt.aliexpress.com/item/32910111787.html?spm=a2g0s.9042311.0.0.d8a5b90avJR3QS). I have no information other than the silk on the board itself and a schematic diagram someone (dmderev) posted on the stm32duino forum (https://www.stm32duino.com/viewtopic.php?t=3165).

It has a STM32F103C8T6 chip (https://www.st.com/resource/en/datasheet/cd00161566.pdf) with 64kB of flash memory and 20KB of SRAM. There supposedly is an 8Mhz cristal, USB pins are connected to the MCU and there are two user-programable LEDS connected to pins PB11 and PB12. Not all MCU pins are breaked-out but there is a small prototyping area on the board. My board came with unsoldered pin header bars.

Programming must be done via SWD protocol as one of the JTAG pins is not accessible. The board may probably programmed by USB using ST bootloader, but I used a FT2232 USB-serial multiprotocol chip to get access to it.
