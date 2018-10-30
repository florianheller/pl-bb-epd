# pl-bb-epd
## PlasticLogic Beaglebone Pocket User Space Project 

This is a fork of the original PlasticLogic epdc-app containing adaptions to run it on a PocketBeagle-Board instead of a BeagleBone Black or White

## Pin Mapping
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

## Configure Pins at Boot Time

We need to create a system service that wil lbe called every time the system boots. See https://github.com/adafruit/adafruit-beaglebone-io-python/blob/master/test/notes/run_config-pin_during_startup.md 

### Create file `/usr/bin/enable-epdc-pins.sh`
 **sudo nano /usr/bin/enable-epdc-pins.sh**
```
#!/bin/bash
#EPDC Connector
config-pin P1.36 lo
config-pin p1.33 hi
config-pin P2.04 in
config-pin P1.32 in
config-pin P1.04 lo
config-pin P1.31 lo
config-pin P1.34 hi
config-pin P1.29 hi+
config-pin P1.20 in  
config-pin P1.10 spi
config-pin P1.6 hi
config-pin P1.12 spi
config-pin P1.8 spi_sclk
config-pin P1.26 i2c
config-pin P1.28 i2c
config-pin P1.35 hi
config-pin P2.02 lo

sleep 1
```

### Make it executable
**sudo chmod 755 /usr/bin/enable-epdc-pins.sh**

### Create a system service file `/lib/systemd/system/enable-epdc-pins.service`
**sudo nano /lib/systemd/system/enable-epdc-pins.service**
```
[Unit]
Description=Enable pins required for the ePaper display
After=generic-board-startup.service

[Service]
Type=simple
ExecStart=/usr/bin/enable-epdc-pins.sh

[Install]
WantedBy=multi-user.target
```
### Enable the new systemd service
**sudo systemctl daemon-reload**
**sudo systemctl enable enable-epdc-pins.service**
```
Created symlink /etc/systemd/system/multi-user.target.wants/enable-epdc-pins.service → /lib/systemd/system/enable-epdc-pins.service.
```

### Reboot and test
**sudo systemctl status enable-epdc-pins.service**
```
● enable-epdc-pins.service - Enable pins required for the ePaper display
   Loaded: loaded (/lib/systemd/system/enable-epdc-pins.service; enabled; vendor preset: enabled)
   Active: inactive (dead) since Mon 2018-10-15 23:03:11 UTC; 43min ago
  Process: 1475 ExecStart=/usr/bin/config-pins.sh (code=exited, status=0/SUCCESS)
 Main PID: 1475 (code=exited, status=0/SUCCESS)

Oct 15 23:03:04 beaglebone systemd[1]: Started Enable pins required for the ePaper display.
```

## Compile and run the project on a PocketBeagle

### Compile the software
In order to compile this eclipse project, you need a cross compilation toolchain for ARM processors. This means that your computer (with an Intel or AMD CPU can generate machine code that can be executed by an ARM processor). 

I used an Ubuntu Linux in a virtual machine. 
[] TODO: more details here 

### Remote debugging on the PocketBeagle
If you want to be able to set breakpoints while running the software, you need to configure your system for remote debugging. You need the packages gdb and gdbserver to be installed on your pocketbeagle. 

More details will follow soon. 

### Configure the EPDC
**epdc-app -start_epdc 0 1**
```
configparser     DISPLAYTYPE: S049_T1.1
configparser     version - CONFIG_S049_T1.1

configparser     Interface: SPI
s1d13541_controller Using HDC GPIO
configparser     No register settings specified in the configuration file.
main             load_nvm_content?: 0
s1d135xx         Product code: 0x0053
s1d13541_controller Ready 360x240
tps65185         Version: 1.2.5
tps65185         tps65185_apply_timings - not yet implemented
nvm              Used nvm format (1) does not support load waveform from nvm.
generic_epdc     NVM_FORMAT_S1D13541 does not support Wf loading from NVM
generic_epdc     Loading wflib: /boot/uboot/S049_T1.1/display/waveform.bin
generic_epdc     Setting vcom: 4000
vcom             input: 4000, scaled: 4035, DAC reg: 0x7B
tps65185         calculate: 4000, 123
```

***epdc-app -update_image /boot/uboot/S049_T1.1/img/dresden.png***
```
configparser     DISPLAYTYPE: S049_T1.1
configparser     version - CONFIG_S049_T1.1

configparser     Interface: SPI
s1d13541_controller Using HDC GPIO
configparser     No register settings specified in the configuration file.
main             path: /boot/uboot/S049_T1.1/img/dresden.png
main             wfID: 1
main             updateMode: 0
main             updateCount: 1
main             waitTime: 0
main             vcomSwitch: 1
s1d135xx         BW
utils            filename /boot/uboot/S049_T1.1/img/dresden.png
utils            width 720, height 120, bit_depth 8, channels 1
generic_epdc     generic_update: stat: 0
generic_epdc     generic_update: stat: 0
generic_epdc     switch_hvs_on: stat: 0
generic_epdc     generic_update: stat: 0
generic_epdc     generic_update: stat: 0
generic_epdc     switch_hvs_off: stat: 0
generic_epdc     generic_update: stat: 0
```

