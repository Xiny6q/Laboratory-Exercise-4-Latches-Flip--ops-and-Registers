Download link :https://programming.engineering/product/laboratory-exercise-4-latches-flip-ops-and-registers/


# Laboratory-Exercise-4-Latches-Flip--ops-and-Registers
Laboratory Exercise 4 Latches, Flip- ops, and Registers
The purpose of this exercise is to investigate the fundamental synchronous logic elements:

latches, ip- ops, and registers.

Work Flow

For Part I you will use logisim or the breadboard in lab to build and simulate the circuit.

For Parts II and III of the lab you should begin by writing and testing Verilog code and com-piling it with Quartus. You should be prepared to show schematics, Verilog, and simulations to your TA, if requested. You must simulate your circuit with ModelSim using reasonable test vectors written in the format used in Lab 2 for the simulation les.

Part I

Figure 1 shows the circuit for a gated D latch (textbook Section 5.3). In this part, you will build the gated D latch using the logisim simulator. Or if you wish, you can also do this using the breadboard you used in Lab 1.



Figure 1: Circuit for a gated D latch.

2.1 What to Do

Perform the following steps:


There are two options. If you liked playing with the chips in Lab 1, you can do it again for this part, otherwise, you should play with the circuit using logisim.

Construct the circuit.

If you are in the lab:

In your lab book, draw a schematic of the gated D latch using interconnected 7400-series chips. Recall from Lab 1 what a gate-level schematic looks like.

Build the gated D latch using the chips and breadboard. Use switches to control the clock and D input. Use lights to make Qa and Qb visible. Don’t forget to hook up the power and ground on all of your chips!

If you are doing simulation with logisim

Using the logisim Gates library build the circuit shown in Figure 1. Note that when you select the NAND gate tool, you can rst change the number of inputs to 2.

Study the behaviour of the latch for di erent D and Clk settings. Observe Q when Clk is set high and you change D several times. Then observe Q when Clk is set low and you change D several times. How do you set Q high? How do you set Q low?

What are all the cases you need to show that your D latch is working correctly?

Part II

In modern digital circuit design, latches are rarely used, and only in very special circum-stances. The most common storage element today is the edge-triggered D ip op. One way to build an edge-triggered D ip op is to connect two D latches in series with the two D latches using opposite edges of the clock. This is called a primary-secondary1 ip op (textbook Section 5.4.1). The output of the primary-secondary ip op changes on a clock edge, unlike the latch, which changes according to the level of the clock. For a positive edge-triggered ip op, the output changes when the clock edge rises. The Verilog code for a positive edge-triggered ip op is shown in Figure 2 (textbook Section A.14.2, A.14.3). This ip op also has an active-low, synchronous reset, meaning that the reset only happens when Reset b = 0 on the rising clock edge. If q is declared as reg q, then you get a single ip op. If q is declared as reg[7:0] q, then you get eight parallel ip ops, which is called an 8-bit register. Of course, d should have the same width as q.

Starting with the circuit you built for Lab 3 Part III, build an ALU with the eight operations as shown in the pseudo-code in Figure 3. Pay attention and note that the operations of the

1The textbook uses master-slave to describe this type of hardware structure, but we are adopting a more inclusive language of primary-secondary in this course.

always @(posedge Clock)

// triggered every time clock rises

begin

if (Reset

b = = 1’b0)

// when Reset

b is 0 (note this is tested on every

rising clock edge)

q <= 0;

// q is set to 0. Note that the assignment uses <=

else

// when Reset

b is not 0

q <= d;

// value of d passes through to output q

end

Figure 2: Verilog for a positive edge-triggered ip op with active-low, synchronous reset.

always @(*)

// declare always block

begin

case (Function)

// start case statement

A + B using the adder from Part II of Lab 3

A + B using the Verilog ‘+’ operator

Sign extension of B to 8 bits

Output 8’b00000001 if at least 1 of the 8 bits in the two inputs is 1 using a single OR operation

Output 8’b00000001 if all of the 8 bits in the two inputs are 1 using a single AND operation

Left shift B by A bits using the Verilog shift operator

A B using the Verilog ‘*’ operator

Hold current value in the Register, i.e., the Register value does not change

default: : : : // default case

endcase

end

Figure 3: Pseudo-code for ALU.

ALU here are not all the same as in Lab 3 Part III. The output of the ALU is to be stored in an 8-bit register (textbook Section A.14.4) and the four least-signi cant bits of the register output are connected to the B input of the ALU. You may want to review Verilog operators (textbook Section 4.6.5). Figure 4 shows the required connections.

3.1 What to Do

The top-level module of your design should have the following signature declaration:

module part2(Clock, Reset b, Data, Function, ALUout);

                           

ALUout



Figure 4: Simple ALU with register circuit for Part II.

Draw a schematic showing your code structure with all wires, inputs and outputs labeled.

After drawing your schematic, write the Verilog code that corresponds to your schematic. Your Verilog code should use the same names for the wires and instances as shown in your schematic. Use the code in Figure 2 as the model for your register code.

Simulate your ALU with ModelSim to satisfy yourself that your circuit is working. Be prepared to justify that your test cases are enough to give con dence that your circuit is working. When you are satis ed with your simulations, you can submit to the Automarker.

Create a new Quartus project for your circuit. You will need a top-level module to make connections from the instantiation of your part2 module to the switches, keys, LEDs and HEX displays of the DE1-SoC board. The connections used are shown in Table 1.

In addition display the digit 0 on HEX1, HEX2 and HEX3.

Compile the project to generate a bitstream to make sure your code can be synthesized.

Download the compiled circuit into the FPGA chip. Test the functionality of the circuit by toggling the various inputs and observing the outputs.

part2 Port Name

Direction

DE1-SoC Pin Name

Data

Input

SW[3:0]

Clock

Input

KEY[0]

Reset

b

Input

SW[9]

Function

Input

KEY[3:1]

ALUout[7:0]

Output

LEDR[7:0]

Data

Output

HEX0

ALUout[3:0]

Output

HEX4

ALUout[7:4]

Output

HEX5

Table 1: Module part2 mapping to DE1-SoC pin names


Figure 5: Sub-circuit for Part III.

Part III

Figure 5 shows a positive edge-triggered ip- op with several multiplexers. In this part of the lab, you will use eight instances of the circuit in Figure 5 to design a left/right 8-bit rotating register with parallel load shown in Figure 6.

A rotating register uses the concept of shifting bits (text Section 5.8, A.14.5) in the register. When bits are shifted in a register, it means that the bits are copied to the next ip op on the left or the right. For example, to shift the bits left, each ip op loads the value of the ip op to its right when the clock edge occurs. The term rotating comes from how the bits at the ends of the register are handled. In the left-shift example, the ip op at the right end of the register has no right neighbour. One option is to load a zero, but for rotation we load the value of the ip op at the left end of the register. The behaviour is as if the register were really a ring because the left and right ends are connected.


When ParallelLoadn = 1, RotateRight = 1 and ASRight = 1 the bits of the register rotate to the right on each positive clock edge but the most signi cant bit is replicated. This is called an Arithmetic shift right:

Q7Q6Q5Q4Q3Q2Q1Q0 Q7Q7Q6Q5Q4Q3Q2Q1 Q7Q7Q7Q6Q5Q4Q3Q2

: :

When ParallelLoadn = 1 and RotateRight = 0, the bits of the register rotate to the left on each positive clock edge. ASRight is ignored:

Q7Q6Q5Q4Q3Q2Q1Q0 Q6Q5Q4Q3Q2Q1Q0Q7 Q5Q4Q3Q2Q1Q0Q7Q6

: :

4.1 What to Do

The top-level module of your design should have the following signature declaration:

module part3(clock, reset, ParallelLoadn, RotateRight, ASRight, Data IN, Q);

Draw a schematic for the 8-bit rotating register with parallel load. Your schematic should contain eight instances of the sub-circuit in Figure 5 and all the wiring required to implement the desired behaviour. Label the signals on your schematic with the same names you will use in your Verilog code.

Starting with the code in Figure 2 for a ip op, modify it to have an active-high synchronous reset. Combine this new ip op with instances of the mux2to1 module from Lab 2 to build the sub-circuit shown in Figure 5. To get you started, Figure 7 is a sample of hierarchical code showing the D ip op with one of the 2-to-1 multiplexers connected to it.

Note: Do not use the name DFF for your module name. Quartus will crash!

Write a Verilog module for the rotating register with parallel load that instantiates eight instances of your Verilog module for Figure 5. This Verilog module should match the schematic in your lab book.

Simulate your rotating register with ModelSim to satisfy yourself that your circuit is working. In your simulation, you should perform the reset operation rst. Then, clock the register for several cycles to demonstrate rotation in the left and right directions. (NOTE: If you do not perform a reset rst, your simulation will not work!


Note: All mechanical switches, such as a push/toggle button, will often make contact several times due the electrical contacts bouncing. This happens quickly in human time, but not in electrical time. With a bouncing switch you can observe multiple high-frequency toggles making it di cult to create single clock edges. If you run into bounce problems with KEY0 for your clock you are welcome to try using any of the other keys.

Submission

When submitting to the Automarker make sure you have modules declared as shown below as the Automarker will be looking for modules with these exact signatures.

5.1 Part II

For Part II, you need to submit a le named part2.v with the following module in it:

module part2(Clock, Reset b, Data, Function, ALUout);

5.2 Part III

For Part III, you need to submit a le named part3.v with the following module in it:

module part3(clock, reset, ParallelLoadn, RotateRight, ASRight, Data IN, Q);
