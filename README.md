## Programming Sensors with Legacy C vs MRAA, UPM

In this part, we would like to compare how MRAA abstraction and UPM sensor library helps developers to reuse code and develop applications using sensors without any replication of legacy code.

For the sake of timing, we picked a simple sensor working on GPIO pins to address actual process. In this example we will use HC-SR04 Ultrasound sensor, used in college courses to teach students for distance measuring using speed of sound and will use UP2 board.

HC-SR04 sensor has 4 pins, VCC, GND, TRIG and ECHO pins. Actual working principle of sensor is that, when triggered a sound wave is generated by sensor and when it echoed back, it estimates the distance with the speed of sound.

Connecting HC-SR04 to UP2 Board Pins:
https://www.electroschematics.com/8902/hc-sr04-datasheet/

Image-1: HC-SR04 Sensor



Image-2: Electronics of HC-SR04 Sensor



We need two GPIO pins, I will use PIN 13-15 from UP2 GP-Bus Expansion.

In Linux SYSFS:
- PIN 13 corresponds to GPIO 432
- PIN 15 corresponds to GPIO 431

See UP2 board pinout: https://wiki.up-community.org/Pinout_UP2

So let's write our code to read distance continuously from HC-SR4 sensor and print the distance in centimeters until you press 'q' or 'Q':

Basic logic of code is :
Set mode of GPIO pins for TRIG and ECHO pins
Toggle TRIG pin for 10us
Measure the us for ECHO PIN get HIGH
Calculate Distance with formula (duration (uS) / 59.0) in centimeters

Logic is simple but, in order to play with GPIO pins, I had developed 4 methods:
gpio_set_mode
gpio_export
gpio_get_value
gpio_set_value

Code Block:

Build Run Instruction:

$ cd legacy_sys/
$ gcc ultrasound.c -o ultrasound

or

$ mkdir build
$ cmake ../
$ make all

$ sudo ./Ultrasound

What if we wanted to use MRAA?

There wouldn't be any need for reinventing the wheel, so we can just use the MRAA methods:

mraa_gpio_init
mraa_gpio_dir
mraa_gpio_write
mraa_gpio_read

to play with GPIO.

Code Block


Build and Run Instructions

What if we skip MRAA and just use UPM which already have HC-SR04 sensor in its library?

UPM library already does sensor initialisation so no need to access for GPIO pins when we defined the PIN numbers.

Code Block



Build and Run Instructions


