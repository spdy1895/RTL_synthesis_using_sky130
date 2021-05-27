# RTL Design, Synthesis and Optimization using Yosys and SKYWATER130 PDKs 
![SkyWater_logo](https://user-images.githubusercontent.com/68396186/119770266-93b18f00-bed9-11eb-9937-8728a79a44f6.png)



In VLSI industry Front-end digital design inlcudes RTL design, synthesis, optimization and verification. In the current repository i have included the deisgn, synthesis and optimization work on various digital modules, from very simple digital logic (mux, decoder, encoder etc.) to complex designs(barrel shifter, booth multiplier, dual port ram etc). All the work has been done using open source tools:-
 * iverilog for RTL simulation.
 * gtkwave for reading .vcd file and waveform generation.
 * yosys for synthesis and netlist generation.
 * skywater 130nm open source pdk.

## Introduction Of OPEN-SOURCE HARDWARE And TOOLS
With the introduction of open-source tools in the ASIC flow, now VLSI engineers have opportunity to design and craft their own ideas starting from RTL 2 GDSII. Moreover further inclusion of VLSI enthusiast and learners have propelled the need of open-source community for hardware also. Organizations such as:-
  - open source hardware association 
  - SiFive
  - FOSSI
  - OpenPOWER Foundation
  - VSD
  - RISCV
  - Redwood EDA 
  - Efabless etc

are committed to bring revolution in the Silicon industry. 

### RTL DESIGN AND SYNTHESIS USING SKYWATER130 PDK ###
- - - -
*I have categorized my work in day wise learning, according to the workshop.*
- - - -
__Table Of Contents__
  1. Day 01 
      1. Introduction to verilog RTL design and Synthesis
          1. introduction to iverilog and gtkwave.
          2. introduction to yosys.
  2. Day 02
  3. Day 03
  4. Day 04
  5. Day 05

_ _ _ _
**DAY01**
#### Introduction to verilog RTL Design and Synthesis ####

> RTL Design
>> *It's a way of writng hdl code for digital design in a way to exploit the behavioral features offered by the language and also utilizing the structural description for design accuracy. So it's a trade off between behavioral constructs and structural description while writing hardware code.*   

> Synthesis
>> *Translation and optimization of the RTL code into the gate level netlist. The synthesis tool(yosys) takes hdl code, liberty file(.lib) and design constraints to generate gate level netlist.*

> Netlist
>> *Representation of the design in terms of actual standard cells and their connections.*

> .lib
>> *Liberty Timing file consist of timing and power parameters associated with standard cells specific to a particular technology node. Generally it includes definition of an inverter, nand2, nand3, nand4, nor2, nor3, nor4, aoi12, aoi22, oai12, oai22, mux, dff etc.*


