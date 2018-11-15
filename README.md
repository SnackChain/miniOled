# miniOled

A lean library for Arduino STM32, to use the ubiquitous SSD1306 0.96" I2C Oled display.  Derived out of __Daniel Turton's OzOled__ project 2014/2015.

 <img align="right" src="P1070121.JPG">The original target for this library was the $3 STM32F030F4P6 board, using Arduino IDE. 
This target has small flash memory, and the established oled libraries are too large to compile into flash. 
miniOled has __minimalist code__ (just enough to be useful). 

It also __optionally uses software I2C__ library "SoftWire" to allow 
choice of I2C pins. The native Arduino IDE Wire configuration for this board has an allocation of I2C pins (PA10/PA9) that conflicts with how the cheap boards are constructed. These (The "STM32F030F4P6 Demo" board) designate PA9 and PA10 as UART TX/RX, 
and connect these to the uart header.

In this code, Oled updates are direct (no canvas buffer) and partial. But it is still slow, not very "snappy." The native I2C method is not much faster than the software method. 

Useage:
```
#include "miniOled.h"   // that's it. We have an "Oled" object

Oled.init(sda, scl); // in setup(). sda and scl will default (PA6/PA5) if omitted
(The sda/scl parameters will be ignored in case of not using SoftWire. PA10/PA9 will be used.)

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
Bignumbers can use X 0-12, Y 0-4, else they fall off the display.
Multiple write without re-setting cursors will move across as expected.
```
<img align="right" src="STM32F030-Dev-Brd.jpg">There should be no reason that miniOled would not 
readily work in other scenarios. 

miniOled code is here:
 - https://github.com/BLavery/STM32F030F4P6-Arduino/tree/master/Arduino/libraries/miniOled

You will also need these for software I2C:
 - __SoftWire__ 2.0 from here: https://www.arduinolibraries.info/libraries/soft-wire
 - __AsyncDelay__ (used by SoftWire) from here: https://github.com/stevemarple/AsyncDelay
 
 In file miniOled.cpp there is a line 
   __#define SOFTWIRE__
 Leave this defined to use software I2C. Undefine it to use regular Arduino (STM32) "Wire.h".
 Using software I2C saves about 1.5k flash.


