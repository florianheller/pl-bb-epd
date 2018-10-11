# pl-bb-epd
## PlasticLogic Beaglebone Pocket User Space Project 

This is a fork of the original PlasticLogic epdc-app containing adaptions to run it on a PocketBeagle-Board instead of a BeagleBone Black or White

The Pin Mapping is as follows:

|	 |HBZ7 Output|	PocketBeagle Port|	BeagleBone Port|
|---|------------|-----------------|---------------------|
|GND|	GND	|GND|	GND|
|GPIO3_14 |	Reserve 1|	P1.36|	P8.19|
|GPIO3_15	|Reserve 2|	P1.33	|P8.11|
|GPIO1_26	|PMIC FLT|	P2.04	|P8.17|
|GPIO1_10	|PMIC POK|	P1.32|	P9.24|
|GPIO2_25	|PMIC EN	|P1.04|	P8.16|
|GPIO3_18	|VCOM_EN / HVSW CTRL|	P1.31	|P9.26|
|GPIO0_26	|RESET#|	P1.34	|P8.14|
|GPIO3_21	|HIRQ	|P1.29	|P9.25|
|GPIO0_20	|HRDY	|P1.20	|P9.27|
|SPI0_MISO (0,3)|	SPI_MISO|	P1.10|	P9.21|
|SPI0_CS0 (0,5)	|SPI_CS	|P1.6|	P9.17|
|SPI0_MOSI (0,4)	|SPI_MOSI|	P1.12|	P9.18|
|SPI0_CLK (0,2)	|SPI_CLK	|P1.8	|P9.22|
|I2C2_SDA (0,12)|	I2C_SDA_A|	P1.26|	P9.20|
|I2C2_SCL (0,13)|	I2C_SCL_A|	P1.28|	P9.19|
|GPIO2_24|	HD/C	|P1.35	|P9.23|
|GPIO1_27	| 5V_EN |P2.02	|P8.15|
|VCC 5V	| VCC 5V	|VCC 5V	|VCC 5V	|



More details will follow soon. 
