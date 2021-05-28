# RTL Design, Synthesis and Optimization using Yosys and SKYWATER130 PDKs 
![SkyWater_logo](https://user-images.githubusercontent.com/68396186/119783113-86e96700-beea-11eb-9830-f5a102e1dbfb.png)


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
*I have categorized my work in day wise learning, according to the workshop conducted by VSD.*
- - - -
__Table of Contents__
=================

  * [Day 01](#day-01)
    * [Introduction to verilog RTL design and synthesis](#introduction-to-verilog-rtl-design-and-synthesis)
        * [RTL Design](#rtl-design)
        * [Synthesis](#synthesis)
        * [Netlist](#netlist)
        * [Lib file](#lib-file)
    * [Synthesis using yosys](#synthesis-using-yosys)
        * [Invoke yosys shell](#invoke-yosys-shell)
        * [Commands for synthesis using yosys](#commands-for-synthesis-using-yosys)
        * [Results](#results)
  * [Day 02](#day-02)
_ _ _ _
## DAY 01
### Introduction to verilog RTL design and synthesis 

#### RTL Design 
>> *It's a way of writng hdl code for digital design in a way to exploit the behavioral features offered by the language and also utilizing the structural description for design accuracy. So it's a trade off between behavioral constructs and structural description while writing hardware code.*   

#### Synthesis
>> *Translation and optimization of the RTL code into the gate level netlist. The synthesis tool(yosys) takes hdl code, liberty file(.lib) and design constraints to generate gate level netlist.*

#### Netlist
>> *Representation of the design in terms of actual standard cells and their connections.*

#### Lib file
>> *Liberty Timing file(.lib) consist of timing and power parameters associated with standard cells specific to a particular technology node. Generally it includes definition of an inverter, nand2, nand3, nand4, nor2, nor3, nor4, aoi12, aoi22, oai12, oai22, mux, dff etc.*

**NOTE**
> Why Standard Cell library includes various(in terms of PPA) types of gate cells?
>> In the digital circuit some combinational paths requires cells that are very fast in performance(but requires more area and power) and likewise some  combinational paths requires cells that are slower in performance(but better in terms of area and power). So adhering to the timing conditions, which standard  cells are to be used the decision is made. These decisions are written in constraint file and are passed to synthesis tool(yosys) for guided synthesis.     

### Synthesis using yosys
#### Invoke yosys shell
               
       $ yosys
>> ![shubh_yosys_invoke](https://user-images.githubusercontent.com/68396186/119873243-f9396600-bf41-11eb-97c7-e27f6c993052.png)


    yosys>
   
#### Commands for synthesis using yosys
 
        yosys> read_liberty -lib ../../path_to_the_standard_library_file.lib         // reading the liberty file.
        yosys> read_verilog ../../path_to_verilog_file_that_is_to_be_synthesized.v   // reading the verilog file.
        yosys> synth -top module_name                                                // the module name that is to be synthesized.
        yosys> abc -liberty  ../../path_to_to_liberty_file.lib                       // netlist generation.
        yosys> show                                                                  // a gui based representation of logic synthesized.
        yosys> write_verilog file_name.v                                             // write the generated netlist to new a module/verilog
                                                                                     // file for verification.
#### Results                                                                                   
> result of the command "yosys> synth -top module_name"
>> ![shubh_synth_result](https://user-images.githubusercontent.com/68396186/119875101-00617380-bf44-11eb-8ebc-7f7950b76983.png)



>* result of the command "yosys> abc -liberty ../../path_to_liberty_file.lib"
> __NOTE__
>>- the difference in synthesis performed by various version of yosys. 

>* synthesis performed by yosys 0.7.
>> ![shubh_abc_result](https://user-images.githubusercontent.com/68396186/119875178-1bcc7e80-bf44-11eb-8982-451f402b60dd.png)



>* logical representation of the deisgn by yosys 0.7. 
>> ![shubh_show](https://user-images.githubusercontent.com/68396186/119875277-3272d580-bf44-11eb-9923-d6fd501b62c5.png)



>* synthesis performed by yosys 0.9.
>> ![shubh_abc_01](https://user-images.githubusercontent.com/68396186/119875888-e96f5100-bf44-11eb-8847-5a179941a37b.png)



> * logical representation of the design by yosys 0.9. 
>> ![shubh_show_01](https://user-images.githubusercontent.com/68396186/119877361-926a7b80-bf46-11eb-8369-49c7503066ba.png)




__NOTE__
> the synthesis performed by yosys 0.7 is choosing different standard cells as compared to synthesis by yosys 0.9. 

>* netlist generated by yosys 0.7.
>>![netlist_generated](https://user-images.githubusercontent.com/68396186/119836746-98009b00-bf1f-11eb-8e10-f7685173bec7.png)


- - - - 
## Day 02
__________
### Timing libs, hierarchical vs flat synthesis and efficient flop coding styles

#### Introduction to timing .libs
