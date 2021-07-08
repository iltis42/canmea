# canmea
Aviation specific NMEA transmission over CAN

This repo is dedicated to an open source standard for CAN in aviation, and includes sending well known NMEA frames and other data between aviation devices such as variometers, wireless bridges, sensors, gadgets like bug-wipers, gear warnings, ballast water valves, GPS devices, anti collission devices and more.

The protocol is directly above the CAN object layer that consist of an 11 bit ID and an up to 8 bytes data field:

<b><ID 11 bit><data 64 bit></b>

To improve performance in noise environment, only the 11 bit ID format, and not the extended format will be used. This eases also the usage of libraries below that may not support extended format. 
The lower the ID, the higher is the priority on the bus.

