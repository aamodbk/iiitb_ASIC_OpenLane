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

![alt text](https://github.com/aamodbk/iiitb_ASIC_OpenLane/blob/main/synth_rpt.png)

