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
sudo make mount
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
To perform floorplanning in tcl console:
```
run_floorplan
```

![alt text](https://github.com/aamodbk/iiitb_ASIC_OpenLane/blob/main/floorplan_v.png)

To view layout in magic:
```
cd /home/aamod/Stoodies/wslfiles/Sem-7/ASIC/OpenLane/designs/picorv32a/runs/RUN_2023.10.01_16.41.42/results/floorplan
magic -T /home/aamod/Stoodies/wslfiles/Sem-7/ASIC/OpenLane/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.nom.lef def read picorv32.def &
```

![alt text](https://github.com/aamodbk/iiitb_ASIC_OpenLane/blob/main/magic_floorplan.png)

To perform placement in tcl console:
```
run_floorplan
```

![alt text](https://github.com/aamodbk/iiitb_ASIC_OpenLane/blob/main/placement_v.png)

To view layout in magic:
```
cd /home/aamod/Stoodies/wslfiles/Sem-7/ASIC/OpenLane/designs/picorv32a/runs/RUN_2023.10.01_16.41.42/results/placement
magic -T /home/aamod/Stoodies/wslfiles/Sem-7/ASIC/OpenLane/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.nom.lef def read picorv32.def &
```

![alt text](https://github.com/aamodbk/iiitb_ASIC_OpenLane/blob/main/magic_placement.png)

### Cell Design and Characterization Flows
Library is a place where we get information about every cell. It has differents cells with different size, functionality,threshold voltages. There is a typical cell design flow steps.
* Inputs : PDKS(process design kit) : DRC & LVS, SPICE Models, library & user-defined specs.
* Design Steps :Circuit design, Layout design (Art of layout Euler's path and stick diagram), Extraction of parasitics, Characterization (timing, noise, power).
* Outputs: CDL (circuit description language), LEF, GDSII, extracted SPICE netlist (.cir), timing, noise and power .lib files

### Standard Cell Characterization Flow
A typical standard cell characterization flow that is followed in the industry includes the following steps:
1. Read in the models and tech files
2. Read extracted spice Netlist
3. Recognise behavior of the cells
4. Read the subcircuits
5. Attach power sources
6. Apply stimulus to characterization setup
7. Provide neccesary output capacitance loads
8. Provide neccesary simulation commands

Now all these 8 steps are fed in together as a configuration file to a characterization software called GUNA. This software generates timing, noise, power models. These .libs are classified as Timing characterization, power characterization and noise characterization.

### Timing Characterization
In standard cell characterisation, One of the classification of libs is timing characterisation.

#### Timing threshold definitions
Timing definition  | Value
------------- | -------------
slew_low_rise_thr |	20% value
slew_high_rise_thr |	80% value
slew_low_fall_thr |	20% value
slew_high_fall_thr |	80% value
in_rise_thr |	50% value
in_fall_thr |	50% value
out_rise_thr |	50% value
out_fall_thr |	50% value

#### Propagation Delay and Transition Time
The time difference between when the transitional input reaches 50% of its final value and when the output reaches 50% of its final value is called propagational delay. Poor choice of threshold values lead to negative delay values. Even thought you have taken good threshold values, sometimes depending upon how good or bad the slew, the delay might be still +ve or -ve.
```
Propagation delay = time(out_thr) - time(in_thr)
```

The time it takes the signal to move between states is the transition time , where the time is measured between 10% and 90% or 20% to 80% of the signal levels.
```
Rise transition time = time(slew_high_rise_thr) - time (slew_low_rise_thr)

Low transition time = time(slew_high_fall_thr) - time (slew_low_fall_thr)
```

## Day 3
### Design and view inverter layout by VLSI System Design
```
git clone https://github.com/nickson-jose/vsdstdcelldesign.git
```

To view the layout of the inverter in magic:
```
magic -T ./libs/sky130A.tech sky130_inv.mag &
```

![alt text](https://github.com/aamodbk/iiitb_ASIC_OpenLane/blob/main/sky130_inv_magic.png)

### Spice Extraction from Magic
To extract spice netlist from magic use the following command in magic tkon window:
```
extract all
ext2spice cthresh 0 rthresh 0
ext2spice
```

ext2spice commands converts the ext file to spice netlist. cthresh and rthresh are the switches to extract all the parasitic resistance and capacitance. The extracted spice list has to be modified as shown below to use ngspice to perform simulation:
```
* SPICE3 file created from sky130_inv.ext - technology: sky130A

.option scale=10m
.include ./libs/pshort.lib
.include ./libs/nshort.lib

//.subckt sky130_inv A Y VPWR VGND
M1000 Y A VGND VGND nshort w=35 l=23
+  ad=1.44n pd=0.152m as=1.37n ps=0.148m
M1001 Y A VPWR VPWR pshort w=37 l=23
+  ad=1.44n pd=0.152m as=1.52n ps=0.156m

VDD VPWR 0 3.3V
VSS VGND 0 0V
Va A VGND PULSE(0V 3.3V 0 0.1ns 0.1ns 2ns 4ns)

C0 VPWR Y 0.117f
C1 Y A 0.0754f
C2 VPWR A 0.0774f
C3 Y VGND 0.279f
C4 A VGND 0.45f
C5 VPWR VGND 0.781f
//.ends

.tran 1n 20n
.control
run
.endc
.end

```

To simulate the generated spice file type the following:
```
ngspice sky130_inv.spice
```

Next in the spice terminal type the following to plot the wave form type:
```
plot y vs time a
```

![alt text](https://github.com/aamodbk/iiitb_ASIC_OpenLane/blob/main/ngspice.png)

### Standard cell Characterization of CMOS Inverter
Characterization of the inverter standard cell depends on Four timing parameters
* Rise Transition: Time taken for the output to rise from 20% to 80% of max value
* Fall Transition: Time taken for the output to fall from 80% to 20% of max value
* Cell Rise delay: difference in time(50% output rise) to time(50% input fall)
* Cell Fall delay: difference in time(50% output fall) to time(50% input rise)

The above timing parameters can be computed by noting down various values from the ngspice waveform.
```
Rise Transition : 2.25421 - 2.18636 = 0.006785 ns / 67.85ps
Fall Transition : 4.09605 - 4.05554 = 0.04051ns / 40.51ps
Cell Rise Delay : 2.21701 - 2.14989 = 0.06689ns / 66.89ps
Cell Fall Delay : 4.07816 - 4.05011 = 0.02805ns / 28.05ps
```

To get a grid and to ensure the ports are placed correctly we can use this command in the tkcon console:
```
grid 0.46um 0.34um 0.23um 0.17um
```

## Day 4
### Custom cell naming and Lef Extraction
To save the new .mag file, save it with a different name:
```
%   save sky130_vsdinv.mag
```

Then, in the tkcon console type the following to generate the .lef file:
```
$   lef write
```

This generates `sky130_vsdinv.lef` file.

### Steps to include custom cell in ASIC design
We have created a custom standard cell in previous steps of an inverter. Into the `OpenLane\designs\picorv32a\src` folder, copy the following files:
* `sky130_fd_sc_hd__fast.lib`
* `sky130_fd_sc_hd__slow.lib`
* `sky130_fd_sc_hd__typical.lib`
* `sky130_vsdinv.lef`

Now, edit the created file `config.tcl` to look like:
```
set ::env(DESIGN_NAME) "picorv32a"

set ::env(VERILOG_FILES) "$::env(DESIGN_DIR)/src/picorv32a.v"

set ::env(CLOCK_PORT) "clk"
set ::env(CLOCK_NET) $::env(CLOCK_PORT)

set ::env(GLB_RESIZER_TIMING_OPTIMIZATIONS) {1}

set ::env(LIB_SYNTH) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"
set ::env(LIB_SLOWEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib"
set ::env(LIB_FASTEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib"
set ::env(LIB_TYPICAL) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"

set filename $::env(DESIGN_DIR)/$::env(PDK)_$::env(STD_CELL_LIBRARY)_config.tcl
if { [file exists $filename] == 1} {
	source $filename
}
```

Change current directory to `OpenLane` and run the following on the terminal:
```
$   sudo make mount
```
When the OpenLane container opens type the following to open the TCL console:
```
$   ./flow.tcl -interactive
```

Then run the below commands on the TCL console:
```
%   package require openlane 0.9
%   prep -design picorv32a
```
Now type the following command to merge the .lef files into a new file named `merged.nom.lef` in the `designs/picorv32a/runs/` folder:
```
%   set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
%   add_lefs -src $lefs
```
Next run the `run_synthesis` command.
Synthesis and STA Logs:

![alt text](https://github.com/aamodbk/iiitb_ASIC_OpenLane/blob/main/synth_sta_log.png)

Next type in the `run_floorplan` and `run_placement` command and check the generated layout in magic.

![alt text](https://github.com/aamodbk/iiitb_ASIC_OpenLane/blob/main/magic_vsd_inv.png)

### Post-Synthesis timimg analysis using OpenSTA
Timing analysis is carried out outside the openLANE flow using OpenSTA tool. For this, `pre_sta.conf` is required to carry out the STA analysis. Invoke OpenSTA outside the openLANE flow as follows:
```
sta pre_sta.conf
```
Copy the SDC file required for openSTA into the `/src` directory of the design.

### Clock Tree Synthesis in OpenLane
To run CTS use the below command in the tcl console:
```
run_cts
```
After running CTS the obtained worst setup and hold slack are as follows:

![alt text](https://github.com/aamodbk/iiitb_ASIC_OpenLane/blob/main/slacks.png)

Next post CTS analysis is performed by OpenRoad within OpenLane flow.
```
read_lef ./designs/picorv32a/runs/RUN_2023.10.01_23.15.12/tmp/merged.nom.lef
read_def ./designs/picorv32a/runs/RUN_2023.10.01_23.15.12/results/cts/picorv32a.def
write_db pico_cts.db
read_db pico_cts.db
read_verilog ./designs/picorv32a/runs/RUN_2023.10.01_23.15.12/results/synthesis/picorv32a.v
read_liberty $::env(LIB_SYNTH_COMPLETE)
read_sdc ./designs/picorv32a/src/my_base.sdc
set_propagated_clock (all_clocks)
report_checks -path_delay min_max -format full_clock_expanded -digits 4
```
Hold Slack:

![alt text](https://github.com/aamodbk/iiitb_ASIC_OpenLane/blob/main/hold_slack.png)

Setup Slack:

![alt text](https://github.com/aamodbk/iiitb_ASIC_OpenLane/blob/main/setupslack.png)

## Day 5
### Maze Routing and Lee's Algorithm
Maze routing is a problem-solving technique used in various fields, including computer science, engineering, and robotics. It involves finding a path through a maze or grid-like structure from a starting point to a destination while avoiding obstacles or obstacles with certain properties. Maze routing is essential in various applications, such as circuit design, robotics path planning, and network routing.
The Maze Routing algorithm, such as the Lee algorithm, is one approach for solving routing problems. In this method, a grid similar to the one created during cell customization is utilized for routing purposes. The Lee algorithm starts with two designated points, the source and target, and leverages the routing grid to identify the shortest or optimal route between them.
However, the Lee algorithm has limitations. It essentially constructs a maze and then numbers its cells from the source to the target. While effective for routing between two pins, it can be time-consuming when dealing with millions of pins. There are alternative algorithms that address similar routing challenges.

### Design Rule Check (DRC)
DRC verifies whether a design meets the predefined process technology rules given by the foundry for its manufacturing. DRC checking is an essential part of the physical design flow and ensures the design meets manufacturing requirements and will not result in a chip failure. It defines the Quality of chip.

**Design rules for physical wires**:
Minimum width of the wire, Minimum spacing between the wires, Minimum pitch of the wire To solve signal short violation, we take the metal layer and put it on to upper metal layer. we check via rules, Via width, via spacing.

### Power Distribution Network Generation
Unlike the general ASIC flow PDN is not a part of the flow run by OpenLane. PDN must be generated after CTS and post-CTS STA analyses:
We can check whether PDN has been created or no by check the current `def` environment variable:  `echo $::env(CURRENT_DEF)`
```
prep -design picorv32a -tag RUN_2023.10.01_23.15.12
gen_pdn
```
Once the command is given, power distribution network is generated.

### Routing
In the realm of routing within Electronic Design Automation (EDA) tools, such as both OpenLANE and commercial EDA tools, the routing process is exceptionally intricate due to the vast design space. To simplify this complexity, the routing procedure is typically divided into two distinct stages: Global Routing and Detailed Routing.
* Global Routing: In this stage, the routing region is subdivided into rectangular grid cells and represented as a coarse 3D routing graph. This task is accomplished by the "FASTE ROUTE" engine.
* Detailed Routing: Here, finer grid granularity and routing guides are employed to implement the physical wiring. The "tritonRoute" engine comes into play at this stage. "Fast Route" generates initial routing guides, while "Triton Route" utilizes the Global Route information and further refines the routing, employing various strategies and optimizations to determine the most optimal path for connecting the pins.

#### TritonRoute features
* Performs detailed routing and honors the pre-processed route guides (made by global route) and uses MILP based (Mixed Integer Linear Programming algorithm) panel routing scheme(uses panel as the grid guide for routing) with intra-layer parallel routing (routing happens simultaneously in a single layer) and inter-layer sequential layer (routing starts from bottom metal layer to top metal layer sequentially and not simultaneously).
* Honors preferred direction of a layer. Metal layer direction is alternating (metal layer direction is specified in the LEF file e.g. met1 Horizontal, met2 Vertical, etc.) to reduce overlapping wires between layer and reduce potential capacitance which can degrade the signal.
TritonRoute is a sophisticated tool that not only performs initial detail routing but also places a strong emphasis on optimizing routing within pre-processed route guides by breaking down, merging, and bridging them as needed to achieve efficient and effective routing results.

Perform the routing using the following command:
```
run_routing
```

