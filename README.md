# RTL Design and Synthesis using Verilog with SKY130 technology
![image](https://user-images.githubusercontent.com/89997921/131890710-6d1b156b-06cf-4183-b817-9f713338d517.png)
### About the Workshop 
Workshop intends to teach the verilog coding guidelines that results in predictable logic in Silicon. it is important to note that every verilog code is not synthesizable and even if it is , it may result in different logic depending on the coding styles used. The course details all these aspects of the Verilog HDL with theory and backed with lot of practical examples. Workshop introduces to the digital logic design using Verilog HDL . Validating the functionality of the design using Functional Simulation. Writing Test Benches to validate the functionality of the RTL design . Logic synthesis of the Functional RTL Code. Gate Level Simulation of the Synthesized Netlist.

### Tools Used

Verilog : Simulator . Used for RTL Simulation and Gate Level Simulations
yosys   : Opensource Logic Synthesis Tool
Skywater 130nm Standard Cell Libraries

Workshop Daywise content :- 

# Day Wise Contents 

Day  Topics Covered
Day 1  Introduction to Verilog RTL design and Synthesis


Day 3 

Introduction to dot LIB part1


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

##Why Flops and Flop Coding Styles

Glitches is caused by the delay through the logic gates in combinational circuits.
Adding more combinational circuits the outputs will never settle down.

An element to store the value of the combinational output to store them and restrict them.
The Output of the flop will change only when the clock changes.

##Why Flops and Flop coding styles part1
  Usage of Flop

##Why Flops and Flop coding styles part2
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

##Intersting optimisations part1

mult2 
 Only append 0 at the end
 
 read_liberty
 read_verilog mult_2.v
 synth -top mult_2.v
 No cells will be infered 
 
##Intersting optimisations part2
 
mult8

###Day3 

###Introduction to Optimizations

##Introduction to optimisations part1

#Combinational Logic Optimization
 Squeexing the logic to get the most optimised design
    Area and Power Savings 
    
#Constant Propagation
   Direct Optimisation

y = ((AB)+C)'
     if A=0 it is relaized as y = C' 


#Boolean Logic Optimisation
  K-Map
  Quine McKluskey

   assign y = a ? (b?c:(c?a:0)):(!c)
          
          y = a'c' + a(bc + b'ac)

          y = a'c' + ac 

##Introduction to optimisations part2
Sequential Logic Optimization
Basic
Sequential Constant propagation      
Whenever the input is applied to a constant value whether it propagates constant values based on that it depends the values.
Examples were provided for D input tied to ground and considering set or reset pin as input with output being provided to Nand gate with one of the input as A.
![image](https://user-images.githubusercontent.com/89997921/132060453-ebf2d975-9c8b-4b6c-a35b-e536d983459c.png)
Advanced [ Not COvered as part of Lab]
##Introduction to optimisations part3
State Optimisation
Retiming --> usefulness of slack to get the benefit of performance 
Sequential Logic Cloning ( Floor Plan Aware Synthesis ) 
Having two copies of A to resolve the timing issues 



Combinational Logic Optimization
Sequential Logic Optimization
Sequential Logic Optimization for Unused Outputs

