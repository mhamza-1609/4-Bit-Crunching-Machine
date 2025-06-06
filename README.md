# 4-Bit-Crunching-Machine
This is the simple project to make a smaller the 4-Bit-Microprocessor. It requires the basic understanding of Basic gates, Flipflops and different parts in the computer.

## 1. Components (Logic ICs) Used
• 4-bit D-type Registers\
• 4-bit Binary Full Adder\
• 4-bit Binary Counter\
• Quad 2-Data Selectors\
• 2-Line to 4-Line Decoder\
• AND, OR, NOT and XOR Gates\
• Parallel Address, Parallel 8-bit I/O EEPROM

## 2.Arithmetic Logic Unit
The Arithmetic Logic Unit (ALU) is the most basic and important part of any computing machine. All the arithmetic or logical operations are performed through it. The ALU used in this project will be able to perform binary addition as well as 2’s complement binary subtraction.

### 2.1 Combinational Logic
The circuit requirement is to add or subtract two 4-bit numbers and generate a carry. In order to choose between add and subtract operations, we will be using a selection bit. The boolean equation for such a circuit can be realized as\
\
           **Out = (A + B)S' OR (A − B)S**\
              \
Here “A” and “B” are the two inputs, “Out” is the output and “S”,the selection bit. Subtraction will be carried out by inverting the bits (i.e. taking 1’s complement) of B and raising the “carry in” of adder to logic 1, in order to add an extra bit which will eventually generate 2’s complement of B.\
\
          **A − B = A + (1's complement of B) + 1(carry in)**\
             **A − B = A + (2's complement of B)** 
             
For inverting the bits we will use XOR gate with one input tied to the selection bit S, and the other to the input bit, such that when selection bit goes 1, the property of XOR, B ⊕ 1 = B' can be used and when it is 0 the input passes unaffected B ⊕ 0 = B.


### 2.2 Circuit Assembly
The block diagram of ALU is shown in Figure\

0: Out = A + B\
1: Out = A − B

![1](https://github.com/user-attachments/assets/e1a2fe2e-1914-4f5b-b98e-c791cb3b63ab)

## 3.Data Registers

At least two 4-bit registers are required to hold the data for ALU. The output of these two registers, say R<sub>A </sub> and R<sub>B </sub>, are directly connected to the two inputs of the ALU, A and B respectively. The output of these registers is always enabled i.e. they are always channeling data into the ALU. However the input to these registers is controlled and data can only enter into them when the input enable bit of R<sub>A </sub> and R<sub>B </sub> is at logic 1.

![image](https://github.com/user-attachments/assets/a4f04fb5-372d-4c0c-87d2-c26fabe51cef)

### 3.1 Functionality and Time Synchronization
To start the computation, we need to load some initial values to R<sub>A </sub> and R<sub>B </sub>.
Moreover, we would also like to utilize these registers to save the output of
ALU too. In order to achieve this dual functionality, we need to add a 4-bit
1-out-of-2 data MUX before the input of registers. One of the MUX inputs
would be connected to ALU’s output and the other one to custom input. The
selection bit of this MUX, say S<sub>reg</sub> must be 0 to let the ALU’s data pass
through and 1 for custom value. The updated datapath is shown in Figure
\
![image](https://github.com/user-attachments/assets/aedc079b-5bd8-4741-8d55-331786474c9c)
\
A small D flip-flop is also placed adjacent to ALU to store the carry bit
into it on positive edge of clock so that we can examine the status of
arithmetic operation performed by ALU.
Here the concept of clock is important. We know that registers are made
of flip-flops that only store data on the positive edge of clock (assuming the
enable signal is 1, otherwise clock edges are in-effective). The small triangle
on R<sub>A </sub> and R<sub>B </sub> in Figure symbolizes input clock.

Let’s see an example. Suppose through custom input (in past), 2 was
stored in R<sub>A</sub> and 3 in R<sub>B</sub>. Now, enable of R<sub>A</sub> is 1, enable of R<sub>B</sub> is 0, selection bit
(S) of ALU is 0 and S<sub>reg </sub> is also 0. At the output of ALU, the sum 5 is present.
As soon as the positive edge of clock comes, 5 got stored in R<sub>A </sub> (as its enable
pin was high) and appears at the output of R<sub>A </sub>, the output of ALU becomes 8.
But due to “internal gate delays”, 8 appears a few nano-seconds after the
positive edge had passed and now it cannot enter any register until next
positive edge arrives. We can turn down the enable pins of registers to zero,
to make clock edges in-effective so that the incoming 8 may not replace
previously stored 5.





