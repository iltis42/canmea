# canmea
Aviation specific NMEA transmission over CAN

The CAN protocol is a multi-master, multi-cast, asynchronous, and high speed serial communication protocol. 

This repo is dedicated to an open source standard for CAN in aviation, and includes sending well known NMEA frames and other data between aviation devices such as variometers, wireless bridges, sensors, gadgets like bug-wipers, gear warnings, ballast water valves, GPS devices, anti collission devices and more.

The protocol is directly above the CAN object layer that consist of mainly an ID of 11 bit, a data length code of 4 bit, followed by a variable data field with a maximum of 8 bytes or 64 bit data. The simplifed format of the CAN frame as is appears to users above the object layer is shown here:

<b><ID 11 bit><DLC 4 bit><data 64 bit></b>

To improve performance in noise environment, only the 11 bit ID format, and not the extended format will be used. This eases also the usage of libraries below that may not support extended format. 
The lower the ID, the higher is the priority on the bus.

The ID shall indicate the source of message, and might be used for CPU efficient filtering at destination that might filter 
