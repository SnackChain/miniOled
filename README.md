# miniOled

A lean library for Arduino STM32, with SSD1306 0.96" I2C Oled display.  Derived out of Daniel Turton's OzOled project 2014/2015. 

The original target for this library was STM32F030F4P6 using Arduino IDE. 

This target has small flash memory, and regular oled library code is too large to compile into flash.

The Arduino IDE configuration for this board also has an allocation of I2C pins (PA9/PA10) that conflicts
with how the cheap boards are constructed. They designate PA9 and PA10 as UART TX/RX.

miniOled has minimalist code (just enough to be useful). It also uses software I2C library "SoftWire" to allow 
choice of I2C pins, and smaller I2C driver code space.

There should be no reason that miniOled should not readily work in other scenarios.

miniOled code is here:
 - https://github.com/BLavery/STM32F030F4P6-Arduino/tree/master/Arduino/libraries/miniOled

You will also need these:
 - __SoftWire__ 2.0 from here: https://www.arduinolibraries.info/libraries/soft-wire
 - __AsyncDelay__ (used by SoftWire) from here: https://github.com/stevemarple/AsyncDelay


