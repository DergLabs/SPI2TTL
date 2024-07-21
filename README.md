# SPI2TTL
FPGA Based SPI to RGB TTL Display Controller

The SPI2TTL board converts an incoming SPI data stream from any 3.3V MCU to a parallel RGB666 TTL interface, designed for use with high resolution TFT/IPS displayes. This project is intended to be used with the Adafruit 3.2" 820x320 IPS ST7701S displays. 

The user facing SPI interface can be configured as a 1x "classic" SPI interface, 4x Quad SPI interface or 8x Octal SPI interface. All interfaces feature two CTRL pins, CTRL0 is used as a Data/Command pin and is required. CTRL1 is used as a reset pin and is optional (reset can be configured to use register command). 

Two 64Mb HyperRam modules are used for storing frame data. Each module operates on an 8-bit 100Mhz DDR Octal SPI inteface, providing 1.6Gb/s of bandwidth. Upto 10 frames can be stored in each ram module. 

When writing to the memory, a "FRAME WRITE BEGIN" command must be sent. Once this command is sent, frame data can be writen. The FPGA will automatically write the data to the unused RAM0 module while the other RAM1 module is used to write frame data to the display. Once all frame data has been writen to RAM0, a "FRAME WRITE END" command is sent. This will switch the display to the RAM0 module containing the newly written data, allowing the user to write new data to the RAM1 module. 

Default display configuration data is stored in blockram on configuration, however new display configuration data may be written using the "CONFIG DISPLAY" command.  

All pixels are stored in RGB888 format to simplify the data handling. The 2 MSB for R, G and B are not used. When using the 1x SPI mode, pixels can be written 18-bits at a time (RGB666). 4x and 8x SPI modes must write pixel data 24-bits at a time (RGB888). 

