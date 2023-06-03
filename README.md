
# VSD_PD_Workshop_OpenLANE-SKY130
This repository contains the workshop's documentation for advanced physical design using OpenLANE/Sky130.

### Contents
#### Day 1: Inception of open-source EDA, OpenLANE and Sky130 PDK
•How to talk to computers •SoC design and OpenLANE •Get familiar to open-source EDA tools

#### Day 2: Good floorplan vs bad floorplan and introduction to library cells
•Chip Floor planning and considerations •Library Binding and Placement •Cell design and characterization flows •General timing characterization parameters

#### Day 3: Design of a library cell using Magic Layout and ngspice characterization
•Labs for CMOS inverter ngspice simulations •Inception of Layout and CMOS fabrication process •Sky130 Tech File Labs

#### Day 4: Pre-layout timing analysis and importance of good clock tree
•Timing modelling using delay tables •Timing analysis with ideal clocks using openSTA •Clock tree synthesis TritonCTS and signal integrity •Timing analysis with real clocks using openSTA

#### Day 5: Final steps for RTL2GDS
•Routing and design rule check (DRC) •Power distribution network and routing •TritonRoute features

## Day-1
#### How to talk to computers
Introduction to the QFN-48 package, chip, pads, core, die, and IPs

Take, for example, an Arduino board. The entire board can be defined in terms of a block diagram, including processor, SD RAM chip, VCC, and so on blocks associated with it. There will be numerous pins available on the chip. This chip, as it is commonly known, should be referred to as a package with numerous pins. The course's specific package is QFN-48. Screenshot 2023-06-02 215346

![Screenshot 2023-06-03 120520](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/faa965d4-3187-4982-aa50-f0ef3bec6c2b)

![image](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/28db552c-fe6d-4369-8944-858f81767997)

The chip is generally placed at the centre of the package and wire bonds are usually used to connect them to the package pins •The chip will have various components: • Pads: Through which signals go through • Core: Place at which the digital logic sits • Die: The size of the entire chip

Foundry IPs are PLLs, SRAMs, and so forth.

A foundry is essentially a large factory with machinery that create silicon chips. As a physical design engineer, you must regularly communicate with the foundry. The visible digital blocks are known as Macros.

There is a significant distinction between macros and IPs; to put it simply, Macros are fundamental digital blocks, whereas IPs require some level of intellect to design and develop. The foundry sends interface files to companies/individuals so that they can communicate with the foundry.

If an example of RISC V SoC is taken, the SoC will look like Foundry IPs are the PLL, SRAM, and other components.

A foundry is essentially a sizable factory with equipment for producing silicon chips. One must regularly communicate with the foundry as a physical design engineer.

The macros are the visible digital blocks.

Between macros and IPs, there is a key difference that can be easily understood by saying that IPs take some level of intellect to generate, whereas macros are fundamental digital building blocks. The foundry provides companies and individuals with interface files so that they can communicate with the foundry.

![image](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/5c8d2a26-7624-4457-b063-4a8d118b379a)

### Introduction to RISC-V
Instruction set architecture (ISA) RISC-V If a C programme is to be run on a specific layout, it is first compiled into ALP (depending on the architecture) and then transformed into Machine language (binary). The RISC-V should be implemented using a specific RTL. The entire process begins with RISC-V architecture, then moves on to RTL, and finally to layout.

### From software applications to hardware
Apps are executed on hardware. The application software enters the System software block, which turns it into binary format that the hardware can interpret. The following are the main components of system software: The operating system manages IO activities, allocates memory, and so on. Compiler: The output is determined by the instruction set. Assembler: binary output The instructions of the compiler output will be determined by the chip architecture (RISC-V, x86, etc.): This is an executable file. Depending on the language, the input to the compiler follows a standard format. We require an HDL (RTL) that implements the specified implementations from assembler to hardware. This RTL is synthesised into a netlist of gates, which is then translated into layout.

### SoC Design and OpenLANE
Introduction to all components of open-source digital ASIC Design

![Screenshot 2023-06-03 123056](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/e7a61480-a9e0-47c0-95d6-a16d50b7dd2c)


![Screenshot 2023-06-03 123410](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/1edf1d49-a14b-41cf-ad8d-121c31d565ce)

Designing Digital ASICs necessitates a number of factors, including: EDA Tools for RTL IPs

Data from PDKs (process design kits) There are numerous open source RTL designs available on the internet, including librecores.org, opencores.org, and github.com. Some EDA tools, such as Spice simulator and Magic, are free source. PDK Initially, the design of an integrated circuit was intimately interwoven with the manufacturing techniques accessible inside each organisation.

Lynn Conway and Carver Mead recognised the need to separate design from technology, and thus the Lambda-based rules were developed.

Since then, we've seen Pure Play Fabs (businesses that just manufacture) and Fabless (companies that only design).

The PDK serves as a bridge between the FAB and the designers.

The PDK contains

Information about device models and technologies I/O libraries, for example. Google entered into a deal with Skywater and issued the first open PDK, known as the FOSS 130nm skywater PDK.

The current market share for the 130nm process is 6%, which is still a significant portion of the market. Because of its maturity, the fabrication node for this node is frequently less expensive.

ASIC design entails numerous processes.

The major goal of this flow is to convert the RTL Design to the GDSII format, which is then used by the fabs.

Since then, we've seen Pure Play Fabs (businesses that just manufacture) and Fabless (companies that only design).

The PDK serves as a bridge between the FAB and the designers.

### Simplified RTL to GDSII flow
![image](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/ffe5b3cf-7120-43e1-a14a-a3142266a73e)


Placement of Synthesis Power/Floor Planning The synthesis of a clock tree Routing Please sign off. Synthesis is the first key stage in a typical ASIC flow. Synthesis is the process of converting RTL to a circuit of components from a standard cell library and then to a gate level netlist. In general, the cell layout is surrounded by a fixed height and a variable width that is an integer multiple. Each has its own set of views/models, such as layout views, SPICE views, electrical views, and so forth.

### Floor and power planning
The goal here is to plan the silicon area and achieve the best feasible result.

The semiconductor die is partitioned between several system building blocks and I/O pads during chip floor layout. The macro dimensions and pin positions are defined in Macro floor planning. The rules and routing tracks are defined in this step.

The power network is built during power planning. A chip is often powered by numerous VDDs and gnds. Upper metal layers are typically employed for decreased resistance. The cells are placed on the floor plan during placement. The cells are put as near together as feasible to reduce interconnect time and for future routing purposes. Typically, placement is accomplished in two processes. Global positioning: Attempts to locate

Before we route the signals, we must route the clock, ensuring that the clock is transmitted to all successive cells with the least amount of skew.

The clock network is typically depicted as a tree with multiple branches (X-tree, H-tree, etc.). Following that, signal routing is used to put the networks together. The interconnects are created using the PDK and the metal layers that are accessible.

Skywater pdk contains six layers. 5 layers are made of aluminium. Because the routing grid is so large, a divide and conquer strategy is adopted. We have two steps once more. Routing at the global level: To produce the route guides Routeing details: The actual wiring is implemented using the routing guides. As a final note, DRC (Design rule check), LVS (makes sure the final netlist matches the gate level schematic) are examples of physical verifications. Timing verification through STA(Static timing analysis)

### Introduction to openLane and STRIVE chipsets
![Screenshot 2023-06-03 122722](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/a03704a6-8b38-4f76-8ae2-43bebad895ab)

The work becomes more difficult when openSource EDA tools are used.

OpenLANE is a free and open source networking protocol. Openlane is a truly open source tapeout experience with an open source flow. STRIVE Efabless provides SoCs. Strive, 2, 2a, 3, 5, 6 are all members of this family. The primary purpose of OpenLANE is to generate a clean GDSII with no human intervention. The skywater 130nm PDK is optimised for OpenLANE. It operates in two modes: Autonomous The design space exploration function of Interactive openLANE can be used to determine the optimal collection of flow configurations. It also includes a vast number of design examples.

### Introduction to openLANE detailed ASIC Flow design
![image](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/47fd4fd2-d207-4fe8-b585-68f431933daf)

openLANE is based on several opensource projects such as: OpenROAD MAGIC VLSI QFlow ABC KLayout Yosys Fault The Flow starts with RTL synthesis, it is fed to yosys with design constraints, it is converted into logic using components, this circuit can be optimized and mapped into library using abc ABC needs to be guided during this process this guidance comes in the form of ABC strategies: synthesis strategies are used to find the best strategy to continue with openLANE has design exploration which can be used to sweep the design exploarations and it generates the number of violations generated, this is useful to find the best configurations openLANE regression testing is used to test among the best known designs and to compare among the best results After synthesis, we have DFT: Scan insertion Automatic test pattern generation Test patterns compaction Fault coverage Fault simulation Next comes the physical implementation which involves several steps, this is done by the openROAD App,Automated power place and route (PnR) Floor/ Power planning End decoupling capacitors and tap cells insertion Placement: Global and detailed Post placement optimization Clock Tree synthesis Routing: Global and detailed Logic equivalance checking can be done using yosys Every time the netlist is modified, verification needs to be done During physical implementation, antenna rules violations should be addressed when a wire segment is fabricated, if it is long enough, it can act as antenna, so the length of transistors is reduced It has 2 solutions: Bridging: attaches a higher level intermediary ![image](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/00803be7-ff58-4009-a828-d4716e957474)


The other solution is to create another antenna diode With OpenLANE ![image](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/bf9d902f-e21d-4b6f-a63e-f0a90d49d421)


a fake antenna diode is created next to every cell input after placement This cell is not a real diode but matches the footprint of the library being used If the checker reports a violation on cell input pin, replace the fake diode by a real one image

One of these 2 approaches can be used in openLANE

Signoff in openLANE STA is done by openSTA

physical signoff includes DRC and LVS Magic is used for DRC and spice extraction from layout Magic and Netgen are used for LVS extracted spice by magic vs verilog netlist


## Getting started with Labs

#### OpenLANE directory structure

To deploy this project run

```bash
~/Desktop/work/tools/openlane_working_dir/openlane
```

The contents are:

![WhatsApp Image 2023-06-03 at 12 54 16](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/d8d02f1b-1eab-4b1e-9bcc-327f9c1476a3)

Under sky130A, libs.tech contains files specific to the tool Under libs.ref and libs.tech, we have

sky130_fd_sc_hd denotes

fd: foundry name, for example, OSU is for Oklahoma State University
sc: normal cell
hd: A PDK variation, hd stands for high density. We may see several files connected with that folder inside the directory.

![WhatsApp Image 2023-06-03 at 13 02 33](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/a7631c22-306e-4bec-9f39-c45ea9cf3b8a)



Design preparation steps
```bash
docker
./flow.tcl -interactive
```
runs the script in interactive mode; without the interactive command, it will run from RTL to GDSII entirely. Running the docker command and executing the tcl script as described opens the container.

![WhatsApp Image 2023-06-03 at 13 10 00](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/15cb4733-62d8-4e7b-9ec7-4488e0d7f76b)

The designs folder contains all of the designs that are being run by openlane. First, we will work on picorv32a. It will include three files,

src file: RTL will be present, and sdc file config.tcl - bypasses any configurations already done in openLANE, i.e. many switches already have a default value. The default values are depicted in the graphic below.

```bash
less config.tcl
```

results in the below image

![WhatsApp Image 2023-06-03 at 13 18 35](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/bd11d407-2d19-4485-bbbe-6e76ce05a296)


config.tcl will have the highest priority

Before running synthesis, we need to prepare the datastructure and the file system Upon running the command,

```bash
prep -design picorv32a
```
The 2 LEFS are merged, techlef and lef files.

![WhatsApp Image 2023-06-03 at 13 18 35](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/5dbc4f8f-4893-487f-a193-af240fbcd21a)
