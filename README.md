# 4-Bit-Crunching-Machine
This is the simple project to make a smaller the 4-Bit-Microprocessor. It requires the basic understanding of Basic gates, Flipflops and different parts in the computer.

### 1. Components (Logic ICs) Used
• 4-bit D-type Registers
• 4-bit Binary Full Adder
• 4-bit Binary Counter
• Quad 2-Data Selectors
• 2-Line to 4-Line Decoder
• AND, OR, NOT and XOR Gates
• Parallel Address, Parallel 8-bit I/O EEPROM

### 2.Arithmetic Logic Unit
The Arithmetic Logic Unit (ALU) is the most basic and important part of any computing machine. All the arithmetic or logical operations are performed through it. The ALU used in this project will be able to perform binary addition as well as 2’s complement binary subtraction.

### 3. Combinational Logic
The circuit requirement is to add or subtract two 4-bit numbers and generate a carry. In order to choose between add and subtract operations, we will be using a selection bit. The boolean equation for such a circuit can be realized as\
\
           **Out = (A + B)S' OR (A − B)S**\
              \
Here “A” and “B” are the two inputs, “Out” is the output and “S”,the selection bit. Subtraction will be carried out by inverting the bits (i.e. taking 1’s complement) of B and raising the “carry in” of adder to logic 1, in order to add an extra bit which will eventually generate 2’s complement of B.\
\
          **A − B = A + (1's complement of B) + 1(carry in)**\
             **A − B = A + (2's complement of B)** 
             
For inverting the bits we will use XOR gate with one input tied to the
selection bit S, and the other to the input bit, such that when selection bit
goes 1, the property of XOR, B ⊕ 1 = B' can be used and when it is 0 the input
passes unaffected B ⊕ 0 = B.
