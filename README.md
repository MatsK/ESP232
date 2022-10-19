# ESP232

https://www.g3gg0.de/wordpress/esp32/esp32-a-tiny-wifi-bt-to-true-rs232-adapter/

Unfortunately it doesn’t always work to fake RS232 using 3.3V lines directly off an ESP32, so I decided to make a custom PCB that features an ESP32 plus a true RS232 Transceiver regarding voltage levels.

My PCB named ESP232 is designed to be quite compact, merely bigger than the footprint of the ESP32 itself. Rotating the ESP32 could have made the PCB a millimeter narrower but a bit longer. But the design intentionally has the USB power plug and antenna on the side as the devices usually are quite long and bump the wall behind anyway.

This time I decided to use an inefficient AMS1117 and not the TPS62291 for the 3.3V supply. It wouldn’t run all day long or drain a battery, so KISS. Maybe I could have saved some mm² space, but the size is dictated by the ESP32 anyway.

The software is built using arduino and supports OTA updates – if you enter update mode by bridging Rx and Tx in the connector (loopback). The bridge gets detected on startup and the firmware enters OTA-only mode.

Why that complicated? I found that the OTA module crashes quite often in my network. (AFAIR it fails to allocate somewhere around the common 1400 bytes using new[] in some UDP receive func). To work around that, I just disable OTA by default. Additionally, this is safer to not accidentally flash the wrong device when updating some other device in hurry.
Of course there is also a rudimentary web interface to select baud rate that gets saved via arduino/ESP32 EEPROM driver.

The serial port is exposed via TCP on the telnet port (23), so you can either connect from your application which supports TCP transport. Or you connect a second ESP32-Board to your computer with a firmware that simply bridges its serial data to the TCP-port of the adapter (USB <-> WiFi <-> RS232).
So your computer sees a COM-port that magically forwards data to the remote (real) RS232. Did a similar thing with one of my 3D printers so cura can print over network.

Unfortunately SmuView doesn’t support the SourceMeter 2400’s SCPI flavour out of the box, so I would have to patch it manually. Sigh :( Maybe somewhen later.

This PCB is not a big thing. Literally. But does its job and it’s cheap.

Schematic/PCB: https://oshwlab.com/EFS-GH/esp32-rs232
Sources: https://github.com/g3gg0/ESP232

Youtube: https://www.youtube.com/watch?v=Nh6mOJvJk3s
