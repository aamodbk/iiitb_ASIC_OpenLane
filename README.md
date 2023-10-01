# Physical Design using OpenLane Flow
Refer to the repository https://github.com/aamodbk/iiitb_asic_course for installation of OpenLane tools for the required resources.

## Day 1
### Introduction to OpenLane
Open-source chip development has become more accessible with the availability of RTL designs and EDA tools at no cost. The SKY130 PDK, a collaboration between Skywater Technologies and Google, plays a crucial role in this by offering an open-source platform. Initially, the design flow was unclear, and the SKY130 PDK was only compatible with industrial equipment. To address these issues, the OpenLane project was introduced. OpenLane is an automated RTL to GDSII flow that streamlines the chip design process. It's not a product but a collection of EDA tools, scripts, and optimized Skywater PDKs designed for use with open-source EDA tools.

### Overall Design Flow
The RTL to GDSII flow encompasses several essential stages in digital chip design. 
* It commences with RTL design, capturing the circuit's functional behavior.
* RTL synthesis transforms this into a gate-level netlist, optimizing for area, power, and timing.
* Floor and power planning partition the chip's area, establish layout dimensions, and determine power distribution.
* Placement assigns physical coordinates to cells, aiming to minimize wirelength and optimize signal delay.
* Clock tree synthesis constructs an efficient clock distribution network, and routing connects components while adhering to design rules.
* Sign-off includes comprehensive verification.
* And finally, GDSII file generation produces the format for fabrication, containing geometric and mask details.

### OpenLane Flow
OpenLane is an automated RTL to GDSII flow based on several components including OpenROAD, Yosys, Magic, Netgen, CVC, SPEF-Extractor, KLayout and a number of custom scripts for design exploration and optimization. It also provides a number of custom scripts for design exploration and optimization. The flow performs all ASIC implementation steps from RTL all the way down to GDSII.
OpenLane integrated several key open source tools over the execution stages:
* RTL Synthesis, Technology Mapping, and Formal Verification : yosys + abc
* Static Timing Analysis: OpenSTA
* Floor Planning: init_fp, ioPlacer, pdn and tapcell
* Placement: RePLace (Global), Resizer and OpenPhySyn (formerly), and OpenDP (Detailed)
* Clock Tree Synthesis: TritonCTS
* Fill Insertion: OpenDP/filler_placement
* Routing: FastRoute or CU-GR (formerly) and TritonRoute (Detailed) or DR-CU
* SPEF Extraction: OpenRCX or SPEF-Extractor (formerly)
* GDSII Streaming out: Magic and KLayout
* DRC Checks: Magic and KLayout
* LVS check: Netgen
* Antenna Checks: Magic
* Circuit Validity Checker: CVC

### Synthesis in OpenLane
Follow the following steps while in directory with OpenLane installed to run synthesis on the PicoRV32 RISC-V processor core verilog code:
```
cd ./OpenLane
make mount
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
run_synthesis
```

![alt text](https://github.com/aamodbk/iiitb_ASIC_OpenLane/blob/main/openlane_synth.png)

To view the synthesized netlist:
```
cd ./OpenLane/designs/picorv32a/runs/RUN_2023.10.01_10.11.00/results/synthesis/
vim picorv32a.v
```

![alt text](https://github.com/aamodbk/iiitb_ASIC_OpenLane/blob/main/synth_v.png)

To view report:
```
vim 1-synthesis.AREA_0.stat.rpt
```

```
24. Printing statistics.

=== picorv32 ===

   Number of wires:               9190
   Number of wire bits:           9572
   Number of public wires:        1512
   Number of public wire bits:    1894
   Number of memories:               0
   Number of memory bits:            0
   Number of processes:              0
   Number of cells:               9470
     sky130_fd_sc_hd__a2111o_2       1
     sky130_fd_sc_hd__a2111oi_2      1
     sky130_fd_sc_hd__a211o_2       79
     sky130_fd_sc_hd__a211oi_2      14
     sky130_fd_sc_hd__a21bo_2       11
     sky130_fd_sc_hd__a21boi_2       7
     sky130_fd_sc_hd__a21o_2       176
     sky130_fd_sc_hd__a21oi_2       92
     sky130_fd_sc_hd__a221o_2      136
     sky130_fd_sc_hd__a221oi_2       1
     sky130_fd_sc_hd__a22o_2       129
     sky130_fd_sc_hd__a22oi_2        2
     sky130_fd_sc_hd__a2bb2o_2      15
     sky130_fd_sc_hd__a311o_2       28
     sky130_fd_sc_hd__a311oi_2       1
     sky130_fd_sc_hd__a31o_2        60
     sky130_fd_sc_hd__a31oi_2        2
     sky130_fd_sc_hd__a32o_2       111
     sky130_fd_sc_hd__a41o_2         2
     sky130_fd_sc_hd__and2_2       202
     sky130_fd_sc_hd__and2b_2       46
     sky130_fd_sc_hd__and3_2       113
     sky130_fd_sc_hd__and3b_2       32
     sky130_fd_sc_hd__and4_2        39
     sky130_fd_sc_hd__and4b_2        5
     sky130_fd_sc_hd__buf_1       2599
     sky130_fd_sc_hd__buf_2          8
     sky130_fd_sc_hd__conb_1       106
     sky130_fd_sc_hd__dfxtp_2     1596
     sky130_fd_sc_hd__inv_2         63
     sky130_fd_sc_hd__mux2_1         1
     sky130_fd_sc_hd__mux2_2      1592
     sky130_fd_sc_hd__mux4_2       478
     sky130_fd_sc_hd__nand2_2      225
     sky130_fd_sc_hd__nand2b_2       2
     sky130_fd_sc_hd__nand3_2        9
     sky130_fd_sc_hd__nand3b_2       5
     sky130_fd_sc_hd__nand4_2        3
     sky130_fd_sc_hd__nor2_2       289
     sky130_fd_sc_hd__nor2b_2        1
     sky130_fd_sc_hd__nor3_2        12
     sky130_fd_sc_hd__nor3b_2        5
     sky130_fd_sc_hd__nor4_2         4
     sky130_fd_sc_hd__nor4b_2        2
     sky130_fd_sc_hd__o2111a_2       5
     sky130_fd_sc_hd__o211a_2       84
     sky130_fd_sc_hd__o211ai_2      10
     sky130_fd_sc_hd__o21a_2       192
     sky130_fd_sc_hd__o21ai_2      181
     sky130_fd_sc_hd__o21ba_2       21
     sky130_fd_sc_hd__o21bai_2       4
     sky130_fd_sc_hd__o221a_2       44
     sky130_fd_sc_hd__o221ai_2      28
     sky130_fd_sc_hd__o22a_2        42
     sky130_fd_sc_hd__o22ai_2        1
     sky130_fd_sc_hd__o2bb2a_2       4
     sky130_fd_sc_hd__o311a_2        5
     sky130_fd_sc_hd__o311ai_2       1
     sky130_fd_sc_hd__o31a_2         8
     sky130_fd_sc_hd__o31ai_2        4
     sky130_fd_sc_hd__o32a_2         6
     sky130_fd_sc_hd__o32ai_2        1
     sky130_fd_sc_hd__o41a_2         3
     sky130_fd_sc_hd__or2_2        294
     sky130_fd_sc_hd__or2b_2        38
     sky130_fd_sc_hd__or3_2         30
     sky130_fd_sc_hd__or3b_2        24
     sky130_fd_sc_hd__or4_2         25
     sky130_fd_sc_hd__or4b_2         8
     sky130_fd_sc_hd__xnor2_2       60
     sky130_fd_sc_hd__xor2_2        42

   Chip area for module '\picorv32': 99853.267200
```

## Day 2
### Chip Floorplanning Considerations
**Utilisation Factor**: The ratio of area occupied by the cells in the netlist to the total area of the core. Best practice is to set the utilisation factor less than 50% so that there will be space for optimisations, routing, inserting buffers etc.
```
Utilization Factor = Area occupied by netlist
                     -------------------------
                      Total area of the core 
```
**Aspect Ratio**: Aspect ratio is the ratio of height to the width of the die. Aspect Ratio of 1 indicates that the die is a square die.
```
Aspect Ratio = Height of Core
              ----------------
               Width of Core
```
### Floorplanning and Placement
Floorplanning and placement involve following stages:
* Pre-Placed cells
* Decoupling Capacitors to the pre placed cells
* Power Planning
* Pin Placement

#### Steps to perform Floorplanning and Placement
To perform floorplanning:
```
run_floorplanning
```
