# Using a Pico with non standard hardware
 Configuring cmake to compile C++ source on a non standard PICO board such as the adafruit QT RP2040

The Raspberry Pi Pico is a great microcontroller for embedded applications. 
There are several different boards designed around the RP2040 chip. These have different hardware than the standard board. 
The Adafruit QT RP2040, for example, has a NeoPixel LED rather than a simple LED and a different flash memory from the standard board. 
In order for your code to compile correctly the board type must be selected in the cmake build.

The PDF for the pico-sdk has a short section on using other boards and this small repository is designed to elaborate a little on how to easily implement the correct board.

When cmake is executed it looks at an ENV variable - in this case PICO_BOARD - to see if is defined. If it is not it defaults to the standard (pico.h).
To compile for a different board you set this ENV variable.

One reason to do this is to easily use the onboard peripherals and to write code that can be ported to different boards.
Also (importantly) you need to set this so the Pico auto starts the code on reset. For example, code compiled using the standard Pico board will run once when you flash the board, but when reset the pico may not run the code again.

The pico-sdk pdf (https://datasheets.raspberrypi.com/pico/raspberry-pi-pico-c-sdk.pdf) says that you can do this by executing 
cmake -D"PICO_BOARD=myboard" ..
in the build directory.
This sets the correct board for the compiler.

At the time of writing the current supported boards are:
* adafruit_feather_rp2040.h
* adafruit_itsybitsy_rp2040.h
* adafruit_qtpy_rp2040.h
* adafruit_trinkey_qt2040.h
* arduino_nano_rp2040_connect.h
* melopero_shake_rp2040.h
* none.h
* pico.h
* pimoroni_interstate75.h
* pimoroni_keybow2040.h
* pimoroni_pga2040.h
* pimoroni_picolipo_16mb.h
* pimoroni_picolipo_4mb.h
* pimoroni_picosystem.h
* pimoroni_plasma2040.h
* pimoroni_tiny2040.h
* pybstick26_rp2040.h
* sparkfun_micromod.h
* sparkfun_promicro.h
* sparkfun_thingplus.h
* vgaboard.h
* waveshare_rp2040_lcd_0.96.h
* waveshare_rp2040_plus_16mb.h
* waveshare_rp2040_plus_4mb.h
* waveshare_rp2040_zero.h

The supported list appears in pico-sdk/src/boards/include/boards/

so, to comfigure your build to use an adafruit QT RP2040, you can navigate touto your_project/build directory and execute:

cmake -D"PICO_BOARD=adafruit_qtpy_rp2040" ..

Then when you make the code the correct board will be used.

A more useful way of doing this is to add a line in your CMakeLists.txt file that instructs cmake how to compile the source.

if you add:

set ( ENV{PICO_BOARD} myboard )

at the top of your CMakeLists.txt file (I have included a simple example in the repository) when you run cmake on the project it will set the correct board to compile to.

So, to compile the code for the adafruit QT RP2040, add:

set ( ENV{PICO_BOARD} adafruit_qtpy_rp2040 )

to your CMakeLists.txt fie
