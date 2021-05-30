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
    * [Timing library hierarchical vs flat synthesis and efficient flop coding styles](#timing-library-hierarchical-vs-flat-synthesis-and-efficient-flop-coding-styles)
        * [Introduction to timing library](#introduction-to-timing-library)
        * [Skywater Foundary provided standard cell libraries](#skywater-foundary-provided-standard-cell-libraries)
        * [Hierarchical vs Flat Synthesis](#hierarchical-vs-flat-synthesis)
        * [Various Flip Flop coding styles and optimization](#various-flip-flop-coding-styles-and-optimization)
        * [Optimization in constant multiplier circuit](#optimization-in-constant-multiplier-circuit)

  * [Day 03](#day-03)
    * [Combinational and Sequential optimization](#combinational-and-sequential-optimization)
        * [Introduction to optimization](#introduction-to-optimization)
        * [Combinational logic optimization](#combinational-logic-optimization)
        	* [Multiple module optimization](#multiple-module-optimization)
      	* [Sequential logic optimization](#sequential-logic-optimization)
      	* [Sequential optimization for unused outputs](#sequential-optimization-for-unused-outputs)

  * [Day 04](#day-04)
    * [GLS, blocking vs non-blocking and synthesis-simulation mismatch](#gls-blocking-vs-non-blocking-and-synthesis-simulation-mismatch)
     	* [Comparing RTL simulation and Gate level simulaiton](#comparing-rtl-simulation-and-gate-level-simulation)
     	* [Synthesis-Simulation mismatch](#synthesis-simulation-mismatch) 

   * [Day 05](#day-05)
     * [If case, for loop and for generate](#if-case-for-loop-and-for-generate) 





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
4



__NOTE__
> the synthesis performed by yosys 0.7 is choosing different standard cells as compared to synthesis by yosys 0.9. 

>* netlist generated by yosys 0.7.
>>![netlist_generated](https://user-images.githubusercontent.com/68396186/119836746-98009b00-bf1f-11eb-8e10-f7685173bec7.png)


- - - - 
## Day 02
### Timing library hierarchical vs flat synthesis and efficient flop coding styles
- - - -
#### Introduction to timing library

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
  
#### Skywater Foundary provided standard cell libraries

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

    always@(posedge clk or posedge async_reset) begin 
        if(async_reset)  q<= 1'b0;
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


> *synchronous reset dff*

    always@(posedge clk) beign
        if(sync_reset)  q<= 1'b0;
        else            q<= d_in;
     end
>> ![syncres_waveform](https://user-images.githubusercontent.com/68396186/120065028-e7b0a500-c08c-11eb-890c-355cdb9bac3a.png)
>> ![dff_synres_show](https://user-images.githubusercontent.com/68396186/120065073-1cbcf780-c08d-11eb-9d09-271adad9941e.png)

__Note__

* Asynchronous reset pin is directly synthesized on the flip flop.
* Synchronous reset signal is passed through a combinational logic to the flip flop. 

#### Optimization in constant multiplier circuit
    module mul2(
      output wire y[3:0],
      input wire in[2:0]
    );
        assign y= a * 2;
    endmodule
    // there is an interesting optimiztion taking place here instead of generating any hardware for the logic.
    
__Note__
* there are only wire components in the circuit. Also note the comment by the synthesis tool(yosys) while mapping.
>> ![mult2_abc](https://user-images.githubusercontent.com/68396186/120066491-53e2d700-c094-11eb-8193-66ec935f6e18.png)
>> * netlist
>> ![mult2_netlist](https://user-images.githubusercontent.com/68396186/120066553-bfc53f80-c094-11eb-8216-21c8adacdb49.png)
>> * logical description
>> ![mult2_show](https://user-images.githubusercontent.com/68396186/120066569-ceabf200-c094-11eb-88ef-3516e4b9d3c6.png)

##### Similar optimization
    module mul8(
        output wire y[5:0],
        input wire in[2:0]
        );
            assign y= a * 9;
    endmodule
    
__Note__
* here replication of input takes place.
>> ![mult8_abc_response](https://user-images.githubusercontent.com/68396186/120066905-6c53f100-c096-11eb-813f-253595a0559e.png)
>> * netlist
>> ![mult8_netlist](https://user-images.githubusercontent.com/68396186/120066923-842b7500-c096-11eb-8869-53105d92d581.png)
>> * logical description
>> ![mult8_show](https://user-images.githubusercontent.com/68396186/120066939-95748180-c096-11eb-9c02-dd69d1722cd0.png)





- - - -
## Day 03
### Combinational and Sequential optimization
- - - -
#### Introduction to optimization
>> command to perform optimization
    
    $ yosys > opt_clean -purge        // all optimizations are done with this
                                      // enter this command after "synth"


#### Combinational logic optimization
>* case 1

    module opt_check(                             
    
      output wire y,
      input wire a,
      input wire b
    );
        assign y= a ? b : 0;
    endmodule

>>* expected optimization by tool.
>>> ![opt1_theory](https://user-images.githubusercontent.com/68396186/120073594-9c12f100-c0b6-11eb-83a1-0bae70efdb03.jpg)
>>* result obtained.
>>> ![opt1_show](https://user-images.githubusercontent.com/68396186/120073616-c2389100-c0b6-11eb-8f3c-0f3a33530c84.png)



>* Case 2

    module opt_check2(
      output wire y,
      input wire a,
      input wire b
    );
        assign y= a ? 1 : b;
    endmodule
>>* expected optimization by tool.
>>> ![opt2_theory](https://user-images.githubusercontent.com/68396186/120073637-d7adbb00-c0b6-11eb-80eb-3d9027af3834.jpg)
>>* result obtained.
>>> ![opt2_show](https://user-images.githubusercontent.com/68396186/120073644-e1372300-c0b6-11eb-9af4-09e5b5ff965d.png)


>* Case 3

    module opt_check3(
      output wire y,
      input wire a,
      input wire b,
      input wire c
    );
      assign y= a ? ( C ? b : 0 ) : 0;
    endmodule
>>* expected optmization by tool.

>>* result obtained.
>>> ![opt_check3_show](https://user-images.githubusercontent.com/68396186/120082459-dee9be80-c0e0-11eb-8ab7-154cdfacaa37.png)


>* Case 4

    module opt_check4(
      output wire y,
      input wire a,
      input wire b,
      input wire c
    );
        assign y= a ? ( b ? ( a & c ) : c ) : ( !c );
    endmodule
>>* expected optimization by tool.

>> result obtained.
>>> ![opt_check4](https://user-images.githubusercontent.com/68396186/120082644-ae565480-c0e1-11eb-8ac9-54a54e473578.png)



#### Multiple module optimization
__Note__
> before running optimization command, first flatten the design.
      
    $ yosys > synth -top module_name              // synthesizing the top module
    $ yosys > flatten                               // flattening the entire design and removing the hierarchy
    $ yosys > opt_clean -purge                      // running optimization
    $ yosys > abc -liberty /path_to_.lib            // mapping into the liberty file standard cells
    $ yosys > write_verilog file.v                  // writing netlist

    
    
>* Case 5

    module sub_module1(input a , input b , output y);
      assign y = a & b;
    endmodule


    module sub_module2(input a , input b , output y);
      assign y = a^b;
    endmodule


    module multiple_module_opt(input a , input b , input c , input d , output y);
      wire n1,n2,n3;

      sub_module1 U1 (.a(a) , .b(1'b1) , .y(n1));
      sub_module2 U2 (.a(n1), .b(1'b0) , .y(n2));
      sub_module2 U3 (.a(b), .b(d) , .y(n3));

      assign y = c | (b & n1); 


    endmodule
>> expected optimization by tool.
>>>
>> result obtained after optimization.
>>> ![multiple_module_opt1](https://user-images.githubusercontent.com/68396186/120084058-d0a0a000-c0ea-11eb-9c1d-f46d3f20e1ff.png)
>> netlist of the optimized design.
>>> ![multiple_module_opt_netlist](https://user-images.githubusercontent.com/68396186/120084177-cb902080-c0eb-11eb-99ca-787af1a9174a.png)


>* Case 6

    module sub_module(input a , input b , output y);
      assign y = a & b;
    endmodule

    module multiple_module_opt2(input a , input b , input c , input d , output y);
      wire n1,n2,n3;

        sub_module U1 (.a(a) , .b(1'b0) , .y(n1));
        sub_module U2 (.a(b), .b(c) , .y(n2));
        sub_module U3 (.a(n2), .b(d) , .y(n3));
        sub_module U4 (.a(n3), .b(n1) , .y(y));
    endmodule
>> expected optimization by tool.

>> result obtained after optimization.
>>> ![multiple_module_opt2_show](https://user-images.githubusercontent.com/68396186/120115120-ea4fef00-c19f-11eb-88e9-55b298c7f089.png)

>> netlist of the optimized design.
>>> ![multiple_module_opt2_newopt_netlist](https://user-images.githubusercontent.com/68396186/120115143-ff2c8280-c19f-11eb-9b0e-4b9d827f1f5e.png)




#### Sequential logic optimization
>* Case 1

    module dff_const1(
      output reg q,
      input wire clk,
      input wire reset
    );
      always @(posedge clk or posedge reset)
        begin
	        if(reset)
		          q <= 1'b0;
	        else
		          q <= 1'b1;
        end
    endmodule
    
>> statistics after synth command.
>>> ![dff_const1_synth](https://user-images.githubusercontent.com/68396186/120086869-5c70f700-c100-11eb-8e6c-6e7c0f803493.png)

>> result after optmization.
>>> ![dff_const1_show](https://user-images.githubusercontent.com/68396186/120086854-4105ec00-c100-11eb-80a3-6bfeaaea7dc0.png)


>* Case 2

    module dff_const2(
      output reg q,
      input wire clk,
      input wire reset
    );
    
    always@(posdge clk or posedge reset) begin
        if(reset) q<= 1'b1;
        else      q<= 1'b1;
    end
    endmodule
    
>> statistics after synth command.
>>> ![dff_const2_synth](https://user-images.githubusercontent.com/68396186/120086907-d2755e00-c100-11eb-8093-8cc67bc814ba.png)

>> result after optimization.
>>> ![dff_const2_show](https://user-images.githubusercontent.com/68396186/120086918-f2a51d00-c100-11eb-8d83-104a87691f15.png)


>* Case 3

    module dff_const3(
      output reg q,
      input wire clk,
      input wire reset
      );
        reg q1;
        
        always@(posedge clk or posedge reset) begin
        if(reset) begin
                q<= 1'b1;
                q1<= 1'b0;
        end
        else begin
              q1<= 1'b1;
              q<= q1;
        end
        end
        endmodule
        
>> statistics after synth command.
>>> ![dff_const3_synth](https://user-images.githubusercontent.com/68396186/120087351-b1167100-c104-11eb-9723-2a3c9eefc1f4.png)

>> result after optimization.
>>> ![dff_const3_show](https://user-images.githubusercontent.com/68396186/120087360-c5f30480-c104-11eb-873a-84142a17559d.png)


>* Case 4

    module dff_const4(
      output reg q,
      input wire clk,
      input wire reset
      );
      reg q1;
        always@(posedge clk or posedge reset) begin
          if(reset) begin
            q<= 1'b1;
            q1<= 1'b1;
          end
        else begin
            q1<= 1'b1;
            q<= q1;
          end
        end
    endmodule
>> statistics after synth command.
>>> ![dff_const4_synth](https://user-images.githubusercontent.com/68396186/120087695-bde89400-c107-11eb-8a44-f4dffc36fdc2.png)

>> result after optimization.
>>> ![dff_const4_show](https://user-images.githubusercontent.com/68396186/120087699-cd67dd00-c107-11eb-9e4c-3029e7bdff7b.png)


>* Case 5
    
    module dff_const5(
      output reg q,
      input wire clk,
      input wire reset
      );
        reg q1;
        always@(posedge clk or posedge reset) begin
            if(reset) begin
                q<= 1'b0;
                q1<= 1'b0;
                end
             else begin
                q1<= 1'b1;
                q<= q1;
                end
         end
    endmodule
    
>> statistics after synth command.
>>> ![dff_const5_synth](https://user-images.githubusercontent.com/68396186/120087928-aad6c380-c109-11eb-8215-1aeb001175bb.png)

>> result after optimization.
>>> ![dff_const5_show](https://user-images.githubusercontent.com/68396186/120087933-be822a00-c109-11eb-9095-7df0d8bbff0f.png)

#### Sequential optimization for unused outputs

>* Case 1 : 3 bit up-counter

    module counter_opt(
      output reg q,
      input wire clk,
      input wire reset
    );
        reg [2:0] count;
        assign q= count[0];
        
          always@(posedge clk or posege reset) begin
            if(reset) count< =3'b000;
            else      count<= count + 1;
          end
    endmodule
>> statistics after synth command.
>>> ![counter_opt1_synth](https://user-images.githubusercontent.com/68396186/120088231-3bae9e80-c10c-11eb-95ed-4219902684be.png)

>> result after optimization.
>>> ![counter_opt1_show](https://user-images.githubusercontent.com/68396186/120088256-5ed94e00-c10c-11eb-95be-9f169796a37d.png)



>* Case 2 : 3 bit up-counter


	module counter_opt(
	  output reg q,
      	  wire clk,
      	  wire reset
    	);
            reg [2:0] count;
            assign q= (count[2:0]== 3'b000);
        
              always@(posedge clk or posege reset) begin
                if(reset)  count< =3'b000;
                 else      count<= count + 1;
               end
    	endmodule
>> statistics after synth command.
>>> ![counter_opt_3_synth](https://user-images.githubusercontent.com/68396186/120115360-f5efe580-c1a0-11eb-84dc-18bb49eca3ce.png)

>> result after optimization.
>>> ![counter_opt_3_show](https://user-images.githubusercontent.com/68396186/120115375-030cd480-c1a1-11eb-8d1e-f6f33bebb629.png)




- - - -
## Day 04
### GLS, blocking vs non-blocking and synthesis-simulation mismatch
- - - -
#### Gate level simulation 
>> *using netlist generated by the synthesis tool as DUT and test bench(same test bench i.e. used for RTL simulation) the simulation tool iverilog can perform a logical simulation called as Gate level simulation. It is performed to verify the logical correctness of the design after synthesis.

#### Comparing RTL simulation and Gate level simulation
>* case 1:

	assign y= sel ? i1 : i0;		// infers a 2:1 mux
	
>> statistics after synth command.
>>> ![ternary_mux_synth](https://user-images.githubusercontent.com/68396186/120117185-8f22fa00-c1a9-11eb-88ca-d25a13d97bd9.png)

>> logical description.
>>> ![ternary_mux_show](https://user-images.githubusercontent.com/68396186/120117198-a06c0680-c1a9-11eb-820e-386b71f7496e.png)

>> RTL simulation.
>>> ![ternary_mux_waveform](https://user-images.githubusercontent.com/68396186/120117539-4ff5a880-c1ab-11eb-846c-221bea6104a7.png)

>> Gate level simulation.
>>> ![gls_waveform_ternary_mux](https://user-images.githubusercontent.com/68396186/120117574-656ad280-c1ab-11eb-815d-ca7f4d9dc9a6.png)

>* Case 2: 

	always@(sel) begin
		if(sel) y<= i1;
		else	y<= i0;	
	end
>> statistics after synth command.
>>> ![bad_mux_synth](https://user-images.githubusercontent.com/68396186/120117724-3739c280-c1ac-11eb-896c-09db2cb68890.png)

>> RTL simulation.
>>> ![bad_mux_waveform_before_synth](https://user-images.githubusercontent.com/68396186/120117754-52a4cd80-c1ac-11eb-8517-196ee5b143d2.png)

>> Gate level simulation.
>>> ![bad_mux_waveform_after_synth](https://user-images.githubusercontent.com/68396186/120117769-67816100-c1ac-11eb-9933-32d30b0832a0.png)


#### Synthesis-Simulation mismatch

	module blocking_caveat(
	   output reg d,
	   input wire a,
	   input wire b,
	   input wire c
	);
		reg x;
		always @(*) begin
		   d= x & c;
		   x= a | b;
	end
	endmodule
>> statistics after synth command.
>>> ![blocking_caveat_stats](https://user-images.githubusercontent.com/68396186/120118545-bfba6200-c1b0-11eb-8fbf-eb9062a4180e.png)

>> RTL simulation.
>>> ![blocking_caveat_rtl_waveform](https://user-images.githubusercontent.com/68396186/120118554-cd6fe780-c1b0-11eb-9a22-2fab06da181f.png)

>> Gate level simulation.
>>> ![blocking_caveat_gsl_synth_waveform](https://user-images.githubusercontent.com/68396186/120118572-e24c7b00-c1b0-11eb-8750-793325cb81dd.png)







- - - -
## Day 05
### If, case, for loop and for generate
- - - -
