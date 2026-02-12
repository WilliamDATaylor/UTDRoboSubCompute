# UTDRoboSubCompute2026
Raspberry PI CM5 carrier board designed for the University of Texas at Dallas' ROV.


Designs are heavily influenced by the reference RPI CM5 IO board.

[CM5 IO Board Datasheet](https://pip-assets.raspberrypi.com/categories/1097-raspberry-pi-compute-module-5-io-board/documents/RP-008182-DS-2-cm5io-datasheet.pdf?disposition=inline)

[CM5 IO Board KiCad Files](https://pip.raspberrypi.com/categories/1098-design-files)

[CM5 Datasheet](https://pip-assets.raspberrypi.com/categories/944-raspberry-pi-compute-module-5/documents/RP-008180-DS-6-cm5-datasheet.pdf?disposition=inline)

## Documentation Overview: 

### Servo (I<sup>2</sup>c) Controller: 
![KiCad Schematic for Servo Controller](https://pics.wdat-homelab.xyz/u/i4zgsi.png)
Manufacturer: NXP
IC: PCA9685PW,118

I<sup>2</sup>c Connection: 
Internally connected to GPIO 2 & GPIO 3 (I<sup>2</sup>c1). Info [here](pinout.xyz/pinout/I<sup>2</sup>c)
The Servo controller is on x0 I<sup>2</sup>c address

#### Connecting Servos 
![KiCad Schematic for Servo header connections](https://pics.wdat-homelab.xyz/u/9LitXA.png)
The servos are seperated into two groups of 8 servos for a total of 16 servos. Each group is connected to its own dedicated power input. The pins are just a standard array of 2.54mm pitch headers. Each servo "bank" (a set of 4 servos) has a dedicated resistor network for the PWM output. 

The max current draw for each set of servo "banks" is 5A (for a +20c rise)

### General Purpose Input Output (GPIO)
![Kicad Schematic for 40 pin GPIO Header](https://pics.wdat-homelab.xyz/u/mBmlh4.png)

The board uses standard RPI GPIO pin outs.
[GPIO Documentation](https://pinout.xyz/pinout/)

Details about the GPIO for the CM5 can be on page 12 of the manual.
Probably obvious, but it does need to be said: "Don’t exceed 50 mA for total current load on all 28 GPIO pins."

### CSI/DSI Connectors
![KiCad Schematic for Dual CSI connectors](https://pics.wdat-homelab.xyz/u/YeA2cA.png)
Manufacturer: Hirose Electric
Model Number: FH12-22S-0.5SH(55) 

Both CSI connectors are configured to be enabled out of the box (deviating from the CM5 design). This may break some HAT compatibility because the CSI1 uses the I<sup>2</sup>c lines normally used for identifying hats (see page 10 & 11 of the CM5 docs). Additionally, this will prevent GPIO 0 & 1 from being used as regular pins. (It is entirely possible that this will not cause issues, but your milage may vary)
Both Connectors are usable as DSI connectors as well. 


### Ethernet
![KiCad Schematic for ethernet connectivity](https://pics.wdat-homelab.xyz/u/KgqZxa.png)
Manufacturer: ABRACON
Model Number: ARJM11B1-502-NN-EW2 

The CM5 supports 1Gig ethernet. An ethernet jack with magnetics was used to improve signal integrety.
Our specific use case specifically prohibits the use of POE, so its addition would be unnecessary, thusly the board lacks POE compatability. 

### SD card/Trans Flash boot
![KiCad Schematic for SD Card](https://pics.wdat-homelab.xyz/u/DqQsOR.png)
Connector:
Manufacturer: Amphenol
Model Number: GTFP08432B1HR 

Power IC:
Manufacturer: Richtek
Model Number: RT9742GGJ5

This will only work when a CM5 lite board is in use. This could be ommited if a CM5 with EPROM is used. 

### Power
![KiCad Schematic for the wago power connectors](https://pics.wdat-homelab.xyz/u/RiyaF4.png)

### USB 2.0
dtoverlay=dwc2,dr_mode=host


### USB 3.0

USB 3.0_1 is P/N swapped to avoid using vias or messy fanouts 

### CM5
The CM5 is placed with the wireless antenna pointing inward. As this is meant to be installed inside two inches of aluminum, wifi and bluetooth connectivity have been disregarded. 

### Fan
max power draw: 12w 