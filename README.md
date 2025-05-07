# UnoMiniC

UnoMiniC is a compact, USB-C–powered Arduino Uno-compatible board designed for modern embedded development. It features the ATmega328PB and ATmega16U2 for reliable USB-to-serial communication with auto-reset support.

Created to address the size and USB limitations of traditional Arduino boards, UnoMiniC offers a Pico-sized design with breakouts for VIN, ICSP, and DTR reset. The board is powered and programmed through a USB C interface; a VIN pin breakout is provided in case it has to be ran on a battery pack. ICSP headers (as test points) is provided for  both  ATmega16U2  and  ATmega328PB.  On-board  auto  reset  is  configured  to  reset  the microcontroller  to programming  mode  when programming with  the  USB interface.

## Software Reqiurement  

**KiCAD**: Platform used to design the schematic and PCB layout.  
**Atmel FLIP**: Used for updating the atmega16U2 with USB interface firmware.  
**Arduino IDE/VSC**: Programming and testing the final board.

## Board Setup

### Flash USB-to-serial firmware in DFU mode

The firmware for ATmega16U2 is located inside `ArduinoCore-avr/firmwares/atmegaxxu2/arduino-usbserial/`.
Use the `Arduino-COMBINED-dfu-usbserial-atmega16u2-Uno-Rev3.hex` hex file.

> [!IMPORTANT]  
> Make sure that the fuses are set to utilise the external 16 MHz crystal oscillator and disable ```Divide clock by 8 internally``` by setting the ```CLKDIV8 bit``` in the CLKPR – Clock Prescale Register.

### Program the ATmega16U2 chip with USB-Serial firmware

#### Using dfu-programmer (macOS/Linux)

* Download **dfu-programmer** from the [GitHub repository](https://github.com/dfu-programmer/dfu-programmer/releases) or install the dfu-programmer package using a package manager like [homebrew](https://brew.sh/index) (macOS) or [apt-get](https://manpages.ubuntu.com/manpages/kinetic/en/man8/apt-get.8.html) (Ubuntu).

* Connect the board to your computer.

* Set the ATmega16U2 to DFU mode.
   - Short the RST Test Pad to the nearest available GND pin

* Open Terminal.

* Erase the memory.
   ```
   dfu-programmer atmega16u2 erase
   ```
* Flash the firmware.
   ```
   dfu-programmer atmega16u2 ./ArduinoCore-avr/firmwares/atmegaxxu2/arduino-usbserial/Arduino-COMBINED-dfu-usbserial-atmega16u2-Uno-Rev3.hex
   ```
* Disconnect and reconnect the board to your computer.

#### Using FLIP (Windows)

* Download and install [Flip](https://www.microchip.com/en-us/development-tool/flip).

* Connect the board to your computer.

* Set the ATmega16U2 to DFU mode.
   - Short the RST Test Pad to the nearest available GND pin

* Open FLIP.

* Connect to the chip by clicking the cable button, select USB, then click open.

* In the Menu, click Device > Select > ATmega16U2.

* Click File > Load HEX file

* Navigate to `ArduinoCore-avr/firmwares/atmegaxxu2/arduino-usbserial/`, select `Arduino-COMBINED-dfu-usbserial-atmega16u2-Uno-Rev3.hex` and then click OK.

* Click on the `Program Target Device Memory` button.

* The firmware will be flashed, and `Programming done` message should be displayed in the bottom-left corner.

### Download MiniCore 
#### Boards Manager Installation
This installation method requires Arduino IDE version 1.8.0 or greater.

* Open the Arduino IDE.

* Open the **File > Preferences** menu item.

* Enter the following URL in **Additional Boards Manager URLs**:

    ```
    https://mcudude.github.io/MiniCore/package_MCUdude_MiniCore_index.json
    ```

* Open the **Tools > Board > Boards Manager...** menu item.

* Wait for the platform indexes to finish downloading.

* Scroll down until you see the **MiniCore** entry and click on it.

* Click **Install**.

* After installation is complete close the **Boards Manager** window.

#### Manual Installation
Click on the "Download ZIP" button in the upper right corner. Extract the ZIP file, and move the extracted folder to the location "**~/Documents/Arduino/hardware**". Create the "hardware" folder if it doesn't exist.
Open Arduino IDE, and a new category in the boards menu called "MiniCore" will show up.

#### Arduino CLI Installation
Run the following command in a terminal:

```
arduino-cli core install MiniCore:avr --additional-urls https://mcudude.github.io/MiniCore/package_MCUdude_MiniCore_index.json
```

## Setting up the board for the first time

> [!NOTE]
> You will need an ICSP Programmer for this step as we need to flash the URBoot bootloader on the ATmega328PB.

* Under the boards menu: select **MiniCore > ATmega328**.

* Select Clock to **External 16MHZ** and Variant to **328PB**.

* Connect your ICSP programmer to the ICSP header on the back of the board.  

* Under **Tools**, select correct programmer and hit **Burn bootloader**.
  

The board is now setup and the USB C port can be used for serial programming just like any other microcontroller development board.
