GoBalloon
=========

GoBalloon is a High Altitude Balloon payload controller in Go.   The software is designed to run on a BeagleBone Black single board computer, which provides a full Linux environment while still being lightweight enough to fly in a balloon.  

GoBalloon communicates back to earth via amateur radio and the APRS protocol; the software speaks a layer 2 protocol (AX.25) directly to a radio modem called a Terminal Node Controller (TNC).  Using the TNC, GoBalloon reports its position and altitude periodically and listens for commands from the ground.  When flying, GoBalloon uses a locally-attached TNC via serial port.  For debugging on the ground, GoBalloon also supports a network-attached TNC using KISS-over-TCP and my [tnc-server](https://github.com/chrissnell/tnc-server) software.

GoBalloon is capable of bi-directional communication with the ground and comes with a APRS library that encodes and decodes most of the popular APRS packet formats including position reports (compressed and uncompressed), messages (send/receive/ACK), and telemetry (compressed and uncompressed).

GoBalloon includes GPIO support and will trigger an external cut-down device when a cut-down message is received via APRS messaging.  GPIO is also used to activate a piezoelectric buzzer upon descent to aid searchers looking for the landed payload.

GoBalloon uses [gpsd](www.catb.org/gpsd/) to communicate with its GPS receiver and thanks to gpsd, supports a wide range of GPS devices.  When flying, the software uses a locally-attached GPS via serial port but supports a remote GPS via TCP when debugging on the ground.

What Works
----------
Initial work has been focused on making the communication and telemetry systems functional.  To that end, this is what works right now:

* APRS Controller (sends position reports, receives+acks cutdown messages)
* Balloon cutdown, triggered remotely by APRS message
* NMEA GPS processing / gpsd integration
* AX.25/KISS packet encoding and decoding over local serial line and TCP
* APRS packet parser-dispatcher: examines the raw packets and dispatches appropriate decoder(s)
* APRS position reports encoding and decoding (compressed and uncompressed, with and without timestamps)
* APRS telemetry reports encoding and decoding (compressed and uncompressed)
* APRS messaging
* APRS-IS client (ganked from @dustin)
* APRS-style Base91 encoding

In Progress
-----------
* Reducing CPU consumption
* Flight controls (strobe, buzzer)

Not Yet Complete
----------------
* Camera servo control
* Text-based console for chase vehicles
* PCB design for BeagleBone cape that integrates GPS & TNC modules
* HTTP console for use during pre-flight checks
* More geospatial calculations
* High altitude digipeater
