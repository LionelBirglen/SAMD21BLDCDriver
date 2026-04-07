# SAMD21 BLDC Driver for the GM3506 Motor
Brushless motor driver board based on a SAMD21 microcontroller and DRV8313 driver with a form factor fitting the IFlight GM3506 Motor<br>
<img src="Media/Circuit_3DCADView.png" alt="Driver" width="600">

This driver is inspired by the project from Jordan Cormack that is presented there: https://cormack.xyz/L433motordriver/ but being far more familiar with the SAMD21 microntroller, I used that one instead of the STM32 of Dr. Cormack's design. The input lines PA08 and PA09 are also now using I2C instead of CAN bus. Therefore, they are simpler to use careful, a big drawback it that it means that these pins are now only 3.3V tolerant. Two 3.3k pullup resistors are integrated to these input lines. The whole board is pin for pin compatible with the Seeeduino XIAO SAMD21 embodiment (https://wiki.seeedstudio.com/Seeeduino-XIAO/) so you can use their firmware to flash the board and then, program it using the USB-C port. The input lines then become D4 and D5 for you to use as you like (I2C or GPIO). See: https://emalliab.wordpress.com/2023/03/12/unbricking-a-seeed-xiao-samd21/ for instructions on how to flash the board through the SWIO 2x5 connector, key pin is indicated by the K letter on the silkscreen. In the following, Seeedstudio equivalent lines are indicated betweeen parentheses. The board has only two electrical layers so it is dirt cheap to manufacture.<br>

The controllers are daisy chainable and approx. 8A are acceptable based on a temperature estimation by software so up to 8 GM3506 motors could be connected in series. Maximal input voltage on the XT30 power lines is 60V. Maximal output current per board is around 2-2.5A, depending on cooling and ambient temperature. There is an available connector on the board that is compatible with the 20mm fan BSB0203HA3-00CER (Digikey ref. 603-2068-ND) but after experimenting with the latter I noticed its cooling power is frankly quite limited and if you block the fins by accident the current drawn will shoot up and cause a brownout of the SAMD21 which is not good so be careful if you want to use active cooling. Alternatively, this connector could be used to power additional components, there are three lines: GND, NC, and 3.3V.<br>

To control the DRV8313 power driver the following lines are used: PA02 (D0) is Enable, PB08 (D6) is PWM1, PA04 (D1) is PWM2, PB09 (D7) is PWM3. Low side total current sensing is availabe through a 0.1 ohm power resistor connected to the PA10 line (A2). An AS5048A encoder chip is mounted on the backside of the board and connected to the SPI port: MISO is PA05 (D9), MOSI is PA06 (D10), CLK is PA07 (D8), SS is PA11 (D3). The decoupling capacitors close to that chip are recommended but can be left unpopulated.

There are four LEDs on the board, the red one indicates 3.3 power is active. The other three are available to the user, if you use the Seeestudio firmware, one will blink during serial communication and the other two are connected to the Rx/Tx lines. Powering the board is done through either the USB lines or the XT30 power connector. Switching from one to the other is automatic as both power lines are OR-ed and protected with 1N5819WS diodes. For actual control of the motor, the simplefoc library works very well and is recommended, see: https://simplefoc.com/. CAD models for 3D printing enclosures are also provided.<br>

Contents:
- Circuit/: interactive html BOM and 3D step model (board)
- Enclosure/: 3D files for two versions of a proposed enclosure for the motor and board
- Gerber/: zip file with the gerber and drill files for manufacturing. PCA files are also provided if factory assembly is desired: bom and verified component location files
- Media/: general pics and videos

Video:<br>
<img src="Media/SAMD21Driver_demo.gif" alt="Demo">
Demonstrating the position control of two motors in series. When the white button is pressed, one motor turns 180 degrees and the other 360 degrees. Measured response time is around 180 ms.

Prof. Lionel Birglen<br>
Polytechnique Montreal, 2026<br>
License: GNU GPL v3
