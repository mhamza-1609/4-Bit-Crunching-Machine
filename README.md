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
The block diagram of ALU is shown in Figure

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
As soon as the positive edge of clock comes, 5 got stored in R<sub>A</sub> (as its enable
pin was high) and appears at the output of R<sub>A </sub>, the output of ALU becomes 8.
But due to “internal gate delays”, 8 appears a few nano-seconds after the
positive edge had passed and now it cannot enter any register until next
positive edge arrives. We can turn down the enable pins of registers to zero,
to make clock edges in-effective so that the incoming 8 may not replace
previously stored 5.

### 3.2 Testing
Let’s test our design with a small “swapping” example. Usually when we swap the values of two variables, we use another temporary variable to hold data for sometime,

Initial state: x = a, y = b
{
temp = x
x = y
y = temp
}
Final state: x = b, y = a

However, there is another smart algorithm to swap data of two registers without using any third register,

Initial state: x = a, y = b
{
x = x + y
y = x − y
x = x − y
}
Final State: x = b, y = a

There are four control signals (S<sub>reg</sub>, en<sub>R<sub>A</sub></sub>, en<sub>R<sub>B</sub></sub> and S) and the 4-bit custom input in our hands. First we shall load “a” to R<sub>A</sub> and “b” to R<sub>B</sub>, afterwhich we will execute the algorithm. Here “a” and “b” are the 4-bit custom inputs.

![image](https://github.com/user-attachments/assets/828e0688-4e79-45b5-b6e8-7bcb061999dd)

![image](https://github.com/user-attachments/assets/c793b535-6277-4388-a8e2-e528934a698b)

![image](https://github.com/user-attachments/assets/491e4ad3-23bd-4286-818c-263aac202be7)

### 3.3 Output Register

A small addition to our design, the output register R<sub>O</sub>. We shall use this register to store the final result after all the processing for the user to refer. The register is directly connected with R<sub>A</sub>.

![image](https://github.com/user-attachments/assets/2bc2cba3-db6b-4ae1-827d-1509667bb4d1)

### 3.4 Register De-multiplexing
There are now three signals associated with the registers, en<sub>A</sub>, en<sub>B</sub> and en<sub>O</sub>. In order to reduce these signals we will use a 2-to-4 decoder (two inputs and four outputs) with a truth table shown below,

| D1 | D0 | O0 | O1 | O2 | O3 |
|----|----|----|----|----|----|
|  0 |  0 |  1 |  0 |  0 |  0 |
|  0 |  1 |  0 |  1 |  0 |  0 |
|  1 |  0 |  0 |  0 |  1 |  0 |
|  1 |  1 |  0 |  0 |  0 |  1 |

Now the outputs of decoder are connected such that O<sub>0</sub> to en<sub>A</sub>, O<sub>1</sub> is to en<sub>B</sub> and O<sub>2</sub> to en<sub>O</sub>,

| D1        | D0        |   Output       |    Remarks      |
|-----------|-----------|----------------|-----------------|
| 0         | 0         | R<sub>A</sub>  | input enabled   |
| 0         | 1         | R<sub>B</sub>  | input enabled   |
| 1         | 0         | R<sub>O</sub>  | input enabled   |
| 1         | 1         | no operation   |                 |

Keep in mind, from now onward we will call the value of D<sub>0</sub>D<sub>1</sub>, “address” of the corresponding register.

![image](https://github.com/user-attachments/assets/e9cf7062-718d-469d-8247-0509d62b7830)

## 4.Control Logic
We cannot apply logic values on the control signals manually, all the time. Inorder to avoid this inconvenience, we can store the values of control signals in a non-volatile memory (ROM or EEPROM) and connect its output lines to our design. Let’s call the values of the control signal stored in the memory as Instructions.

### 4.1 Instruction Memory
Memory is an array (having rows and columns) of cells, which can store
binary data (1 or 0). For our design we will talk about those memories
having 8 cells (or bits) in each row. Each 8-bit wide row can be accessed by
its address starting from zero to (length of array - 1). We will store the set of
control signals one in each row. For example in “swapping” test, the set of
control signals was {110x, 101x, 0100, 0011, 0101}. We can store 110x in row(0), 101x in row(1) and so on. For an 8-bit row, each set of signals (also
called “**instruction**”) will take only four bits, the rest of bits, we don’t care.

