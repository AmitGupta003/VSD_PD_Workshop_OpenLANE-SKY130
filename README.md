# VSD_PD_Workshop_OpenLANE-SKY130
This repository contains the workshop's documentation for advanced physical design using OpenLANE/Sky130.

Contents
------------------------------------------------------------------------------------------------
Day 1: Inception of open-source EDA, OpenLANE and Sky130 PDK
--------------------------------
•How to talk to computers                                                                   •SoC design and OpenLANE
•Get familiar to open-source EDA tools

Day 2: Good floorplan vs bad floorplan and introduction to library cells
-------------------------
•Chip Floor planning and considerations
•Library Binding and Placement
•Cell design and characterization flows
•General timing characterization parameters

Day 3: Design of a library cell using Magic Layout and ngspice characterization
-----------------------
•Labs for CMOS inverter ngspice simulations
•Inception of Layout and CMOS fabrication process
•Sky130 Tech File Labs

Day 4: Pre-layout timing analysis and importance of good clock tree
------------------------------
•Timing modelling using delay tables
•Timing analysis with ideal clocks using openSTA
•Clock tree synthesis TritonCTS and signal integrity
•Timing analysis with real clocks using openSTA

Day 5: Final steps for RTL2GDS
------------------------------------
•Routing and design rule check (DRC)
•Power distribution network and routing
•TritonRoute features


Day-1
-------------------------------------------
How to talk to computers
-------------------------------
Introduction to the QFN-48 package, chip, pads, core, die, and IPs

Take, for example, an Arduino board. The entire board can be defined in terms of a block diagram, including processor, SD RAM chip, VCC, and so on blocks associated with it. There will be numerous pins available on the chip. This chip, as it is commonly known, should be referred to as a package with numerous pins. The course's specific package is QFN-48.
![Screenshot 2023-06-02 215346](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/12b8a453-98cb-4ecb-9071-96ee5b586bc0)

![image](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/f3f3059d-c34c-43aa-81ea-5a6db7a109f1)

The chip is generally placed at the centre of the package and wire bonds are usually used to connect them to the package pins
•The chip will have various components:
• Pads: Through which signals go through
• Core: Place at which the digital logic sits
• Die: The size of the entire chip
If an example of RISC V SoC is taken, the SoC will look like

![Screenshot 2023-06-02 215355](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/7091679f-6957-4a41-b27b-6bfe97fa53c8)

