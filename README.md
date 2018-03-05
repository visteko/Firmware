# Firmware

Note: Firmware name should be changed to BOOT.bin before put it to SD card. e.g. “BOOT(ID-101, Date- 2018.3.5).bin”in this repo should be firstly modified into "BOOT.bin", then copied into the root directory of SD card in the VISTEKO Tracker device.

## File Description  

### BOOT(ID-101, Date- 2018.3.5).bin
   the updated firmware for VISTEKO Tracker with ID "101", that add "Single Stray Marker" 3D position measurment function. The communication mechanism between the device and PC was implemented with "Request from PC" and then "Answer from Device". The communication protocal in the SDK was described as follows.  
   
### BOOT(ID-103, Date- 2018.3.5).bin
   the updated firmware for VISTEKO Tracker with ID "103".   
   
   
## SDK Protocal  
   **start** -- PC client should firstly send "start" string to the VISTEKO Tracker to enable 3D measurement and tracking functions. If it works, the tracker will reply "vstarted" string to the PC client via TCP/IP.  
   
   **req1**  -- Rigid Body measurement request from the PC client. After "start" cmd was run successfully, string "req1" cmd via TCP/IP will let VISTEKO measure the NDI Polaris_Rigid_Body_(8700339) and report the 3D position and quaternion rotation to the PC client. The orgin of the rigid body followed the specification of NDI. The return packet from VISTEKO Tracker was as following format,  
   > VISTEKO OT packet is composed of "vspTool" + "x" + "y" + "z" + "rms" + "q0" + "q1" + "q2" + "q3" + "vep"  
   
   > HERE is a demo of VISTEKO OT packet "vspTool2x100.00y101.01z1010.10rms0.1234q0-1.0000q10.0000q20.0000q30.0000vep", which means that _vspTool_ is "2", _x_ is "100.00" (in milimeter), _y_ is "101.01", _z_ is "1010.10", _rms_ is "0.1234", _q0_ is "-1.0000", _q1_ is "0.0000", _q2_ is "0.0000", _q3_ is "0.0000". _vep_ is the tag of end of packet.  
   
   > If no rigid body of NDI Polaris_Rigid_Body_(8700339) detected in the current scene, the VISTEKO Tracker will reply "Not enough markers detected for current rigid body" string via TCP/IP.
   
   **req2**  -- string "req2" cmd via TCP/IP will let VISTEKO measure the NDI Polaris_Passive_Probe_(8700340) and report the 3D position and quaternion rotation to the PC client. the return packet from VISTEKO Tracker was same as "req1"  
   
   **reqsm** -- string "reqsm" cmd via TCP/IP will let VISTEKO to measure the 3D postion of Stray Markers in the current scene. The return packet from VISTEKO Tracker was as following format,  
   > VISTEKO OT packet is composed of "vspMarkersCNT" + "count of stray markers" + "x of marker 1" + "y of marker 1" + "z of marker 1" + ["x of marker 2" + "y of marker 2" + "z of marker 2" ......] + "vep"
   
   > HERE is a demo of VISTEKO OT packet "vspMarkersCNT3m1x100.00m1y101.01m1z1010.10m2x200.00m2y202.02m2z2020.20m3x300.00m3y301.01m3z3030.30vep", which means that the count of sing stray markers is "3".   
   The _x_ of single marker 1 is "100.00" (in milimeter), _y_ of single marker 1 is is "101.01", _z_ of single marker 1 is is "1010.10"  
   The _x_ of single marker 2 is "200.00" (in milimeter), _y_ of single marker 2 is is "202.02", _z_ of single marker 2 is is "2020.20"  
   The _x_ of single marker 3 is "300.00" (in milimeter), _y_ of single marker 3 is is "303.03", _z_ of single marker 3 is is "3030.30"  
   _vep_ is the tag of end of packet.  
   
   > If no single marker detected in the current scene, the VISTEKO Tracker will reply "No single marker" string to the PC Client.