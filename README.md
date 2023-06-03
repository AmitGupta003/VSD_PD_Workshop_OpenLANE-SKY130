
# VSD_PD_Workshop_OpenLANE-SKY130

This repository contains the workshop's documentation for advanced physical design using OpenLANE/Sky130.

## Contents

#### Day 1: Inception of open-source EDA, OpenLANE and Sky130 PDK
- How to talk to computers 
- SoC design and OpenLANE 
- Get familiar to open-source EDA tools

#### Day 2: Good floorplan vs bad floorplan and introduction to library cells
- Chip Floor planning and considerations 
- Library Binding and Placement 
- Cell design and characterization flows 
- General timing characterization parameters

#### Day 3: Design of a library cell using Magic Layout and ngspice characterization
- Labs for CMOS inverter ngspice simulations 
- Inception of Layout and CMOS fabrication process 
- Sky130 Tech File Labs

#### Day 4: Pre-layout timing analysis and importance of good clock tree
- Timing modelling using delay tables 
- Timing analysis with ideal clocks using openSTA 
- Clock tree synthesis TritonCTS and signal integrity 
- Timing analysis with real clocks using openSTA

#### Day 5: Final steps for RTL2GDS
- Routing and design rule check (DRC) 
- Power distribution network and routing 
- TritonRoute features

## DAY-1

#### How to talk to computers

Introduction to the QFN-48 package, chip, pads, core, die, and IPs

Take, for example, an Arduino board. The entire board can be defined in terms of a block diagram, including processor, SD RAM chip, VCC, and so on blocks associated with it. There will be numerous pins available on the chip. This chip, as it is commonly known, should be referred to as a package with numerous pins. The course's specific package is QFN-48.
![Screenshot 2023-06-03 120520](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/2e1e2b75-8fbc-46b0-89b1-1c6ad1d628c7)


![image](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/832701ff-c622-4646-941c-849055ad36b8)



- The chip is generally placed at the centre of the package and wire bonds are usually used to connect them to the package pins 
- The chip will have various components: 
  - Pads: Through which signals go through 
  - Core: Place at which the digital logic sits 
  - Die: The size of the entire chip


Foundry IPs are PLLs, SRAMs, and so forth.

- A foundry is essentially a large factory with machinery that create silicon chips. 

- As a physical design engineer, you must regularly communicate with the foundry. 

The visible digital blocks are known as Macros.

- There is a significant distinction between macros and IPs; to put it simply, Macros are fundamental digital blocks, whereas IPs require some level of intellect to design and develop. The foundry sends interface files to companies/individuals so that they can communicate with the foundry.

- If an example of RISC V SoC is taken, 
![image](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/4658e9d9-5f60-4dd9-aaa1-6c86b87661f3)


- The PLL, SRAM, etc are called foundry IPs,

- A foundry is essentially a sizable factory with equipment for producing silicon chips. One must regularly communicate with the foundry as a physical design engineer.

- The macros are the visible digital blocks.

- Between macros and IPs, there is a key difference that can be easily understood by saying that IPs take some level of intellect to generate, whereas macros are fundamental digital building blocks. The foundry provides companies and individuals with interface files so that they can communicate with the foundry.

## Introduction to RISC-V

#### Instruction set architecture (ISA) RISC-V 
- If a C programme is to be run on a specific layout, it is first compiled into ALP (depending on the architecture) and then transformed into Machine language (binary). 
- The RISC-V should be implemented using a specific RTL. 
- The entire process begins with RISC-V architecture, then moves on to RTL, and finally to layout.

## From software applications to hardware

- Apps are executed on hardware. 
- The application software enters the System software block, which turns it into binary format that the hardware can interpret. 
- The following are the main components of system software: 
   - The operating system manages IO activities, allocates memory, and so on. 
   - Compiler: The output is determined by the instruction set.
   - Assembler: binary output 

- The instructions of the compiler output will be determined by the chip architecture (RISC-V, x86, etc.): This is an executable file. 
- Depending on the language, the input to the compiler follows a standard format. 
- We require an HDL (RTL) that implements the specified implementations from assembler to hardware.
- This RTL is synthesised into a netlist of gates, which is then translated into layout.

## SoC Design and OpenLANE

#### Introduction to all components of open-source digital ASIC Design
![WhatsApp Image 2023-06-02 at 22 16 56](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/add90990-bfb9-4242-83b4-2565caefd8b3)

![WhatsApp Image 2023-06-02 at 22 18 33](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/06350df8-0297-4875-af79-ec18ff9ac9d6)

- Designing Digital ASICs necessitates a number of factors, including: 
   - EDA Tools
   - RTL IPs
   - PDK(PROCESS DESIGN KITS) data

- There are numerous open source RTL designs available on the internet, including librecores.org, opencores.org, and github.com. 
- Some EDA tools, such as Spice simulator and Magic, are free source. 

- PDK Initially, the design of an integrated circuit was intimately interwoven with the manufacturing techniques accessible inside each organisation.

- Lynn Conway and Carver Mead recognised the need to separate design from technology, and thus the Lambda-based rules were developed.

- Since then, we've seen Pure Play Fabs (businesses that just manufacture) and Fabless (companies that only design).

- The PDK serves as a bridge between the FAB and the designers.

- The PDK contains
   - device models
   - technologies information 
   - I/O libraries etc.. 

- for example. Google entered into a deal with Skywater and issued the first open PDK, known as the FOSS 130nm skywater PDK.

- The current market share for the 130nm process is 6%, which is still a significant portion of the market. Because of its maturity, the fabrication node for this node is frequently less expensive.

- ASIC design entails numerous processes.

- The major goal of this flow is to convert the RTL Design to the GDSII format, which is then used by the fabs.

- Since then, we've seen Pure Play Fabs (businesses that just manufacture) and Fabless (companies that only design).

- The PDK serves as a bridge between the FAB and the designers.

## Simplified RTL to GDSII flow

![WhatsApp Image 2023-06-02 at 22 19 58](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/a2dd8204-757d-4851-a2a3-08acedc09d1e)

- Synthesis
- Power/floor planning
- placement
- clock tree synthesis
- Routing
- Sign off The first major step in a typical ASIC flow is Synthesis
- Synthesis basically converts RTL to a circuit of components from the standard cell library to a gate level netlist
- Generally, the cell layout is enclosed by a fixed height and the width is variable, it is an integer multiple
- Each has different views/models, for example, layout views, SPICE views, electrical views, etc.

### Floor and power planning
Objective here is to plan the silicon area and obtain the most optimum result possible

- In chip floor planning , the chip die is paritioned between different system building blocks and I/O pads
- In Macro floor planning, the macro dimensions and its pin locations are defined. In this step, the rules and routing tracks are defined
- In power planning, the power network is constructed. Typically, a chip is powered by multiple VDD and gnds. Typically, upper metal layers are used for lower resistance
- In placement, the cells are placed on the floor plan. The cells are placed as close as possible to reduce interconnect delay and for routing purpoes that are seen later
- Placement is done in 2 steps usually
  - Global placement: Tries to find the optimal positions for all cells. - May not be legal positions
  - Detailed Placement: The positions are minimally altered to make them legal
- Before routing the signals, we need to route the clock, ensuring that the clock is delivered to all sequential cells with minimum skew
- The clock network usually looks like a tree with various branches (X-tree, H-tree, etc)
- Next, signal routing to implement the nets. The interconnects are made based on the PDK and the available metal layers
- The skywater pdk has 6 layers. 5 layers are aluminium layers
- Routing grid is huge so, a divide and conquer approach is used. again, we have 2 steps
  - Global routing: To generate the routing guides
  - Detailed routing: uses the routing guides to implement the actual wiring
For signoff
- Physical verifications like DRC(Design rule check), LVS( makes sure that the final netlist is matched with the gate level schematic)
- Timing verification through STA(Static timing analysis)

## Introduction to openLane and STRIVE chipsets

![WhatsApp Image 2023-06-02 at 22 23 51](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/31478be8-f716-4b9e-9205-4cf850871bd4)

When using openSource EDA tools, the task becomes a bit complicated

- OpenLANE is open source and free
- Openlane is for an open source flow for a true open source tapeout experience
- STRIVE SoCs are from Efabless
- This family has many memners like
  - Strive, 2, 2a, 3, 5, 6
- The main goal for OpenLANE is to produce a clean GDSII with no human in the loop
- OpenLANE is tuned for the skywater 130nm PDK
- It has 2 modes of operation:
  - Autonomous
  - Interactive
- openLANE has a design space exploaration feature which can be used to find the best set of flow configurations
- It also comes with a large number of design examples

## Introduction to openLANE detailed ASIC Flow design

![image](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/d762be3f-6d3c-46ab-9434-d6aa9d64d999)

- openLANE is based on several opensource projects such as:
  - OpenROAD
  - MAGIC VLSI
  - QFlow
  - ABC
  - KLayout
  - Yosys
  - Fault
- The Flow starts with RTL synthesis, it is fed to yosys with design constraints, it is converted into logic using components, this circuit can be optimized and mapped into library using abc
- ABC needs to be guided during this process
- this guidance comes in the form of ABC strategies: synthesis strategies are used to find the best strategy to continue with
- openLANE has design exploration which can be used to sweep the design exploarations and it generates the number of violations generated, this is useful to find the best configurations
- openLANE regression testing is used to test among the best known designs and to compare among the best results
- After synthesis, we have DFT:
  - Scan insertion
  - Automatic test pattern generation
  - Test patterns compaction
  - Fault coverage
  - Fault simulation

-Next comes the physical implementation which involves several steps, this is done by the openROAD App,Automated power place and route (PnR)
  - Floor/ Power planning
  - End decoupling capacitors and tap cells insertion
  - Placement: Global and detailed
  - Post placement optimization
  - Clock Tree synthesis
  - Routing: Global and detailed

- Logic equivalance checking can be done using yosys
- Every time the netlist is modified, verification needs to be done
- During physical implementation, antenna rules violations should be addressed
- when a wire segment is fabricated, if it is long enough, it can act as antenna, so the length of transistors is reduced
- It has 2 solutions:
  - Bridging: attaches a higher level intermediary ![image](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/f3841bac-8761-4797-9050-f929d6c1f6f1)



  - The other solution is to create another antenna diode ![image](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/0e4d1668-925f-44dd-aa64-405801e80c02)


With OpenLANE, a preventive approach has been taken

- a fake antenna diode is created next to every cell input after placement
- This cell is not a real diode but matches the footprint of the library being used
- If the checker reports a violation on cell input pin, replace the fake diode by a real one

![image](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/73dd2c6c-00e5-4c1a-957e-2b165161768b)

One of these 2 approaches can be used in openLANE

signoff in openLANE STA is done by openSTA

- physical signoff includes DRC and LVS
- Magic is used for DRC and spice extraction from layout
- Magic and Netgen are used for LVS
- extracted spice by magic vs verilog netlist
