
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



### 1.Design preparation steps
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

Before running synthesis, we need to prepare the datastructure and the file system Upon running the command

### 2. Design Setup Stage:
```bash
prep -design picorv32a
```
The 2 LEFS are merged, techlef and lef files.

![WhatsApp Image 2023-06-03 at 13 23 25](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/3b1827c7-903e-40f2-b37d-bb707a913478)

A runs directory is created beneath the picorv32a folder, which contains all of the required files.

The LEF file contained within this directory will have all of the cell, layer, and other information, whereas the results folder will contain separate folders for each phase. The config.tcl file basically shows all of the default parameters used by the run cmds.log file records all of the commands.
```bash
run_synthesis
```

### 3. Run synthesis:

![WhatsApp Image 2023-06-03 at 13 23 25](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/0f33067a-b0c6-4d1a-a62f-9addf01afc95)

After running synthesis, inside the runs/05-06_08-09/results/synthesis is picorv32a_synthesis.v which is the mapping of the netlist to standard cell library using ABC. The runs/05-06_08-09/reports/synthesis will contain synthesis statistic reports and static timing analysis reports. The runs/05-06_08-09/synthesis/logs contains log files for the terminal output dumps for running yosys and OpenSTA.

After the results, we need to calculate the flop ratio.

To find the FLOP RATIO i.e, number of D-Flip Flops:

flop ratio = (no of flops)/(total no of cells)
Number of cells: 14876

Number of D FF: 1613

Flop count (Number of D-FF/ Number of cells) : 0.1084

Flop count percentage: 10.84

```bash
Printing statistics.

=== picorv32a ===

   Number of wires:              14596
   Number of wire bits:          14978
   Number of public wires:        1565
   Number of public wire bits:    1947
   Number of memories:               0
   Number of memory bits:            0
   Number of processes:              0
   Number of cells:              14876
     sky130_fd_sc_hd__a2111o_2       1
     sky130_fd_sc_hd__a211o_2       35
     sky130_fd_sc_hd__a211oi_2      60
     sky130_fd_sc_hd__a21bo_2      149
     sky130_fd_sc_hd__a21boi_2       8
     sky130_fd_sc_hd__a21o_2        57
     sky130_fd_sc_hd__a21oi_2      244
     sky130_fd_sc_hd__a221o_2       86
     sky130_fd_sc_hd__a22o_2      1013
     sky130_fd_sc_hd__a2bb2o_2    1748
     sky130_fd_sc_hd__a2bb2oi_2     81
     sky130_fd_sc_hd__a311o_2        2
     sky130_fd_sc_hd__a31o_2        49
     sky130_fd_sc_hd__a31oi_2        7
     sky130_fd_sc_hd__a32o_2        46
     sky130_fd_sc_hd__a41o_2         1
     sky130_fd_sc_hd__and2_2       157
     sky130_fd_sc_hd__and3_2        58
     sky130_fd_sc_hd__and4_2       345
     sky130_fd_sc_hd__and4b_2        1
     sky130_fd_sc_hd__buf_1       1656
     sky130_fd_sc_hd__buf_2          8
     sky130_fd_sc_hd__conb_1        42
     sky130_fd_sc_hd__dfxtp_2     1613
     sky130_fd_sc_hd__inv_2       1615
     sky130_fd_sc_hd__mux2_1      1224
     sky130_fd_sc_hd__mux2_2         2
     sky130_fd_sc_hd__mux4_1       221
     sky130_fd_sc_hd__nand2_2       78
     sky130_fd_sc_hd__nor2_2       524
     sky130_fd_sc_hd__nor2b_2        1
     sky130_fd_sc_hd__nor3_2        42
     sky130_fd_sc_hd__nor4_2         1
     sky130_fd_sc_hd__o2111a_2       2
     sky130_fd_sc_hd__o211a_2       69
     sky130_fd_sc_hd__o211ai_2       6
     sky130_fd_sc_hd__o21a_2        54
     sky130_fd_sc_hd__o21ai_2      141
     sky130_fd_sc_hd__o21ba_2      209
     sky130_fd_sc_hd__o21bai_2       1
     sky130_fd_sc_hd__o221a_2      204
     sky130_fd_sc_hd__o221ai_2       7
     sky130_fd_sc_hd__o22a_2      1312
     sky130_fd_sc_hd__o22ai_2       59
     sky130_fd_sc_hd__o2bb2a_2     119
     sky130_fd_sc_hd__o2bb2ai_2     92
     sky130_fd_sc_hd__o311a_2        8
     sky130_fd_sc_hd__o31a_2        19
     sky130_fd_sc_hd__o31ai_2        1
     sky130_fd_sc_hd__o32a_2       109
     sky130_fd_sc_hd__o41a_2         2
     sky130_fd_sc_hd__or2_2       1088
     sky130_fd_sc_hd__or2b_2        25
     sky130_fd_sc_hd__or3_2         68
     sky130_fd_sc_hd__or3b_2         5
     sky130_fd_sc_hd__or4_2         93
     sky130_fd_sc_hd__or4b_2         6
     sky130_fd_sc_hd__or4bb_2        2

   Chip area for module '\picorv32a': 147712.918400
   ```
  ![WhatsApp Image 2023-06-03 at 15 11 38](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/14693a08-ce20-48a9-b82a-046e92cac99c)

![WhatsApp Image 2023-06-03 at 15 11 39](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/8bfef2f9-0273-4f3a-b8cf-c5b4a30eb1b1)

![WhatsApp Image 2023-06-03 at 15 22 10](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/228bf6e4-e1e3-415b-a92d-6f6c6f12e69b)

![WhatsApp Image 2023-06-03 at 15 22 10](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/d18f4da2-7be6-4cd8-9327-f0b538456d54)


### Yosys synthesis:

#### run_floorplan

![WhatsApp Image 2023-06-03 at 15 44 40](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/f55376b0-0e5d-434a-bc4c-11b13fa4a709)

![WhatsApp Image 2023-06-03 at 16 22 58](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/11f50691-ceae-4144-8927-4203b80e05a2)

![WhatsApp Image 2023-06-03 at 17 06 17](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/d291add5-48bd-441b-8ae9-9e26cb77caea)

## Floorplan Stage:

```bash
% run floor_plan
```

##### 1.Determine the height and width of the core and die.
The logic blocks are positioned in the core, which is located in the centre of the die. The width and height are determined by the dimensions of the standard cells on the netlist. The utilisation factor is (netlist area occupied)/(total core area). In practise, the utilisation factor is between 0.5 and 0.6. This is the only space utilised by the netlist; the remaining space is for routing and extra cells. Aspect ratio is defined as (height)/(width) of the core, hence only aspect ratio 1 produces a square core shape.

##### 2.Define the position of the Preplaced Cell.
These are pre-implemented reusable complicated logicblocks, modules, IPs, or macros (memory, clock-gating cell, mux, comparator, etc.). The core placement is user-defined and must be completed before placement and routing (thus preplaced cells). Because the automatic place and route tools will be unable to touch or move these preplaced cells, this must be clearly indicated.

##### 3.Surround preplaced cells with decoupling capacitors. 
The complex preplaced logicblock requires a high amount of current from the powersource for current switching. But since there is a distance between the main powersource and the logicblock, there will be voltage drop due to the resistance and inductance of the wire. This might cause the voltage at the logicblock to be not within the noise margin range anymore (logic is unstable). The solution is to use decoupling capacitors near the logic block, this capacitor will send enough current needed by the logicblock to switch within the noise margin range.

##### 4.Power Scheduling.
It is not possible to apply a decoupling capactor to all of the logic blocks on the chip, but just to the key pieces (preplaced complicated logicblocks). Switching a large number of elements to logic 0 may produce ground bounce due to the enormous amount of current that must be sinked at the same time, and switching to logic 1 may cause voltage droop due to insufficient current from the powersource to supply the requisite current of all elements. Because of ground bounce and voltage droop, the voltage may not be within the noise margin range. The answer is to have numerous powersource taps (power mesh) from which elements can source current from the nearest VDD tap and sink current to the nearest VSS tap.

##### 5.Pin Placement
The input and output ports are situated between the core and the die. The location of the ports is determined by the location of the cells connected to those ports on the core. Because this clock must be capable of driving the entire chip, the clock port is thicker (has the lowest resistance route) than the data ports.

##### 6.Logical Cell Placement Blockage
Blockage of Logical Cell Placement This ensures that the automated placement and routing tool does not place any cells on the die's pin positions.

Priority order of configuration files to be used by the Openlane flow:

sky130A_sky130_fd_sc_hd_config.tcl

conifg.tcl

floorplan.tcl - System default variables.

The variables we are interested in as of now:

Floorplan environment variables or switches:

FP_CORE_UTIL - floorplan core utilization
FP_ASPECT_RATIO - floorplan aspect ratio
FP_CORE_MARGIN - Core to die margin area
FP_IO_MODE - defines pin configurations (1 = equidistant/0 = not equidistant)
FP_CORE_VMETAL - vertical metal layer
FP_CORE_HMETAL - horizontal metal layer

- Constrains added in the config.tcl for the simulation.
set ::env(FP_CORE_UTIL) "80"
```bash set ::env(PL_TARGET_DENSITY) "0.2"
set ::env(PL_BASIC_PLACEMENT) "0"
set ::env(GLB_RESIZER_TIMING_OPTIMIZATIONS) "0"
set ::env(FP_SIZING) "absolute"
set ::env(DIE_AREA) "0.0 0.0 1000.000 1000.000"
set ::env(CORE_AREA) "5.25 10.88 900.00 900.00"

set ::env(DIODE_INSERTION_STRATEGY) "4"
set ::env(GLB_RT_MAXLAYER) "5"

set ::env(VDD_NETS) [list {vccd1}]

set ::env(GND_NETS) [list {vssd1}]
```

### Review floor plan layout in magic

![WhatsApp Image 2023-06-03 at 17 33 38](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/0b730ddc-0937-4c5e-a251-b622480d92e4)

Move onto an object and press s, open the tkcon window and type what, it will show the layer

![WhatsApp Image 2023-06-03 at 17 34 17](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/197d0ccf-90ce-49f8-9302-7c6826c21f65)


### Run floorplan on OpenLane:

The command to run floorplan is

```run_floorplan```

- To view floor plan layout in magic
`magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def`

![WhatsApp Image 2023-06-03 at 17 44 12](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/91e33706-0e82-4c39-9b31-0ad876c2e92b)

The following are the design's utilisation factor and aspect ratio:

 `(Area occupied by netlist)/(total Area of the core) = Utilisation Factor`

`(height/width) equals Aspect Ratio`

A Utilisation Factor of 1 denotes complete utilisation, with no room for routing or further logic. However, in practise, the Utilisation Factor will typically be 0.5-0.6, indicating that 50 to 60% of the area is used for macros, standard cells, and the remainder is used for routing and extralogic. Similarly, an Aspect ratio of 1 indicates that the chip is square in shape. Rectangular chip is represented by any value other than 1.

### Placement Stage:
1. Bind the netlist to a real-world physical cell. The physical cell will be drawn from a library with many alternatives for shape, dimension, and delay for the same cell.

2.Next is placement of those physical cells to the floorplan.

3.Maintain signal integrity by optimising location. This is where we estimate wirelength and capacitance (C=EA/d) and put repeaters/buffers based on that. The wirelength will generate a resistance, causing an unwanted voltage drop, and a capacitance, causing a slew rate that may be insufficient for fast current switching of logic gates. To reduce resistance and capacitance, include buffers for lengthy lines that act as intermediaries, separating a single long wire into multiple ones. If it has to run at a high frequency (2GHz), we sometimes perform arrangements where logic cells are put very close to one other (virtually zero latency). Because we can employ independent metal layers, crisscrossing of paths is a normal circumstance for PnR.

4. After placement optimization, We will setup timing analysis using idle clock (zero delay for wires and has no clock buffer related delays) considering we have not yet done CTS.

The purpose of placement is still congestion rather than timing. Furthermore, typical cells are not placed on the layout stage, but rather on the Placement stage. Macros, also known as preplaced cells, are the cells that are placed on the floorplan stage.On the floorplan stage, macros or preplaced cells are placed.

`%run_placement` is the placement. This command is a wrapper that does global placement (through the RePlace tool), optimisation (by the Resier tool), and detailed placement (via the OpenDP tool). It shows hundreds of iterations of HPWL and OVFL. If the overflow decreases, the algorithm is considered to be converging. It also verifies legality.

`magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &`

![WhatsApp Image 2023-06-03 at 18 01 19](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/25e0a32e-bdf2-4fd1-866a-9fcd7ff2845f)

![WhatsApp Image 2023-06-03 at 22 54 47](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/1ba7f927-f823-4b4f-bf1d-dc8b3ed88f95)

`Placement Analysis
--------------------------------
total displacement        0.0 u
average displacement      0.0 u
max displacement          0.0 u
original HPWL        766080.0 u
legalized HPWL       779196.5 u
delta HPWL                  2 %
`

Placement is done on two stages:

Global Placement = placement with no legalizations and goal is to reduce wirelength. It uses Half Perimeter Wirelength (HPWL) reduction model.
Detailed Placement = placement with legalization where the standard cells are placed on stadard rows, abutted, and must have no overlaps

![WhatsApp Image 2023-06-03 at 23 07 25](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/4a14a00f-a170-44a8-ac87-e3dd31ad63e5)

![WhatsApp Image 2023-06-03 at 23 09 00](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/6a069b12-5298-4534-8fe3-c2145f4af098)

![WhatsApp Image 2023-06-03 at 23 10 48](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/f93dd124-4e06-4ea0-9fb8-ac3b1ebaf242)

### CTS Command Script:
After extracting the modified verilog netlist after doing timing ECO, run_floorplan and run_placement and then run_cts. In CTS, the verilog netlist is modified to add the clock buffers and this new verilog netlist is saved under /runs/[date]/results/cts/.

run_cts and the other OpenLane commands are actually just calling the tcl proc (procedure) inside /OpenLane/scripts/tcl_commands/. This tcl procedure will then call OpenROAD to run the actual tool. For example, run_cts can be found inside /OpenLane/scripts/tcl_commands/cts.tcl, this tcl procedure will call OpenROAD and will call /OpenLane/scripts/openroad/cts.tcl which contains the OpenROAD commands to run TritonCTS.

Inside the /OpenLane/scripts/openroad/cts.tcl contains the configuration variables for CTS. Notables ones are:

CTS_CLK_BUFFER_LIST = list of clock branch buffers (sky130_fd_sc_hd__clkbuf_8 sky130_fd_sc_hd__clkbuf_4 sky130_fd_sc_hd__clkbuf_2)
CTS_ROOT_BUFFER = clock buffer used for the root of the clock tree and is the biggest clock buffer to drive the clock tree of the whole chip (sky130_fd_sc_hd__clkbuf_16)
CTS_MAX_CAP = maximum capacitance of the output port of the root clock buffer.
To clone the necessary mag files and spice models for the inverter, PMOS, and NMOS in the Sky130 process, follow these steps:

Open your terminal or command prompt.

Run the following command to clone the files from the GitHub repository:

git clone https://github.com/nickson-jose/vsdstdcelldesign.git

The layout details of an inverter can be found in the sky130_inv.mag file.
The file contains information about the physical arrangement of the inverter components on the chip.
The inverter layout is important for understanding how the inverter is designed and how its components are connected.
To access the file and view the layout information, navigate to the sky130_inv.mag file in the repository.
The layout information provided in the file helps in understanding the spatial organization of the inverter's transistors, metal interconnects, and other elements.
By examining the inverter layout, one can gain insights into the physical implementation of the circuit and its interconnections.
Understanding the inverter layout is crucial for analyzing its performance, identifying potential issues, and making improvements if necessary.
To make use of the information in the sky130_inv.mag file, open it in a layout viewing tool compatible with the file format.
The file provides a visual representation of the inverter layout, enabling designers to examine the physical structure and connectivity of the components.
By studying the inverter layout, designers can gain a deeper understanding of its physical characteristics, aiding in the overall design process.
For layout we run magic command

magic -T sky130A.tech sky130_inv.mag &

![WhatsApp Image 2023-06-03 at 23 12 16](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/5e2a0fa6-6019-4c0a-aa36-05e324fdc02b)

![WhatsApp Image 2023-06-03 at 23 15 29](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/480d2449-c3ae-4970-a156-ce8007509c55)

![WhatsApp Image 2023-06-03 at 23 17 23](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/095fbcbf-7e26-43ac-bb43-c9471a237478)

![WhatsApp Image 2023-06-03 at 23 29 41](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/39c92d76-8589-4d36-bea0-9a9e5ff450ac)

![WhatsApp Image 2023-06-04 at 01 44 54](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/48c69921-267d-4e99-be6a-f73b69f452d2)

![WhatsApp Image 2023-06-04 at 01 45 17](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/6166200b-a5ca-4478-b3d1-c5c0a166e860)

![WhatsApp Image 2023-06-04 at 01 48 45](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/4bdd3f8e-1368-4e4e-9575-6e98384d166a)

2.Modify the spice file to be able to plot a transient response:
* SPICE3 file created from sky130_inv.ext - technology: sky130A

.option scale=0.01u
.include ./libs/pshort.lib
.include ./libs/nshort.lib

* .subckt sky130_inv A Y VPWR VGND
M0 Y A VGND VGND nshort_model.0 ad=1435 pd=152 as=1365 ps=148 w=35 l=23
M1 Y A VPWR VPWR pshort_model.0 ad=1443 pd=152 as=1517 ps=156 w=37 l=23
C0 A VPWR 0.08fF
C1 Y VPWR 0.08fF
C2 A Y 0.02fF
C3 Y VGND 0.18fF
C4 VPWR VGND 0.74fF
* .ends

* Power supply 
VDD VPWR 0 3.3V 
VSS VGND 0 0V 

* Input Signal
Va A VGND PULSE(0V 3.3V 0 0.1ns 0.1ns 2ns 4ns)

* Simulation Control
.tran 1n 20n
.control
run
.endc
.end

3.Open the spice file by typing ngspice sky130A_inv.spice. And Generate a graph using plot y vs time a :
Circuit:** spice3 file created from sky130_inv.ext - technology: 

Scale set
Doing analysis at TEMP = 27.000000 and TNOM = 27.000000

Warning: va: no DC value, transient time 0 value used

Initial Transient Solution
--------------------------
Node                                                Voltage
----                                                -------
y                                                       3.3
a                                                         0 
vgnd                                                      0
vpwr                                                      0
va#branch                                               3.3
vss#branch                                      3.33336e-12
vdd#branch                                     -3.33339e-12

Reference value: 0.00000e+00

No. of Data Rows: 160
ngspice 1 -> plot y vs time a 
ngspice 1 ->

![WhatsApp Image 2023-06-04 at 01 48 55](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/dd48c263-1b47-42d2-bd49-2cc675820d12)

![WhatsApp Image 2023-06-04 at 02 16 50](https://github.com/AmitGupta003/VSD_PD_Workshop_OpenLANE-SKY130/assets/135353855/ef96acde-aacf-41b8-9e30-850a3977941d)
