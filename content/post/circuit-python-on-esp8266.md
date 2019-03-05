---
title: "CircuitPython on the ESP8266"
date: 2019-01-08T07:43:09-08:00
publishdate: 2019-01-08
lastmod: 2019-01-08
draft: false
tags: [micro-controller, circuit-python]
---

![Blinka image](https://upload.wikimedia.org/wikipedia/en/thumb/5/58/Blinka.png/200px-Blinka.png)


I've been enjoying circuit-python on my [Hallowing](https://www.adafruit.com/product/3900) and [Circuit Playground Express](https://www.adafruit.com/product/3333) boards but don't really want to tie those up with any long term projects, so I decided to dust off my old [ESP8266 Feather Huzzah](https://www.adafruit.com/product/2821) to see if I could get that running circuit-python as well.  Because it lacks built-in USB support, the Huzzah is a little more complicated to get going but really not bad as long as you don't mind the command line.

I'm using Windows 10, but a similar process will work on other operating systems.

# Steps to get setup:
### [Make use of esptool to get the board setup](https://learn.adafruit.com/welcome-to-circuitpython/circuitpython-for-esp8266)
- esptool requires legacy Python 2.7 so I created a conda environment: `conda create --name py27 python=2.7 -y`
- then `pip install esptool`
- [Install the SiLabs CP210x driver](https://www.silabs.com/products/mcu/Pages/USBtoUARTBridgeVCPDrivers.aspx) to make the board's serial port visible.
- [Download the latest CircuitPython ESP8266 firmware file](https://github.com/adafruit/circuitpython/releases/latest)
- Determine which port your esp8266 is connected to.  On Windows 10 just open up device manager, then under the view menu show hidden devices. After that you'll see it under Ports and something like 'Silicon Labs CP210x USB to UART Bridge (COM6)'.
- Erase the flash of ESP8266. Substitute your com port.  Mine was on COM6: `esptool --port COM6 erase_flash`
- Run the following command to load the downloaded firmware file: `esptool --port COM6 --baud 115200 write_flash --flash_size=detect 0 adafruit-circuitpython-feather_huzzah-3.1.2.bin`

### [Then use ampy to move files back and forth](https://learn.adafruit.com/micropython-basics-load-files-and-run-code/)
- Now you can use Python 3 again so change environments if you wish.  I setup another conda environment for this and other micro-controller specific setup.  It's called `micro`.
- `pip install adafruit-ampy`
- After that it's just a matter of running the following **to run** a python script on the board:
`ampy --port COM6 run code.py`
- I don't want to constantly type the port so I set AMPY_PORT environment variable to COM6 like this: `set AMPY_PORT=COM6`
- Now just run `ampy run code.py` **to run** a python script on the board
- run `ampy put code.py` **to write** a python script to the board
- Also see: `ampy --help`

# Some examples:
- to print the pin names available to this board in circuit python:
{{< highlight python >}}
import board
print(dir(board))
{{< /highlight >}}

- Of course we always need a simple blink example:
{{< highlight python >}}
import board
import digitalio
import time

# Setup led pin
LED_PIN = board.GPIO4

# Setup digital output for LED:
led = digitalio.DigitalInOut(LED_PIN)
led.direction = digitalio.Direction.OUTPUT

# blink
while True:
   led.value = True
   time.sleep(0.2)
   led.value = False
   time.sleep(0.2)

{{< /highlight >}}

#### [Here's a diagram of the pinouts for the Feather Huzzah ESP8266](https://learn.adafruit.com/assets/46249)