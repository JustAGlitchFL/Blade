# Project Journal

## The Beginning
Concepts and planning are listed in README.md

To start this, I disassembled the bottom half of my SB2. Removing the glued-on bottom cover (a sheet of aluminum) lifts a lithium-ion battery, glued to the cover. Once the battery was disconnected and disposed of, we can see an enclosure CNC machined from a solid aluminum billet. There are two PCBs mounted in the chassis, one larger, containing the BMS, charge connector, USB-C port and controller, and link to the upper section. On the other side, we see a smaller board containing two USB-A 3.0 ports, and a SD card reader. Unscrewing these two PCBs leaves the kayboard and trackpad remaining. The trackpad can be extracted with 6 torx screws, lifting out of the chassis. Lastly, the keyboard is strongly glued to the chassis, as it also serves as the plate for the board. Because of this, I will cut the chassis around the board, so I do not have to extract the board and design a custom plate. 

## Reverse Engineering
As this specific Synaptics touchpad is made specifically for the SB2, I was unable to find a pinout for it. To figure this out for myself, I started at the ribbon cable. It contains 6 traces, 4 thin and 1 thick. Presumably, the two thick traces are 5v+ and GND. (Note: As the connector has 8 pins, with the middle 6 being used, I will be referencing pin numbers per the connector, not the cable). This was confirmed by pin 4 having continuity to the mounting bracket. It was also connected to a filter capacitor, with the other end connecting to the other thick trace, leading me to believe this is 5v+. I called it a night here as it was getting late, but I returned to it the next day to find the data protocol used by this touchpad for communication (likely either PS/2 or USB). It's probably PS/2, as a PS/2 cable has 6 conductors, matching the ribbon cable.

The next day, I was able to find a rough schematic for my laptop. It appears that since the Surface Book 2 centers around a detachable design, the trackpad and keyboard are set up differently than other laptops. A single microcontroller lives on the primary IO board, which has the key matrix connected directly through GPIO, and the trackpad is linked over I2C. This single microcontroller is then connected via USB, providing the signals for both HID components. This means that I should be able to connect the keyboard to a standard microcontroller as if it was an array of mechanical switches, and use another to read the trackpad inputs. This may require some custom software, though.

As these are substantially different projects, I may split this into two highway entries, depending on what acon and Alex say.

# Daily entries after seeing the updated Highway journal guidelines

## 5/28/2025: RE struggles
Hours: 5

I spent a while tracing connections, attempting to identify which were SDA and SCL on the board. I did some more online research and eventually found a more detailed schematic for this. I was able to find which pins in the flex cable corresponded to SDA and SCL, so I can make a driver board. However, I can't figure out what "TRACKPAD_INT" and "TRACK_SPARE" pins are right now. I also found a connector for the other end of the flex cable, so I can use the cable rather than soldering each pin individually. 
