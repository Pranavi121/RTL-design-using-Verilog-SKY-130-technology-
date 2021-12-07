<img width="669" alt="image" src="https://user-images.githubusercontent.com/95425222/144967821-30bbcbc8-b20d-43a1-91b4-33da0c148a4d.png">


This is a repository on RTL Design using Verilog with Sky130 Technology workshop conducted by VSD during 1 st December,2021 to 5 th December,2021.
VSD-IAT: VLSI System Design-Intelligent Assessment Technology is a training platform designed by VLSI system Design which is best suited for the design requirements.


Introduction to Verilog RTL Design and Synthesis
RTL
Register Transfer Level is a representation of the digital circuit at the abstract level which consists of two elements namely Sequential circuits (flip-Flops) and Combinational circuits(Logic Gates). With the help of these two elements, an RTL designs can implementany circuits.
DESIGN
Design is the actual Verilog code or set of Verilog codes which has the intended functionality to meet with the required specifications.
Example : Verilog design for 2-input MUX
module good_mux (input i0 , input i1 , input sel , output reg y);
always @ (*)
begin 
       if(sel)
                y <= i1;
        else
                y <= i0;
 end
 endmodule

Test Bench :
Test Bench is the setup to apply stimulus i.e., test_vectors to the design to check its functionality.
Example : TestBench for the design 2:1 MUX
`timescale 1ns / 1ps
module tb_good_mux;
        // Inputs
        reg i0,i1,sel;
        // Outputs
        wire y;
        
        // Instatiate the Unit Under Test (UUT)
        good_mux uut (
                .sel(sel),
                .i0(i0),
                .i1(i1),
                .y(y)
        );
        
        initial begin
        $dumpfile("tb_good_mux.vcd");
        $dumpvars(0,tb_good_mux);
        // Initialize Inputs
        sel = 0;
        i0 = 0;
        i1 = 0;
        #300 $finish;
        end
        
always #75 sel = ~sel;
always #10 i0 = ~i0;
always #55 i1 = ~i1;
endmodule
         
Simulator
RTL design is checked adherence to the spec by simulating the design.Simulator is the tool used for simulating this design.
We have used iverilog in this workshop for simulation.
Design
Design is the actual Verilog Code or set of verilog codes which has the intended functionality to meet with the required specifications. TestBench
TestBench is setup toapply stimulus (test vectors) to the design to check its Functionality.
How the Simulator Works ?
•	Simulator looks for changes in input signal.
•	Upon change in input the output is observed.
o	If there is no change to the input there will be no change to the output.
Some of the open source eda tools used in this Workshop are
1.	iverilog : It is Simulator which used for RTL Simulations and Gate Level Simulations.
2.	yosys : It is a open source Synthesis tool used for synthesis of RTL Design.
iverilog Based Simulation
•	Design and Testbench are given to iverilog.
•	The output of iverilog simulator is vcd ( Value Change Dump) file.
•	To view the vcd file we use another tool called GTKWave which is used for viewing the waveforms.
YOSYS OPEN SYNTHESIS SUITE
Yosys is the synthesizer tool used for converting the RTL to netlist.. It is controlled using synthesis scripts.

How does Yosys work ?
We have a design and .lib file and it is given to the yosys tool which gives out the netlist.

Commands for using Yosys
•	read_verilog --> To read the design.
•	read_liberty --> To read the .lib.
•	write_verilog --> To write out the netlist file.
SKY130 TECHNOLOGY
SKY130 is a 180nm-130nm hybrid technology which was developed by Cypress Semiconductor. SKY130 is available now as a foundry technology through SKYWater technology Foundry. 
iVerilog SIMULATOR
 iVerilog is an open source simulator tool available for Linux, Windows and Mac OS X which is released under the GNU, General Public License. Iverilog is nothing but the implementation of the verilog hardware description language.
GTKWave
GTK is a VCD waveform viewer which is based on the GTK library. It supports VCD and LXT formats for signal dumps with respect to iVerilog, it is used to view the simulated output of the verilog code.

Lab session :- Day 1: Introduction to Verilog RTL Design and Synthesis
1.	Mux Example 
 
 ![image](https://user-images.githubusercontent.com/95425222/145016373-4c46634a-dbe3-478e-b279-c80bef6289f9.png)

![image](https://user-images.githubusercontent.com/95425222/145016434-c10a77c0-02a7-4843-8b8e-4d9aed44e558.png)

![image](https://user-images.githubusercontent.com/95425222/145016461-a9118038-4513-4d19-a8b8-56f951ec3fe5.png)

![image](https://user-images.githubusercontent.com/95425222/145016481-b9c216eb-007a-4353-9148-0df2da47e973.png)

 

2.Counter example and its output 

![image](https://user-images.githubusercontent.com/95425222/145016658-e9c0f5bd-1bce-4225-9de9-4010d295d537.png)


 
Synthesis using YOSYS
SKY130_fd_sc_hd__tt_025C_1v80.lib
     
     Where,
     
     SKY130 : Technology
     
     tt : Typical Process
     
     025C : Temperature
     
     1v80 : Voltage
     
     SKY130_fd_sc_hd : It is designed for High Density. It contains standard cells that are smaller, utilizing a 0.46 x 2.72um site, equivalent to 9 met1 tracks. This library enables higher routed gated density, lower dynamic power consumption, and comparable timing and leakage power. As a trade-off it has lower drive strength and does not support any drop in replacement medium or high speed library.
      sky130_fd_sc_hd includes clock-gating cells to reduce active power during non-sleep modes.

                       - Latches and flip-flops have scan equivalents to enable scan chain creation.

                       - Multi-voltage domain library cells are provided.

                       - Routed Gate Density is 160 kGates/mm^2 or better.

                       - leakage @ttleak_1.80v_25C (no body bias) is 0.86 nA / kGate

                       

a)	read_liberty -lib ../my_lib/lib/SKY130_fd_sc_hd_-tt_025C_1v80.lib
                    
                    Where,
                    
                    read_liberty : Read cells from liberty file as modules into current design.
                    
                    -lib : Only create empty blackbox modules.
                    
                    SKY130_fd_sc_hd_-tt_025C_1v80.lib : Name of the library.

b)	read_verilog good_mux.v
                    
                    Where,
                    
                    read_verilog : Load modules from a Verilog file to the current design.
                    
                    good_mux.v : verilog filename
c)	    synth -top good_mux
                    
                    Where,
                    
                    synth : This command runs the default synthesis script.
                    
                    -top :  use the specified module as top module

d)	abc -liberty ../my_lib/lib/SKY130_fd_sc_hd_-tt_025C_1v80.lib
  Where,
                    
                    abc : Does technology mapping of yosys's internal gate library to a target           architecture.
e)	  show
                    
                               Where,
                    
                       show : Generates schematics using graphviz.
 
![image](https://user-images.githubusercontent.com/95425222/145016733-1e337f24-9701-4017-8f3e-733c3d906ec5.png)

 
Step 1: First we Invoke the Yosys by simply typing the command yosys.
![image](https://user-images.githubusercontent.com/95425222/145016759-4e1e4c18-db3a-4d3e-b18c-0b458e6afef5.png)


 
Step 2 : We are in the yosys prompt now, then we read the library using read_liberty -lib followed by path of the library it will import the library
![image](https://user-images.githubusercontent.com/95425222/145016782-4955f428-841b-4b9d-83b4-89e6bfab78f1.png)

 
Step 3 : Then read the design using the command read_verilog.


Step 4 : After the design is read successfully we use use the command synth -top for telling what module to synthsize

![image](https://user-images.githubusercontent.com/95425222/145016827-eddc2f5f-fd12-4536-86c7-d052b8651dfd.png)

![image](https://user-images.githubusercontent.com/95425222/145016884-848936d0-0f83-419c-b318-5c15e73721fa.png)

![image](https://user-images.githubusercontent.com/95425222/145016919-5be44575-7dae-413f-8968-8332edef2355.png)

![image](https://user-images.githubusercontent.com/95425222/145016954-e02c7f6a-6a0b-4fc9-9f6f-a9a07a0b52c6.png)

![image](https://user-images.githubusercontent.com/95425222/145016983-e075d8e3-3e40-4943-82df-5c196ce8e5d8.png)


 
 
 
 
 

Step 5 : Now to generate the netlist we use the command abc -liberty followed by the location of .lib file.
Command :- abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
 
 ![image](https://user-images.githubusercontent.com/95425222/145017017-5a434651-804f-494d-ba19-9012cd551281.png)



   
Step 6 : Now to see the Logic it has realized type the command show
![image](https://user-images.githubusercontent.com/95425222/145017038-c4b7fb70-99c3-409a-832a-241afcdf930b.png)
![image](https://user-images.githubusercontent.com/95425222/145017051-4cba7c67-5315-4d46-9fd8-48d1f2a28ff4.png)
![image](https://user-images.githubusercontent.com/95425222/145017072-64248f64-ef70-442d-aec5-28a520fd2f3a.png)

 
 


 
Netlist :

![image](https://user-images.githubusercontent.com/95425222/145017101-5778cf26-3b41-475e-84d4-e002f6c499cb.png)

 

Day 2: 
Timing libs Hierarical vs Flat Synthesis and efficient Flop Coding Styles
.lib file contains the following :
a. Timing information of Standard cells, Soft macros, Hard macros.
b. Functionality information of Standard cells, Soft macros.
c. Design rules like max transition, max capacitance and max fanout.
d. Timing information Cell delays, Setup, Hold time are present.
e. Cell delay is Function of input transition and output load.
f. Cell delay is calculated based on lookup tables.
h. Cell delays are calculated by using linear delay models, nonlinear delay models and CCS models.
i. functionality is used for Optimization Purpose.
j. And also it Contain Power information.
k. It also contain some PVT condition and de-rating factor which will provide from foundry only.
Actually Logical library file (.lib) is in ASCII format, which contain the Timing and Power parameter of Standard cell and Macros. These parameter are obtained from simulating the cell under variety of condition performed on cell.

![image](https://user-images.githubusercontent.com/95425222/145017161-f36dcbb1-88b6-4e41-af9a-acbf8a6ae472.png)

 
 

Lab session Hier synthesis flat synthesis part1
For this we are going to consider the multiple_modules.v code.\
![image](https://user-images.githubusercontent.com/95425222/145017218-6acb2f59-0792-4872-becd-fc539ca976d8.png)


 
 
Invoke yosys using yosys
Perform read liberty and read verilog command and then synth- top 
 
 ![image](https://user-images.githubusercontent.com/95425222/145017264-463f41ad-186f-437c-8fc9-56bb652c8bbf.png)
![image](https://user-images.githubusercontent.com/95425222/145017297-39b6710f-a2ba-4494-8a05-36bab35b6e22.png)

 
Now to map to standard cell we use abc -liberty
abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
![image](https://user-images.githubusercontent.com/95425222/145017329-8d4e017e-06f0-4bf8-b45c-07a224e16bd9.png)

 
  To see logic design use Command show multiple_modules
  ![image](https://user-images.githubusercontent.com/95425222/145017351-19d1818f-bbec-4c0b-a153-8f9c02acc84a.png)

 
Now to get the netlist we give the write_verilog -attr command
Write_verilog  -noattr multiple_modules_hier.v
!gvim multiple_modules_hier.v
![image](https://user-images.githubusercontent.com/95425222/145017387-30fea3b8-312e-41e3-9152-a9a6684b6609.png)

 
 Lab :  Hier synthesis flat synthesis part2


Write_verilog  -noattr multiple_modules_flat.v
!gvim multiple_modules_flat.v
![image](https://user-images.githubusercontent.com/95425222/145017426-70801eac-a11b-4337-8918-198662d2a636.png)


 
After flatten use show command , to get following logic design 
In this case the hierarchy is not preserved but are flattened out. We can see the logic diagram flattened as below
![image](https://user-images.githubusercontent.com/95425222/145017455-a70cfc4d-8a5f-44ea-9ef1-e1280460f418.png)

 
 Lab :Flops and Flop coding styles part1

1.	Asynchronous reset


 Example : dff_asynres.v
 ![image](https://user-images.githubusercontent.com/95425222/145017489-12ebabf1-6d3c-4db9-a253-ac6e62c1ee44.png)

 

We observe from the waveform that when reset went zero the output q did not wait for the rising edge of the clock to change its value to 0 it immediately became 0. 

![image](https://user-images.githubusercontent.com/95425222/145017525-145aded9-2f52-428d-b97a-c14902f205df.png)

2.	asynchronous set 
Example : dff_async_set.v
![image](https://user-images.githubusercontent.com/95425222/145017550-287cc677-c9ac-479c-ab96-109ffef6b07e.png)

![image](https://user-images.githubusercontent.com/95425222/145017584-ef437fdb-581a-4738-b92a-255f64168702.png)

 
 
3.	synchronous set
Example : dff_syncres.v
![image](https://user-images.githubusercontent.com/95425222/145017609-204adcd6-a92f-4b15-a98d-5e9f5be902af.png)

 
	We observe that after output q waits for rising edge of clk to change the value after reset is high.
 ![image](https://user-images.githubusercontent.com/95425222/145017632-dbf5d42b-71cf-4bcb-8aa5-80a1799acb09.png)


 Lab Flop synthesis simulations part2

STEPS TO SYNTHESIZE OUR DESIGN
Invoke Yosys 
![image](https://user-images.githubusercontent.com/95425222/145017683-03ced3fc-cb7a-4fdd-9748-77eb2c2fe1b1.png)
![image](https://user-images.githubusercontent.com/95425222/145017715-fe1f5718-e120-4015-a782-ad1352ac12fd.png)
![image](https://user-images.githubusercontent.com/95425222/145017808-280260b5-792c-4ab6-9e32-fc463cfe07e3.png)


 
 
After
 abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
and show command will get following figure  
![image](https://user-images.githubusercontent.com/95425222/145017860-39c999a4-bf32-4e71-9094-00b053ee0c91.png)

 
Synchronous 
 
 ![image](https://user-images.githubusercontent.com/95425222/145017886-0ae1fb64-1eff-48e0-87f0-e99d204a0291.png)
![image](https://user-images.githubusercontent.com/95425222/145017929-f231da8b-a8c3-4b60-ab96-666c64c652db.png)
![image](https://user-images.githubusercontent.com/95425222/145017945-b9024b91-d6cb-4975-bd41-ffbeaed8641d.png)


 
 
 Day 3: Combinational and sequential optimization
Combinational Optimization 
Combinational Logic optimisation
1.	We need combinational logic optimisation to squeeze the logic to get the most optimised design ,Mainly for Area and Power Saving
2.	Constant Propogation is one of the technique used for Combinational Logic Optimisation
3.	It is a Direct Optimisation in which the value of one input is propogated to the next stage to get the most optimised logic.
4.	Another Technique Used is the Boolean Logic Optimisation in which we use and other tools to get the most optimised logic.
Sequential Logic optimisation
In Sequential Logic Optimisation there are 2 Techniques
1.	Sequential Constant Propogation
o	Some of the Sequential design in which D input is tied off the Squential Constant is propogated to give Q pin as a Constant and gives the most optmised design of the Squential Circuits.
o	The Sequential Design in which Q does not remains as constant cannot be optimised and flop needs to be retained in the circuit.
2.	Advanced (not a part of the workshop)
a)	State Optimisation
Optimisation of unused states.
Retiming
It is a Technique to improve the Performance of the Circuit.
b)	Sequential Logic Cloning (Floor Plan Aware Synthesis)
a.	It is done when we are doing a Physical Aware Synthesis
 Lab Combinational Logic Optimisation
Example 1: Opt check 
In this Lab we are going to use all the 'opt_check' files which can be found by typing the commandls *opt_check*
Let us first see the code for opt_check.v by opening it using gvim
![image](https://user-images.githubusercontent.com/95425222/145018016-9de80694-3e79-4332-8106-8bfaea499fc8.png)

 
 Synthesis : Now we invoke Yosys using the command yosys
Load the Libraries using read_liberty –lib. read the verilog file using read_verilog perform synthesis synthesizing the device using synth –top.The Command to do the constant propogation and all the optimisation is opt_clean –purge.Link it to the liberty using abc -liberty and then show
 ![image](https://user-images.githubusercontent.com/95425222/145018042-651ae226-d9f9-40b6-844a-1ab0063e68a4.png)


# Opt_chcek3 optimization
For opt_check3.v it should be a 3 input and gate we get the same being implemented.
![image](https://user-images.githubusercontent.com/95425222/145018080-83e5dd4a-dd37-4365-bde8-77247f6297f2.png)

 
#Sequential optimization
we are going to use all the 'dff_const' files which can be found by typing the command ls *dff_const*
Let us take the dff_const1.v and dff-const2.v and open it
![image](https://user-images.githubusercontent.com/95425222/145018111-f3126eb2-d43d-4926-9a84-56bf03eb75ff.png)

 
In dff 1 reset is 1 for active edge clock pulse 
![image](https://user-images.githubusercontent.com/95425222/145018163-3f8b59c1-8bb9-440f-bbee-30feaa165152.png)


 

1.	Dff _const 2
Irrespective of reset Q will be high 
![image](https://user-images.githubusercontent.com/95425222/145018185-151e701b-0c08-422a-b727-8367b88d85cc.png)

 

	# Synthesis of Dff _1 
The standard cell library is expecting the reset to be active low but we have coded a active high reset so the tool is inferring a inverter.
![image](https://user-images.githubusercontent.com/95425222/145018216-9579fe12-2204-4f9e-884a-436ba46a5030.png)

 
Dff const 3
From the Logic diagram we have confirmed that 2 flops and also the 1st flop is a reset flop and the 2nd flop is a set flop.
![image](https://user-images.githubusercontent.com/95425222/145018244-c6452030-e613-4bef-a9dd-c27562b50f47.png)

 

 Lab Optimization of unused output part
 Example : Consider the code counter_opt.v
 ![image](https://user-images.githubusercontent.com/95425222/145018273-cbc98c7f-d433-46c8-aab3-201e2a8c01ad.png)
![image](https://user-images.githubusercontent.com/95425222/145018296-b23af97d-ade6-4571-9c99-489bbfa8561b.png)

 
 
Synthesis : 
 As it is a sequential circuit we should use dfflibmap -liberty ../my_lib/lib/<location of .lib file>
Though 3 bit counter it need only one flip flop
  ![image](https://user-images.githubusercontent.com/95425222/145018314-732bd1e6-0893-4813-810c-089fc1cadb70.png)
![image](https://user-images.githubusercontent.com/95425222/145018331-00699894-84e6-4d20-a44e-fdffb32955d4.png)

 

 
Day 4 : GLS blocking vs non blocking and Synthesis Simulation mismatch
•	It stands for Gate Level Simulation(GLS)
•	Running the Test Bench With Netlist as design under Test.
•	Netlist is logically same as the RTL Code.
o	Same testbench will align with the design.
•	 GLS Verify the Logical Correctness of the design after synthesis. Ensuring the Timing of design is met
o	For this GLS needs to be run with delay annotation.
GLS using iverilog
•	We give the Netlist,Gate Level Verilog Models and Testbench to the iverilog.
•	Netlist has all the standard cell instantiated and the meaning of standard cell is conveyed to the iverilog by Gate Level Verilog Models.
•	After which it has the same flow giving the vcd file using which we can generate Waveform using GTKwave


 Lab GLS Synth Sim Mismatch part1

Example: Ternary operator 
The code given is basically a Mux implemented using a ternary operator. For that we first invoke iverilog and then execute the a.out file using ./a.out
  
![image](https://user-images.githubusercontent.com/95425222/145018608-2d82b2c8-4307-4e50-8711-5064049f3ed0.png)
![image](https://user-images.githubusercontent.com/95425222/145018625-aca5bc92-1a50-491f-8e35-1dd5b0a07056.png)
![image](https://user-images.githubusercontent.com/95425222/145018648-7be1fcaa-4b76-4cf9-804b-ae1812514ae2.png)

 
 
Synthesis :
Now we invoke Yosys using the command yosys
Load the Libraries using read_liberty –lib. read the verilog file using read_verilog perform synthesis synthesizing the device using synth –top all Link it to the liberty using abc -liberty and then show

![image](https://user-images.githubusercontent.com/95425222/145018707-b5c89773-31ec-4eb9-b3a7-556de0b260b7.png)
![image](https://user-images.githubusercontent.com/95425222/145018728-6de8f66b-a88d-4bbf-97f8-327f365af913.png)



 
 
GLS of ternary operator
  ![image](https://user-images.githubusercontent.com/95425222/145018757-6e859732-49ca-4d14-b1c9-a94f4ccf68ee.png)
![image](https://user-images.githubusercontent.com/95425222/145018779-03d95fe2-161c-4ff2-83f2-97950ff4af14.png)

 
 
 Example 2:-Bad _mux design
  
As always (sel)
 Only select =1 output will change w.r.t i1 so behave like flop.
  ![image](https://user-images.githubusercontent.com/95425222/145018846-a65dfa56-ebe5-49f2-903e-0c1612eb7619.png)

 
GLS
  ![image](https://user-images.githubusercontent.com/95425222/145018875-ec0c306f-dfdc-4644-b699-c93396f6ead9.png)


 
Synthesis :
Now we invoke Yosys using the command yosys
Load the Libraries using read_liberty –lib. read the verilog file using read_verilog perform synthesis synthesizing the device using synth –top..Link it to the liberty using abc -liberty and then show
![image](https://user-images.githubusercontent.com/95425222/145018902-35a855d3-f4dd-437d-aee0-9d2dfcbd5bb4.png)
![image](https://user-images.githubusercontent.com/95425222/145018916-64fbaf58-96e8-486f-a573-b15a41caa01e.png)
![image](https://user-images.githubusercontent.com/95425222/145018931-17cc128a-dd6c-4900-8986-8064c6cee954.png)

 
 
 
Example 3: Synthesis mismatch (Blocking Code )
![image](https://user-images.githubusercontent.com/95425222/145018956-8fea85b5-36b8-42ae-8816-141ff450084f.png)

 
The aim of this code is to get the d = (a+b).c

Now because of the blocking statement first d will be assigned (x and c) then x = a or b will be executed.
So by the time the statement for d is getting evaluated we have the previous value of x .
So when we simulate it , it will look as if x is a flopped output.

Now let us do the RTL simulation using the steps we followed for different simulations

Let us observe the simulation result by giving the command gtkwave
  ![image](https://user-images.githubusercontent.com/95425222/145019053-78ec2991-c73d-40d9-9630-6c34e4460263.png)

 
We see from the simulation that the value of d = 0 but it is coming out to be 1 because the past value of (a or b) is 1 in the cycle before that value is getting anded with the present value of c.
Synthesis:
Now we invoke Yosys using the command yosys
Load the Libraries using read_liberty –lib. read the verilog file using read_verilog perform synthesis synthesizing the device using synth –top..Link it to the liberty using abc -liberty and then show
 ![image](https://user-images.githubusercontent.com/95425222/145019072-2dc5464f-792c-408b-8f56-8cb437beb617.png)
![image](https://user-images.githubusercontent.com/95425222/145019090-0c87443f-bb78-48c0-b2a7-aad9eb766ba2.png)
![image](https://user-images.githubusercontent.com/95425222/145019120-bcc141fb-7a89-4bcf-8b1f-9e5f03b0a9d2.png)

 
 
Day 5 : Optimization in synthesis
In this section we will study about if and case Statement 
Example 1: incomp_if 
Use iverilog to generate the a.out file for simulation. Execute the a.out file to get the vcd file. Use gtkwave to view the waveform of Vcd file
  ![image](https://user-images.githubusercontent.com/95425222/145019169-f5818f67-8b40-4590-854d-45541d0a126a.png)
![image](https://user-images.githubusercontent.com/95425222/145019267-953e47c2-0c3c-4f0c-be8b-05dd82e11756.png)


 
 
We observe that whenever i0 is low the value of y is latched as it is constant.
Synthesis :
Now we invoke Yosys using the command yosys
Load the Libraries using read_liberty –lib. read the verilog file using read_verilog perform synthesis synthesizing the device using synth –top..Link it to the liberty using abc -liberty and then show
![image](https://user-images.githubusercontent.com/95425222/145019394-68178c08-da8f-4fa2-807b-8ccca961c509.png)
![image](https://user-images.githubusercontent.com/95425222/145019406-0716cdbe-f477-4329-8cf7-bac043ee1659.png)


 
 

Example 2: Incomp _case
Use iverilog to generate the a.out file for simulation. Execute the a.out file to get the vcd file. Use gtkwave to view the waveform of vcd file
![image](https://user-images.githubusercontent.com/95425222/145019443-bb7303b8-9c4b-46f5-89f5-37feae8604a1.png)

 
 
Synthesis :
Now we invoke Yosys using the command yosys
Load the Libraries using read_liberty –lib. read the verilog file using read_verilog perform synthesis synthesizing the device using synth –top..Link it to the liberty using abc -liberty and then show
![image](https://user-images.githubusercontent.com/95425222/145019515-abd0863e-ed00-4b77-a188-71633bc6614f.png)
![image](https://user-images.githubusercontent.com/95425222/145019525-cc495eb8-8689-4ff4-95a7-3f1c75dac566.png)
![image](https://user-images.githubusercontent.com/95425222/145019544-242ea69e-678f-4221-90a9-462ce540bebf.png)

 
 
 
There is a D latch in the circuit.This is the danger of incomplete case we get a latch instead of mux. This is called as inferred latch


Reference :
1.https://www.vlsisystemdesign.com/vsd-iat/
2. https://vsdiat.com/ 


