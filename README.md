# UTDRoboSubCompute2026
Raspberry PI CM5 carrier board designed for the University of Texas at Dallas' ROV team, [RoboSub](https://utdrobosub.org/).

This will compete in the [2026 MateROV competition](https://materovcompetition.org/).



![KiCad 3D render of the PCB ](https://pics.wdat-homelab.xyz/u/gkNJ3p.png)

Designs are heavily influenced by the reference RPI CM5 IO board.

[CM5 IO Board Datasheet](https://pip-assets.raspberrypi.com/categories/1097-raspberry-pi-compute-module-5-io-board/documents/RP-008182-DS-2-cm5io-datasheet.pdf?disposition=inline)

[CM5 IO Board KiCad Files](https://pip.raspberrypi.com/categories/1098-design-files)

[CM5 Datasheet](https://pip-assets.raspberrypi.com/categories/944-raspberry-pi-compute-module-5/documents/RP-008180-DS-6-cm5-datasheet.pdf?disposition=inline)

## Documentation Overview: 

### Table of Contents:

1. [Servo Controller](#servo-i2c-controller)
2. [GPIO Pins](#general-purpose-input-output-gpio)
3. [CSI/DSI](#csidsi-connectors)
4. [SD Card boot](#sd-cardtrans-flash-boot)
5. [Power Inputs](#power)
6. [USB 2.0](#usb-20)
7. [USB 3.0](#usb-30)
8. [CM5](#cm5)
9. [Fan](#fan)
10. [Sizing and dimensions](#sizing-and-dimensions)
11. [Additional Details](#additional-details)



### Servo (I<sup>2</sup>c) Controller: 
![KiCad Schematic for Servo Controller](https://pics.wdat-homelab.xyz/u/i4zgsi.png)

Manufacturer: NXP

IC: PCA9685PW, 118

I<sup>2</sup>c Connection: 

Internally connected to GPIO 2 & GPIO 3 (I<sup>2</sup>c1). Info [here](pinout.xyz/pinout/i2c)

The Servo controller is on the x0 I<sup>2</sup>c address

#### Connecting Servos 
![KiCad Schematic for Servo header connections](https://pics.wdat-homelab.xyz/u/9LitXA.png)
The servos are seperated into two groups of 8 servos for a total of 16 servos. Each group is connected to its own dedicated power input. The pins are just a standard array of 2.54 mm pitch headers. Each servo "bank" (a set of 4 servos) has a dedicated resistor network for the PWM output. 

The max current draw for each set of servo "banks" is *5A*. Realistically, the servos will draw a fraction of this during normal usage, but peaks could be high enough for it to matter. The traces were calibrated for these servos:

[Servos Injora 35KG Brushless Servos](https://www.amazon.com/dp/B0B56SN46D)

### General Purpose Input Output (GPIO)
![KiCad Schematic for 40-Pin GPIO Header](https://pics.wdat-homelab.xyz/u/mBmlh4.png)

The board uses standard RPI GPIO pin outs.
[GPIO Documentation](https://pinout.xyz/pinout/)

Details about the GPIO for the CM5 can be found on page 12 of the manual.
Probably obvious, but it does need to be said: "Don’t exceed 50 mA for total current load on all 28 GPIO pins."

### CSI/DSI Connectors
![KiCad Schematic for Dual CSI connectors](https://pics.wdat-homelab.xyz/u/YeA2cA.png)

Manufacturer: Hirose Electric

Model Number: FH12-22S-0.5SH(55) 

Both CSI connectors are configured to be enabled out of the box (deviating from the CM5 design). This may break some HAT compatibility because the CSI1 uses the I<sup>2</sup>c lines normally used for identifying HATs (see pages 10 & 11 of the CM5 docs). Additionally, this will prevent GPIO 0 & 1 from being used as regular pins. (It is entirely possible that this will not cause issues, but your mileage may vary.)
Both connectors are usable as DSI connectors as well. 


### Ethernet
![KiCad Schematic for ethernet connectivity](https://pics.wdat-homelab.xyz/u/KgqZxa.png)

Manufacturer: ABRACON

Model Number: ARJM11B1-502-NN-EW2 

The CM5 supports 1 gigabit ethernet. An Ethernet jack with magnetics was used to improve signal integrity.
Our specific use case specifically prohibits the use of POE, so its addition would be unnecessary; thus, the board lacks POE compatibility. 

Two ESD protection diodes were used to minimize the impact of static. Our use case necessitates long cable runs, making this a possible issue. 

IC: TPD4E05U06QDQARQ1
Manufacturer: Texas Instraments

### SD card/TransFlash boot
![KiCad Schematic for SD Card](https://pics.wdat-homelab.xyz/u/DqQsOR.png)
Connector:

Manufacturer: Amphenol

Model Number: GTFP08432B1HR 


Power IC:

Manufacturer: Richtek

Model Number: RT9742GGJ5

This will only work when a CM5 Lite board is in use; if a non-Lite CM5 is used, this can be omitted. 

### Power
![KiCad Schematic for the wago power connectors](https://pics.wdat-homelab.xyz/u/RiyaF4.png)

As previously mentioned, the voltage for the servos is divided to accommodate differing servo voltages. 

The connectors themselves were chosen for their ease of connection and high power rating (10A at 300V).

Connector: 

Manufacturer: Wago

Model Number: 2091-1372


### USB 2.0
![KiCad Schematic for USB 2.0](https://pics.wdat-homelab.xyz/u/2EP8gA.png)

Connector: GSB191217FHR

Manufacturer: Amphenol

The USB 2.0 port uses repurposed data lines originally used for USB OTG. As a result of this change, the following must be added to the ```config.txt``` file:

```dtoverlay=dwc2,dr_mode=host```

For more information, review page 8 in the CM5 Documentation.

The USB 3.0 and 2.0 ports share a current regulator that provides up to 2.1 A across all three ports.

IC: AP22653W6-7

Manufacturer: Diodes Inc.

### USB 3.0
![KiCad Schematic of USB 3.0 Ports](https://pics.wdat-homelab.xyz/u/CMJG52.png)

USB 3.0_1 is P/N swapped to avoid using vias or messy fanouts 

As previously mentioned, the USB ports share a current regulator. 

### CM5
![KiCad Schematic for Amphenol FFC Connectors for CM5](https://pics.wdat-homelab.xyz/u/6KXRks.png)


The CM5 is placed with the wireless antenna pointing inward. As this is meant to be installed inside a solid aluminum chassis and several meters underwater, Wi-Fi and Bluetooth connectivity have been disregarded. Further, the CM5 that we tested with does not support wireless, so your mileage may vary.

There is an included disable wifi/bluetooth header if necessary for testing.

### Fan
![KiCad Schematic for Fan Header](https://pics.wdat-homelab.xyz/u/2RXl0h.png)

Optional fan header. The schematic uses a generic PC fan header, but any pins with 2.54 mm pitch will work.

Max power draw: 12w 

### Sizing and Dimensions

The PCB itself is roughly a 100mm x 100mm square (real value 100mm x 100.08mm), with four M3 mounting holes 92mm center offset.

![PCB Dimensions](https://pics.wdat-homelab.xyz/u/BImy2G.png)

A 3D model has been included in this GitHub repository for reference. 


### Additional Details

Additional SMD pads for optional bulk and decoupling capacitors have been added to the design if issues arise. 

During assembly, it is highly recommended to use the included interactive BOM (ibom.html).

