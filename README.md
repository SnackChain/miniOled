# miniOled

A lean library for Arduino STM32, to use the ubiquitous SSD1306 0.96" I2C Oled display.  Derived out of __Daniel Turton's OzOled__ project 2014/2015. 

The original target for this library was STM32F030F4P6 using Arduino IDE. 
This target has small flash memory, and the established oled libraries are too large to compile into flash.

The Arduino IDE configuration for this board also has an allocation of I2C pins (PA9/PA10) that conflicts
with how the cheap boards are constructed. They designate PA9 and PA10 as UART TX/RX.

miniOled has __minimalist code__ (just enough to be useful). It also uses __software I2C__ library "SoftWire" to allow 
choice of I2C pins, and smaller I2C driver code space.

Useage:
```
#include "miniOled.h"   // that's it. We have an "Oled" object

Oled.init(sda, scl); // in setup(). sda and scl will default if omitted

Oled.wideFont = true;  // Tiny chars get double-width
Oled.chrSpace = 1;   // inter-char space. Use 1, 2 or 3.
Oled.printChar(chr, X, Y);
Oled.printString(str, X, Y);
Oled.printInt(int_number, X, Y);
Oled.drawLine(page, pattern);
Oled.printNumber(long_number, X, X);
Oled.printNumber(float_num, precision, X, Y);
Oled.drawBitmap(*imagebitmap, X, Y, width, height);
Oled.printBigNumber("2", X, Y);  // Only "0"-"9" or ":"   BigNumbers are 24x32

Oled.setCursorXY(Column, Row);
Oled.clearDisplay();
Oled.setPowerOff();
Oled.setPowerOn();
Oled.setPageMode();  // each (little) number printing is within one page
Oled.setHorizontalMode(); // (little) text can flow down whole display.

A "page" (0-7) is a horizontal slice of display, 8 pixels deep.  8 pages total.
"Pattern" is an 8bit pixel pattern to set which pixel lines to paint across a page.
X and Y are column / row cursor positions. In 8-pixel jumps! So X across is 0 - 15, Y down 0 - 7.

Bignumbers can use X 0-12, Y 0-4, or they fall off the display.
Multiple write without re-setting cursors will move across as expected.
```
There should be no reason that miniOled would not readily work in other scenarios. 

miniOled code is here:
 - https://github.com/BLavery/STM32F030F4P6-Arduino/tree/master/Arduino/libraries/miniOled

You will also need these:
 - __SoftWire__ 2.0 from here: https://www.arduinolibraries.info/libraries/soft-wire
 - __AsyncDelay__ (used by SoftWire) from here: https://github.com/stevemarple/AsyncDelay


