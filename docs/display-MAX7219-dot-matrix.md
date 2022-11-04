## Using MAX7219 based 8x8 LED dot matrix modules (1088AS) to display text

![20211204_133243](https://user-images.githubusercontent.com/24937182/144709719-2fb4ff2c-7645-4dd8-b0a3-eccd306d63f0.jpg)
![20211204_133418](https://user-images.githubusercontent.com/24937182/144709723-833ff002-ef84-431a-b05f-8b7ef30e0072.jpg)

To use this driver, tasmota has to be compiled with:

`#define USE_DISPLAY_MAX7219_MATRIX`



```
  Connect the MAX7219 display module's pins to any free GPIOs of the ESP8266 or ESP32 module.
  VCC should be 5V. Depending on the number of used modules and the brightness, the used current can be more than 500 mA.

  Connect the 5 outgoing pins (VCC, GND, DI, CS, CLK) of the first module to the next one. 
  With this you can connect up to 32 modules. 
  To extend the display hieght, multiple rows are supported. Each module row starts from left to right.

  Assign the pins as follows from Tasmota's GUI:

  DIN hardware pin --> "MAX7219 DIN"
  CS hardware pin --> "MAX7219 CS"
  CLK hardware pin --> "MAX7219 CLK"


  Once the GPIO configuration is saved and the ESP8266/ESP32 module restarts,
  set the Display Model to 19 and Display Mode to 0

  Depending on order of the wired 8x8 matrix modules you have got a display of size pixel_width x pixel_height.
  The size has to be set with the commands "DisplayWidth <pixel_width>" and "DisplayHeight <pixel_height>"

  After the ESP8266/ESP32 module restarts again, turn ON the display with the command "Power 1"


  Now, the following "Display" commands can be used:

  DisplayText  text 
    Sends the text to the display. 
    If the text fits into the display, it is shown in the center.
    Otherwise it scrolls to the left and repeats as long it is cleared or new "DisplayText" overwrites it.

  DisplayDimmer [0..100]
    Sets the intensity of the display.

  Power [ON|OFF]
    Sitches the display on or off. When "off", the display buffer is not cleared and will be shown again when after "Power ON". 
    Other display commands are still active when off.

  DisplayClear
    Clears the display

  DisplayScrollDelay [0..15]   // default = 0
    Sets the speed of text scroll. Smaller delay = faster scrolling.
    The maximum scroll speed is 50ms per pixel on DisplayScrollDelay 0.

  DisplayWidth [8..256]
    Sets the pixel width of the display (8x number of modules in a row)

  DisplayHeight [8..256]
    Sets the pixel height of the display (8x number of module rows)

  DisplayClock  [0|1|2]
    Displays a clock.
    Commands "DisplayClock 1"     // 12 hr format
             "DisplayClock 2"     // 24 hr format
             "DisplayClock 0"     // turn off clock
```
### Constrains:
- Only one text row is supported
- Only standard ascii is supported, UTF8 or escape characters are not supported yet.
- `#define USE_DISPLAY_MAX7219_MATRIX` disables `USE_DISPLAY_MAX7219` and `USE_DISPLAY_TM1637` (made for seven segment displays), cause it is using the same pin config.

