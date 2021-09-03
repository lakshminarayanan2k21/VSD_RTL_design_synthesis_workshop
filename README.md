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
