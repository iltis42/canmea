# canmea
Aviation specific NMEA transmission over CAN

The CAN protocol is a multi-master, multi-cast, asynchronous, and high speed serial communication protocol.
It is intended to use a fixed <b>1MBit datarate</b> to exclude radio interference in aviation that would occur with lower datarates such as 125kBit/s by the symbol rate and (sub-) harmonics.

This repo is dedicated to an open source standard for CAN in aviation, and includes sending well known NMEA frames and other data between aviation devices such as variometers, wireless bridges, sensors, gadgets like bug-wipers, gear warnings, ballast water valves, GPS devices, anti collission devices and more.

The protocol is directly above the CAN object layer that consist of mainly an ID of 11 bit, a data length code of 4 bit, followed by a variable data field with a maximum of 8 bytes or 64 bit data. The simplifed format of the CAN frame as is appears to users above the object layer is shown here:

<b><ID 11 bit><DLC 4 bit><data 64 bit></b>

To improve performance in noise environment, only the 11 bit ID format, and not the extended format will be used. This eases also the usage of libraries below that may not support extended format. 
The lower the ID, the higher is the priority on the bus.

The ID shall indicate the source of message, and might be used for CPU efficient filtering at destination that might filter or not this type of messages.
  
The ID's are hardcoded and directly linked with the capability of the device that takes part in CAN transmission. Devices are allowed to transmit up to 100 Messages of 8 byte data per second.  


|TAG          |ID      |Purpose|
|-------------|--------|---------------------------------------------------------|
|CN_PRIO_CMD  |0x005   |High Prio Commands, e.g. bugwiper start/stop|
|CN_CMD       |0x010   |Low Prio Commands, e.g. MC, ballast or bugs change|
|CN_NMEA      |0x015   |Standard NMEA messages, e.g. GPGGA, GPRMC, etc.|
|CN_NS_NMEA   |0x020   |Non Standard NMEA messages, e.g. PFLAU, etc.|
|CN_PRIO_SENS |0x025   |High prio sensor data, such as temperature, voltage|
|CN_SENS      |0x030   |Low prio sensor data, such as TE or Baro pressure data|

The maximum length of data to be transmitted is 80 bytes per message, same as for NMEA protocol.
The API is defined as C++ class with featuring an interface to send data and callbacks for receive of data.

The interface may look like this:
 

CANMEA myStation();             
myStation.sendLine( CN_NMEA, "$GPVTG,360.0,T,348.7,M,000.0,N,000.0,K*43" );
float T=25.3;
myStation.sendData( CN_SENS, sizeof(T), &T );

void myCallback( char* msg ){ log(msg); };     // User provided callback for receive
myStation.attach( CN_NMEA, myCallback );



