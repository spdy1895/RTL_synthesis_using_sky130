# RTL Design, Synthesis and Optimization using Yosys and SKYWATER130 PDKs 
![Verilog-flyer](https://user-images.githubusercontent.com/68396186/120019036-93aead80-c005-11eb-8d90-11b52d5f81e1.png)


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
### Timing libs, hierarchical vs flat synthesis and efficient flop coding styles

#### Introduction to timing .libs
- - - -
>> ![skywater-pdk-logo](https://user-images.githubusercontent.com/68396186/120022278-cfe40d00-c009-11eb-95d2-40494dc9dd90.png)
>>> **sky130_fd_sc_hd_tt_025C_1v80.lib** 
>>>> the liberty timing file(.lib) is an ASCII file consists of detailed information of timing and power parameters about any standard cell of a particular technology node. The parameters are  affected by PVT variations (process, voltage, temperature). These variations are factorized while designing IC. So the Standard Cell library is characterized to model these variations. The feature size in this technology node is 130nm. The terms in the .lib file are:-
 - fd    - the skywater foundary
 - sc    - digital standard cell
 - hd    - high density
 - tt    - typical process
 - 025C  - temperature
 - 1v    - voltage 


> library in the sky130 pdk are named using the following scheme:
>> ""  Process name _ Library Source Abbreviation _ Library Type Abbreviation _ Library Name  ""

Library Source                 |    Abbreviation
-----------------------------  |----------------------
The SkyWater Foundary          |       fd
Efabless                       |       ef
Oklahoma State University      |       osu
  
  
Library Type                   |    Abbreviation
-------------------------------|----------------------
Primitive Cells                |       pr
Digital Standard Cells         |       sc
Build Space(Flash, SRAM, etc)  |       sp
IO and Periphery               |       io
High Densiry                   |       hd
  
#### SkyWater Foundry Provided Standard Cell Libraries

- sky130_fd_sc_hd - High Density Standard Cell Library.
- sky130_fd_sc_hdll - High Density, Low Leakage Standard Cell Library.
- sky130_fd_sc_hs - Low Voltage (<2.0V), High Speed, Standard Cell Library.
- sky130_fd_sc_ms - Low Voltage (<2.0V), Medium Speed, Standard Cell Library.
- sky130_fd_sc_ls - Low Voltage (<2.0V), Low Speed, Standard Cell Library.
- sky130_fd_sc_lp - Low Voltage (<2.0V), Low Power, Standard Cell Library.
- sky130_fd_sc_hvl - High Voltage (5V), Standard Cell Library.


more details can be found at [skywater-pdf](https://skywater-pdk.readthedocs.io/en/latest/contents/libraries/foundry-provided.html "skywter-pdk").

> standard cell definition of  and3_or1_invert:-
>> ![standard_cell_definition](https://user-images.githubusercontent.com/68396186/120035686-99fc5400-c01c-11eb-91e0-551a6f6214af.png)

> area comparison of different types of and cells.
>> ![area_comparison](https://user-images.githubusercontent.com/68396186/120036977-881bb080-c01e-11eb-9367-ab868377c64e.png)
 

#### Hierarchical vs Flat Synthesis
- - - -
> hierarchical synthesis
>> the synthesis results obtained after hierarchical synthesis shows that the sub-modules and the hierarchy of it's instantiation is preserved. The design is partitioned into a much higher level of abstraction.
>>  ![synth_multiple_module](https://user-images.githubusercontent.com/68396186/120040031-82749980-c023-11eb-9ca5-e48db01dbe6a.png)


> logical description of hierarchical synthesis
>>  ![show_hierar](https://user-images.githubusercontent.com/68396186/120039107-0168d280-c022-11eb-88c3-395b888660cc.png)

> netlist of hierarchical synthesis
>>  ![netlist_noattr](https://user-images.githubusercontent.com/68396186/120039264-3aa14280-c022-11eb-83f4-46c223036485.png)


**Note: in the Hierarchical Synthesis the design is partitioned into sub-modules hence it becomes easy to trace back each component of the design. Also the design complexity is less and easy to comprehend.** 


> flat synthesis

    $ yosys> flatten                                            // to invoke flat synthesis after netlist generation
    $ yosys> write_verilog -noattr multiple_modules_flat.v      // to write verilog netlist without attributes(clean)

>> in the flat synthesis, instantiation of standard cells take place and the design is partitioned into a much lower level of abstraction.

> logical description of flat synthesis
>> ![show_flattened](https://user-images.githubusercontent.com/68396186/120041355-b650be80-c025-11eb-9812-3b1363c88eaa.png)


> netlist comparison of flat synthesis vs hierarchical synthesis
>> ![netlist_hie_flat](https://user-images.githubusercontent.com/68396186/120041275-94efd280-c025-11eb-90dd-2d61491b5208.png)

#### Sub-module level synthesis

    $ yosys> synth -top sub_module1           // to synthesize only the submodule
    
    
- sub-module synthesis is preferred if:
  * there are multiple instances of same module. So it's better to synthesize the module once and then use it multiple times.
  * if desing and conquer approach is required i.e. if the design is very massive and complex, so it's better to synthesize module by module and then finally stitch together all the modules into the top module.




#### Various Flip Flop coding styles and optimization
- - - -

> *asynchronous reset dff*

    always@(posedge clk or posedge async_resset) begin 
        if(async_resset)  q<= 1'b0;
        else              q<= d_in;
    end
>> ![async_rst_01](https://user-images.githubusercontent.com/68396186/120044025-a8516c80-c02a-11eb-946b-0112343fffaf.png)
>> ![asyn_rst_show](https://user-images.githubusercontent.com/68396186/120044322-3cbbcf00-c02b-11eb-9fec-6a2acdd918fd.png)


> *asynchronous set dff*

    always@(posedge clk or posedge async_set) begin
        if(async_set)  q<= 1'b1;
        else           q<= d_in;
    end
>> ![async_set](https://user-images.githubusercontent.com/68396186/120044443-7e4c7a00-c02b-11eb-811a-0cf18a6a5c3a.png)
>> ![async_set_show_synth](https://user-images.githubusercontent.com/68396186/120044488-945a3a80-c02b-11eb-9d17-c96243639a95.png)


> *asynchronous-synchronous reset dff*

    always@(posedge clk or posedge async_reset) begin
        if(async_reset)     q<= 1'b0;
        else if(sync_reset) q<= 1'b0;
        else                q<= d_in;
    end
>> ![dff_asyncres_syncres_vcd](https://user-images.githubusercontent.com/68396186/120059288-300c9a80-c06e-11eb-8dd9-30387a6d52ad.png)
>> ![asyncres_synres_show_abc](https://user-images.githubusercontent.com/68396186/120059297-461a5b00-c06e-11eb-9fe2-a95c94d12213.png)









