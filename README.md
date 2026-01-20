# UTDRoboSubCompute
PCB for Custom Raspberry pi CM5 carrier board design for the University of Texas at Dallas RoboSub ROV Team.


Designs are heavily influenced by the RPI CM5 IO board. Its datasheet can be found [here](https://pip-assets.raspberrypi.com/categories/1097-raspberry-pi-compute-module-5-io-board/documents/RP-008182-DS-2-cm5io-datasheet.pdf?disposition=inline).


## Integrated System Documentation: 

### Servo (I2C) Controller: 
IC: PCA9685PW,118
I2C Connection: 
Internally connected to GPIO 2 & GPIO 3 (I2C1). Info [here](pinout.xyz/pinout/i2c)
The Servo controller is on x0 I2C address 

### General Purpose Input Output (GPIO)
The board uses standard RPI GPIO pin outs.
For information [here](https://pinout.xyz/pinout/)

### CSI/DSI Connectors
Both CSI connectors are configured to be enabled out of the box. This may break hat compatibility because the CSI1 uses the I2C lines normally used for identifying hats (see page 10 of the CM5 Docs).


### 
