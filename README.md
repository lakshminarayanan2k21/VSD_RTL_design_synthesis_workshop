# RTL Design and Synthesis using Verilog with SKY130 technology
![image](https://user-images.githubusercontent.com/89997921/131890710-6d1b156b-06cf-4183-b817-9f713338d517.png)
### About the Workshop 
   - Teach verilog coding guidelines resulting in predictable logic in Silicon
   - Importance of verilog code synthesizing to expected behaviour depends on coding styles used
   - The course details all these aspects of the Verilog HDL with theory and backed with lot of practical examples
   - Workshop introduces to the digital logic design using Verilog HDL
   - Validating the functionality of the design using Functional Simulation
   - Handon with functionsl and GLS test bench using iverilog & gtkwave for RTL and Netlist
   - Logic synthesis of the Functional RTL Code. 

### Tools Used

Verilog : Simulator . Used for RTL Simulation and Gate Level Simulations
yosys   : Opensource Logic Synthesis Tool
Skywater 130nm Standard Cell Libraries

Workshop Daywise content :- 


## Day Wise Contents 
1. [Day1](#Day1) Introduction to Verilog RTL design and Synthesis
2. [Day2](#Day2) Timing libs, hierarchical vs flat synthesis and efficient flop coding styles
3. [Day3](#Day3) Combinational and sequential optmizations
4. [Day4](#Day4) GLS, blocking vs non-blocking and Synthesis-Simulation mismatch
5. [Day5](#Day5) Optimization in synthesis


# 

Day  Topics Covered
Day 1  Introduction to Verilog RTL design and Synthesis


#### Day1
***Introduction to Verilog RTL design and Synthesis***

  **VERILOG CODE**
  
  Command used : **vim good_mux.v**
  
  ```verilog
  module good_mux (input i0 , input i1 , input sel , output reg y);
  always @ (*)
  begin 
         if(sel)
                  y <= i1;
          else
                  y <= i0;
   end
   endmodule
   ```
  

#### Day2
***Timing libs, hierarchical vs flat synthesis and efficient flop coding styles***

## Introduction to dot LIB part1


Library follows a naming convention that helps us to identify the three basic parameters .

Process      - Variation due to fabrication. 
Voltage      - Variation in voltage
Temperature  - Sensitivity to temperature 

This parameters determine how the silicon works. With all the changes in parameter the silicon is expected to work.
Basically a silicon will need to work in different parts of geographically working conditions whether it is extreme hot or cold temperatures.
Even a day also has variations in temperature we can build a chip works only in particular climatic conditions.

sky130_fd_sc_hd_tt_025C_1v80.lib

Here 025C represents 25 Degree
1v80 as 1.8volts
tt typical 

Technology - CMOS
Units of time , voltage , power (nw), capcitance(pf)

.lib is bucket of all the standard cells that are available 

cell marks the begining of the cell definition


Hierarchical Synthesis and Flat Synthesis

Module1 
Module2

stacked PMOS is bad 


write_verilog -noattr multiple_module_hier.v

#### Hierarchical synthesis preserves hierarchy and doesnt utilize the standard cells 

read_liberty -lib 
read_verilog multiple_modules.v
synth -top multiple_modules.v
abc -liberty ../my_lib/lib/sky

flatten 

show 

flatten  --> command to flatten the hierarchy

write_verilog -noattr multiple_module_flat.v

sub Module wise synthesize

1. why module level synthesize is preferred when we have multiple instances of same module.
2. Divide and Conquer --> Instead of giving a massive design we give sub modules and stitch them together at the top.

synth -top submodule1

## Why Flops and Flop Coding Styles

Glitches is caused by the delay through the logic gates in combinational circuits.
Adding more combinational circuits the outputs will never settle down.

An element to store the value of the combinational output to store them and restrict them.
The Output of the flop will change only when the clock changes.

## Why Flops and Flop coding styles part1
  Usage of Flop

## Why Flops and Flop coding styles part2
  Sync reset , sync set and Async reset along with sync reset and async reset signals in same circuit.

Lab Flop synthesis simulations part1
 Simulation of all types of dflipflops asynchronus , synchronous resets 
  dff_asyncres.v
  dff_async_set.v
  dff_syncres.v

##Lab Flop synthesis simulations part2
synthesis of all the above files 

read_verilog dff_asyncres.v
synth -top dff_asyncres
dfflibmap -liberty ../mylib/lib/
only will look for the dff libraries

dff_syncres.v  relaized with inverted reset connected to AND gate connected to flipflop 

## Intersting optimisations part1

mult2 
 Only append 0 at the end
 
 read_liberty
 read_verilog mult_2.v
 synth -top mult_2.v
 No cells will be infered 
 
## Intersting optimisations part2
 
mult8

##### Day3 
***Combinational and sequential optmizations***

#### Introduction to Optimizations

### Introduction to optimisations part1

# Combinational Logic Optimization
 Squeexing the logic to get the most optimised design
    Area and Power Savings 
    
# Constant Propagation
   Direct Optimisation

y = ((AB)+C)'
     if A=0 it is relaized as y = C' 


#Boolean Logic Optimisation
  K-Map
  Quine McKluskey

   assign y = a ? (b?c:(c?a:0)):(!c)
          
          y = a'c' + a(bc + b'ac)

          y = a'c' + ac 

### Introduction to optimisations part2
 *Sequential Logic Optimization*
 *Basic*
 *Sequential Constant propagation*      

Whenever the input is applied to a constant value whether it propagates constant values based on that it depends the values.
Examples were provided for D input tied to ground and considering set or reset pin as input with output being provided to Nand gate with one of the input as A.

![image](https://user-images.githubusercontent.com/89997921/132060453-ebf2d975-9c8b-4b6c-a35b-e536d983459c.png)

Advanced [ Not COvered as part of Lab]

## Introduction to optimisations part3
*State Optimisation*
Retiming --> usefulness of slack to get the benefit of performance 
Sequential Logic Cloning ( Floor Plan Aware Synthesis ) 
**Having two copies of A to resolve the timing issues** 
![image](https://user-images.githubusercontent.com/89997921/132061335-6cf9d906-ae5a-4686-9353-3080cc4780fa.png)

##Combinational Logic Optimization

**Lab06 Combinational Logic Optimisations part1**
verilog_files> ls *opt*
opt_check2.v opt_check3.v opt_check.v  lab ( opt_check4.v multiple_module_opt.v multiple_module_opt.v )
opt_check.v Mux optimized to AND gate
opt_check2.v Mux optimized to OR gate
after synth -top opt_check
opt_clean -purge --> command to do optimization
opt_check2 Inverted NAND gate
opt_check3 y = a?(c?b:0):0) --> y= abc
## Sequential Logic Optimization
**Lab07 Sequential Logic Optimisations part1**

```
ls *df*const*
```   

![image](https://user-images.githubusercontent.com/89997921/132086038-af7ad8f9-33c3-4e22-8c10-6429c4ff5785.png)

```
ls gvim -o dff_const1.v -O dff_const2.v
```   

![image](https://user-images.githubusercontent.com/89997921/132086155-5b3d250b-eff5-440e-a302-b78666bb6e97.png)

**Expected Logic Circuit for dff_const1**

![image](https://user-images.githubusercontent.com/89997921/132086248-8936f29c-b4d2-4160-b414-8a4dc7cd8fd6.png)


**Actual Realized circuit dff_const1**

![image](https://user-images.githubusercontent.com/89997921/132086264-ca92527b-1bbf-4466-ad01-a92d081e10cc.png)

**Actual Realized circuit dff_const2**

![image](https://user-images.githubusercontent.com/89997921/132086343-a9a69c59-196a-4399-8f93-6747271943cd.png)

**Syntehsis Result dff_const1**

![image](https://user-images.githubusercontent.com/89997921/132086464-56fdbbe6-9447-4cc6-8625-1e9843e5f69b.png)

Since library uses active low reset and our code uses active high there is one inverter in the path of the reset.

**Lab07 Sequential Logic Optimisations part2**


**Synthesis Result dff_const2**

![image](https://user-images.githubusercontent.com/89997921/132086614-611bb639-a2eb-44b6-89b1-7849c4b57877.png)

Since there is no usage of input and output there is no logic cell realized in the circuit after synthesis.

**Synthesis Result dff_const3**
![image](https://user-images.githubusercontent.com/89997921/132086749-178ede40-5cc6-4f4d-acda-4827b3ca95dc.png)

```
Question : Whether the flops will be optimized ?
Answer   : Not it can't be optimized the answer is in the below waveform
```
![image](https://user-images.githubusercontent.com/89997921/132087038-02c30dba-d3e3-42cf-83b2-5466e2b1e321.png)
**Lab07 Sequential Logic Optimisations part3**
```
iverilog dff_const3.v tb_dff_const3.v
./a.out
gtkwave tb_dff_const3.vcd
```
![image](https://user-images.githubusercontent.com/89997921/132087168-b5df61bc-8450-4804-ad24-9051705f896a.png)

```
yosys
yosys>read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> read_verilog dff_const3.v
yosys> synth -top dff_const3
yosys>dfflibmap -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys>abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys>show
```
![image](https://user-images.githubusercontent.com/89997921/132087300-9abfb2f9-2e14-4f73-adba-bbee09c6919e.png)

**Synthesis Result dff_const4**
**Synthesis Result dff_const5**
# Sequential Logic Optimization for Unused Outputs
**L1 Seq optimisation unused outputs part1**

Unused Output Optimization

```verilog
module counter_opt (input clk , input reset , output q);
reg [2:0] count;
assign q = count[0];

always @(posedge clk ,posedge reset)
begin
	if(reset)
		count <= 3'b000;
	else
		count <= count + 1;
end

endmodule
```

![image](https://user-images.githubusercontent.com/89997921/132087503-75d79b4f-fe4f-4694-a822-803ae729576f.png)

```
We have unused ports in the output port 
```

![image](https://user-images.githubusercontent.com/89997921/132087575-80732a08-a381-4901-ab71-6fd23e288279.png)

```
yosys
yosys>read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> read_verilog counter_opt.v
yosys> synth -top counter_opt
yosys>dfflibmap -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys>abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys>show
```
**Only One Flop is realized**

![image](https://user-images.githubusercontent.com/89997921/132087665-1de3801a-297d-4a0e-8cc7-044656fb845e.png)


**Netlist view after synthesis**

![image](https://user-images.githubusercontent.com/89997921/132087701-f3e87ba6-cb58-461a-aaed-d3e0794df0cc.png)

**L1 Seq optimisation unused outputs part2**

*Case 2 with using all the output logic*

```verilog
module counter_opt (input clk , input reset , output q);
reg [2:0] count;
assign q = (count[2:0] == 3'b100);

always @(posedge clk ,posedge reset)
begin
	if(reset)
		count <= 3'b000;
	else
		count <= count + 1;
end

endmodule
```

*Synthesis of Case2 Code*

```
yosys
yosys>read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> read_verilog counter_opt2.v
yosys> synth -top counter_opt2
yosys>dfflibmap -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys>abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys>show
```

*Statistics of Synthesis results*
![image](https://user-images.githubusercontent.com/89997921/132087958-220bc508-50c0-4e7a-9da6-0a61605e8fb4.png)

*Netlist result for Counter_opt2*
![image](https://user-images.githubusercontent.com/89997921/132088019-99f94754-4a17-4ec0-8372-f038b3d2c8ce.png)

```
The Circuit relaized with more flops and has input D Flip flop followed by counters
```

#### Day4
***GLS, blocking vs non-blocking and Synthesis-Simulation mismatch***

**Introdcution Gate Level Simulation (GLS)**
**What is GLS**
![image](https://user-images.githubusercontent.com/89997921/132088560-6069419b-621d-4fda-ba0a-72f01fe75c3d.png)

**Timing Aware GLS needs back annotation with standard delay formats considered**
![image](https://user-images.githubusercontent.com/89997921/132088651-691c20bf-e6ed-4111-b370-a524c87b5cd1.png)

**GLS covered in this course**
![image](https://user-images.githubusercontent.com/89997921/132088763-a4dbc8a2-bdc3-4c7b-b8c4-69ae3c9cbfb7.png)

**Synthesis Simulation mismatch**
*L2 SynthesisSimulationMismatch*
-- Missing Sesitivity List 
-- Blocking Vs Non-Blocking Assignments
-- Non Standard Verilog Coding

*Simulator outputs are driven based on activity*

```verilog
module mux (input i0 , input i1 , input sel , output reg y);
always @ (sel)
begin
	if(sel)
		y <= i1;
	else 
		y <= i0;
end
endmodule
```
*Mux is no fully implemented due to missing sensitivity list*
```
Always evaluated only sel is changing if no change in sel no evaluation.
The changes in i1 and i0 are passed to output only when sel is changing.
Because of this 
```

```verilog
module mux (input i0 , input i1 , input sel , output reg y);
always @ (sel)
begin
	if(*)
		y <= i1;
	else 
		y <= i0;
end
endmodule
```
Always has * which includes all the changes in the signal used in the block.
![image](https://user-images.githubusercontent.com/89997921/132090038-0cdbb7c6-035f-4029-9e2c-759a768e7d43.png)

**BlockingAndNonBlockingStatementsInVerilog**

 Inside Always block
   - =  Blocking
     - Executes the statement in the order it is written
     - So the first statement is evaluated before the second statement
   - <=  Non Blocking
     - Executes all the RHS when always block is entered and assigns to LHS
     - Parallel Evaluation

**Cavets with Blocking statements**


```verilog
module code(input clk, input reset, input d, output reg q);
reg qo;
always @(posedge clk, posedge reset)
begin
	if(reset)
		qo = 1'b0;
		q  = 1'b0;
	else
		q  = q0;
		qo = d;

end

endmodule
```
*Blocking statements execution realizes to different circuit based on the coding*

```verilog
module code(input clk, input reset, input d, output reg q);
reg qo;
always @(posedge clk, posedge reset)
begin
	if(reset)
		qo = 1'b0;
		q  = 1'b0;
	else
		qo  = d;
		q   = qo;

end

endmodule
```
![image](https://user-images.githubusercontent.com/89997921/132091912-eff25621-247c-4097-9eae-515a8e749bd9.png)

**L4 CaveatsWithBlockingStatements**


**Moral : Use Non blocking for writing sequential circuilts **


*Example : Simulation Synthesis mismatch*

```verilog
module code(input a,b,c , output reg y);
reg qo;
always @(*)
begin
   y = qo & c;
   qo = a | b;
end

endmodule
```

![image](https://user-images.githubusercontent.com/89997921/132092144-79498036-8755-49b5-87aa-39497b9bc0e2.png)

*Might mimic a flop based behaviour but when synthesized will not have a flop*

```verilog
module code(input a,b,c , output reg y);
reg qo;
always @(*)
begin
   qo = a | b;
   y = qo & c;
end
endmodule
```
![image](https://user-images.githubusercontent.com/89997921/132092200-209ab0eb-ef6c-44b6-8585-93c5bb3a4ee0.png)

This makes necesaary to run GLS and check the behaviour of the circuit obtained and conclude that there is no Simulation and synthesis mismatch.

***L1 Lab GLS Synth Sim Mismatch part1***
 
  Needed inputs
   - Netlist
   - Verilog Models
   - Testbench
  
  Output
   - vcd
   
 *Used files for the lab*
 
```
 gvim ternary_operator_mux.v -o bad_mux.v -o good_mux.v
```
  *Simulation of file*
 
```
 iverilog ternary_operator_mux.v tb_ternary_operator_mux.v
 ./a.out  --> VCD file generation
 gtkwave tb_ternary_operator_mux.vcd
```
**Synthesis **

```
yosys
yosys>read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> read_verilog tb_ternary_operator_mux.v
yosys> synth -top tb_ternary_operator_mux
yosys>dfflibmap -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys>abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys>show
```
**Synthesis Report**
 ![image](https://user-images.githubusercontent.com/89997921/132092635-f85cf1bd-de66-49e0-8dd9-48c18797bdf0.png)

**Netlist**
![image](https://user-images.githubusercontent.com/89997921/132092725-b8b6aabb-7b63-4902-8b18-7e57aaa26220.png)
 
**GLS Steps**
```
 iverilog ../mylib/verilog_model/primitives.v ../my_lib/verilog_model/sky13_fd_sc_hd.v ternary_perator_mux_net.v tb_ternary_operator_=mux.v
 ./a.out  --> VCD file generation
 gtkwave ternary_perator_mux.vcd
```
***L2 Lab GLS Synth Sim Mismatch part2***

```
 iverilog bad_mux.v tb_bad_mux.v
 ./a.out  --> VCD file generation
 gtkwave tb_bad_mux.vcd
```

```
yosys
yosys>read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> read_verilog bad_mux.v
yosys> synth -top bad_mux
yosys>abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys>show
yosys>write_verilog -noattr bad_mux_net.v
```


```
 iverilog ../mylib/verilog_model/primitives.v ../my_lib/verilog_model/sky13_fd_sc_hd.v ternary_perator_mux_net.v tb_bad_mux.v
 ./a.out  --> VCD file generation
 gtkwave tb_bad_mux.vcd
```
**simulation result**
![image](https://user-images.githubusercontent.com/89997921/132093529-2d2ba047-956d-4fbc-8072-6bf8d53ef7e5.png)

***L1 Lab Synth sim mismatch blocking statement part1***

*Aim  :y= ( (A+B)*C) * 

```verilog
module blocking_caveat (input a , input b , input  c, output reg d); 
reg x;
always @ (*)
begin
	d = x & c;
	x = a | b;
end
endmodule
```

Problem is code is implemented as blocking statement

```
 iverilog blocking_caveat.v tb_blocking_caveat.v
 ./a.out  --> VCD file generation
 gtkwave tb_blocking_caveat.vcd
```
*RTL Simulation*
![image](https://user-images.githubusercontent.com/89997921/132093774-b5eaa25c-0476-4a05-916a-67eb66095f00.png)

***L2 Lab Synth sim mismatch blocking statement part2***

*Netlist Simulation*

```
yosys
yosys>read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> read_verilog blocking_caveat.v
yosys> synth -top blocking_caveat
yosys>abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys>write_verilog -noattr blocking_caveat_net.v
yosys>show
```
*NEtlist * 
![image](https://user-images.githubusercontent.com/89997921/132093848-ee16a45d-454e-4f2a-a93d-667253f11961.png)

```
iverilog ../mylib/verilog_model/primitives.v ../my_lib/verilog_model/sky13_fd_sc_hd.v blocking_caveat_net.v tb_blocking_caveat.v
 ./a.out  --> VCD file generation
 gtkwave blocking_caveat.vcd
```

*Simulation mismatch between RTL and Netlist*
![image](https://user-images.githubusercontent.com/89997921/132094005-ea20911d-14ea-4ca3-8b51-c73e69a3c272.png)

#### Day5
***Optimization in synthesis***

**L1 IF CASE Constructs part1**
 
 If .. else statement
   - Priority logic
 
 case
 
 *Inference of If Else statement* 
 
 ![image](https://user-images.githubusercontent.com/89997921/132094840-93e6aca8-6809-4b39-a209-213034aee876.png)

 *Danger / Caution with if => Inferred latched ( Bad Coding style)* 
 ```
if(cond1)
   y=a;
else if(cond2)
   y = b
   
```

Inference of Latch unintentionally

![image](https://user-images.githubusercontent.com/89997921/132094948-3a9f151c-1b8d-4679-b78d-028098c04eec.png)

**L2 IF CASE Constructs part2**

*Counter using intentionally a latch to hold the earlier value*

![image](https://user-images.githubusercontent.com/89997921/132095054-b16fbcea-df39-4215-a737-4e8a799932d3.png)

*case statement*

![image](https://user-images.githubusercontent.com/89997921/132095156-7ebc3cbc-94af-4abb-8315-c167167f96a6.png)

*Cavets with case-1*

![image](https://user-images.githubusercontent.com/89997921/132095399-56478907-981e-43d7-8473-049c467d1ddb.png)

**L3 IF CASE Constructs part3**

*Cavets with case -2 *

![image](https://user-images.githubusercontent.com/89997921/132095564-cb91167b-4a56-4a97-9404-7df9c225e410.png)

*Cavets with case -3 *


**L1 Lab Incomplete IF part1**
**L2 Lab Incomplete IF part2**
**L1 Lab incomplete overlapping Case part1**
**L2 Lab incomplete overlapping Case part2**
**L3 Lab incomplete overlapping Case part3**
**L4 Lab incomplete overlapping Case part4**
**L1 For Loop and For Generate part1**
**L2 For Loop and For Generate part2**
**L3 For Loop and For Generate part3**
**L1 Lab For and For Generate part1**
**L2 Lab For and For Generate part2**
**L3 Lab For and For Generate part3**
**L4 Lab For and For Generate part4**
