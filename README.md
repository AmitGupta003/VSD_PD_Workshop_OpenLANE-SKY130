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


Foundry IPs are PLLs, SRAMs, and so forth.

A foundry is essentially a large factory with machinery that create silicon chips.
As a physical design engineer, you must regularly communicate with the foundry.
The visible digital blocks are known as Macros.

There is a significant distinction between macros and IPs; to put it simply, Macros are fundamental digital blocks, whereas IPs require some level of intellect to design and develop.
The foundry sends interface files to companies/individuals so that they can communicate with the foundry.


If an example of RISC V SoC is taken, the SoC will look like
Foundry IPs are the PLL, SRAM, and other components.

A foundry is essentially a sizable factory with equipment for producing silicon chips.
One must regularly communicate with the foundry as a physical design engineer.

The macros are the visible digital blocks.

Between macros and IPs, there is a key difference that can be easily understood by saying that IPs take some level of intellect to generate, whereas macros are fundamental digital building blocks.
The foundry provides companies and individuals with interface files so that they can communicate with the foundry.

![Screenshot 2023-06-02 215355](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/7091679f-6957-4a41-b27b-6bfe97fa53c8)

Introduction to RISC-V
------------------------------
Instruction set architecture (ISA) RISC-V
If a C programme is to be run on a specific layout, it is first compiled into ALP (depending on the architecture) and then transformed into Machine language (binary).
The RISC-V should be implemented using a specific RTL.
The entire process begins with RISC-V architecture, then moves on to RTL, and finally to layout.

From software applications to hardware
------------------------------
Apps are executed on hardware.
The application software enters the System software block, which turns it into binary format that the hardware can interpret.
The following are the main components of system software:
The operating system manages IO activities, allocates memory, and so on.
Compiler: The output is determined by the instruction set.
Assembler: binary output
The instructions of the compiler output will be determined by the chip architecture (RISC-V, x86, etc.): This is an executable file.
Depending on the language, the input to the compiler follows a standard format.
We require an HDL (RTL) that implements the specified implementations from assembler to hardware.
This RTL is synthesised into a netlist of gates, which is then translated into layout.

SoC Design and OpenLANE
-----------------------------
Introduction to all components of open-source digital ASIC Design
----------------------------------
Designing Digital ASICs necessitates a number of factors, including:
EDA Tools for RTL IPs

Data from PDKs (process design kits)
There are numerous open source RTL designs available on the internet, including librecores.org, opencores.org, and github.com.
Some EDA tools, such as Spice simulator and Magic, are free source.
PDK Initially, the design of an integrated circuit was intimately interwoven with the manufacturing techniques accessible inside each organisation.

Lynn Conway and Carver Mead recognised the need to separate design from technology, and thus the Lambda-based rules were developed.

Since then, we've seen Pure Play Fabs (businesses that just manufacture) and Fabless (companies that only design).

The PDK serves as a bridge between the FAB and the designers.

The PDK contains

Information about device models and technologies
I/O libraries, for example.
Google entered into a deal with Skywater and issued the first open PDK, known as the FOSS 130nm skywater PDK.

The current market share for the 130nm process is 6%, which is still a significant portion of the market. Because of its maturity, the fabrication node for this node is frequently less expensive.

ASIC design entails numerous processes.

The major goal of this flow is to convert the RTL Design to the GDSII format, which is then used by the fabs.

Since then, we've seen Pure Play Fabs (businesses that just manufacture) and Fabless (companies that only design).

The PDK serves as a bridge between the FAB and the designers.

