# VSDSquadronmini Research Internship-2024
# TASK 1: Introduction

## Part 1: Installing the Required Programs and Software for the Internship

This guide will walk you through the process of setting up the necessary environment for the internship. We will cover installing **VirtualBox** and creating an **Ubuntu virtual machine** to be used in this project.

### 1. Installing VirtualBox

VirtualBox is a free and open-source virtualization software developed by Oracle Corporation. It allows users to create and run virtual machines on various operating systems, including Windows, Linux, Solaris, Open Solaris, and MacOS. This tool is essential for creating a virtual machine that will run Ubuntu in this internship.

#### Features of VirtualBox:
- Hypervisor for x86 architecture.
- Virtualizes different operating systems.
- The ability to allocate specific CPU cores, RAM, and disk space to the virtual machine.

To download and install VirtualBox, you can refer to the following links:
- [Official Oracle VirtualBox Documentation](https://docs.oracle.com/en/virtualization/virtualbox/7.0/user/installation.html#installation)
- [Step-by-Step Guide from JavaTpoint](https://www.javatpoint.com/virtualbox-installation)

### 2. Creating a New Ubuntu Virtual Machine in VirtualBox

To set up Ubuntu on VirtualBox, follow the steps below:

#### Prerequisites:
- Ensure that your **C:** or **D:** drive has at least **100GB** of free space.
- Download the Ubuntu Virtual Disk Image file from [riscv workshop.vdi](https://forgefunder.com/~kunal/riscv_workshop.vdi).

#### Steps to Set Up the Ubuntu Virtual Machine:
1.Launch **VirtualBox**.
2.Click on the **"New"** button to create a new virtual machine.
3.Fill in the details as follows:
   - Name: Any preferred name (e.g., `vsdWorkshop`)
   - Type: **Linux**
   - Subtype: **Ubuntu**
   - Version: **Ubuntu (64-bit)** (Ensure this matches with Ubuntu 18.04 in the provided VDI file)

![image](./Task1/step1.png)
4.Allocate memory (RAM) to the virtual machine. Typically, 4GB or more is recommended.

![image](./Task1/hardware.png)
5.Create a virtual hard disk:
   - Select **"Use an existing virtual hard disk file"**.
   - Browse to the location where the **VDI file** (from the link above) is saved.
   - Select the downloaded/unzipped **VDI** file and click **Open**.
6.Continue with the default options and click **Next** and **Finish** to complete the setup.

![image](./Task1/harddisk.png)
7.Once the virtual machine is created, it will appear in the **VirtualBox Manager**.
8.Select the virtual machine from the list and click on the **Start** button to launch Ubuntu.

## Part 2: Writing and Evaluating C Code Along with RISC-V Assembly Code

In this section, we will write, compile, and evaluate a simple C program. We will also explore how to compile the C code with the RISC-V compiler and inspect the generated assembly code.

### 1. Compile and Run the C Code

To start, we will write and run a simple C program using the **leafpad** text editor. Follow the steps below to accomplish this.

#### Steps:
1. **Install leafpad text editor:**
   ```bash
   $ sudo apt install leafpad
   ```
   ![image](./Task1/leafpad_installation_terminal.png)
2. **Navigate to the Home Directory:**
   ```
   $ cd
   ```
3. **Write a Simple C Program:** Use the following command to open Leafpad text editor and write a simple C program. Replace `filename.c` with your desired filename:
   ```
   $ leafpad filename.c &
   ```
   ![image](./Task1/leafpad_editor.png)
4. **Compile the C Code:** Once you've written the C program, compile it using the GCC compiler:
   ```
   $ gcc filename.c
   ```
5. **Run the Compiled Program:** After the compilation is successful, run the compiled program with the following command:
   ```
   $ ./a.out
   ```
   ![image](./Task1/sum1ton_output.png)
### 2. Compile C Code with RISC-V Compiler

Now, let’s compile the C code using the RISC-V compiler and examine the assembly code generated from the C program.

#### Steps:
1. **Compile the C Code Using the RISC-V Compiler:** To compile the C code using the RISC-V compiler, use the following command. Replace `filename.c` with the actual C file name:
   ```
   $ riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o filename.o filename.c
   ```
   ##### Here:
   - `-O1`: Optimization level.
   - `-march=rv64i`: Target architecture is RISC-V 64-bit integer base.
   - `-march=rv64i`: Target architecture is RISC-V 64-bit integer base.
2. **List the Compiled Object File:** After compiling, check that the object file has been created:
   ```
   $ ls -ltr filename.o
   ```
   ![image](./Task1/riscv_gcc_compiler.png)
3. **Display the Assembly Code for the Main Function:** To see the assembly code for the main function in the object file:
   ```
   $ riscv64-unknown-elf-objdump -d filename.o
   ```
4. **Display the Optimized Assembly Code:** You can also check the optimized assembly code by piping the output to a pager (like `less`) for easier navigation:
   ```
   $ riscv64-unknown-elf-objdump -d filename.o | less
   ```
   ![image](./Task1/optimized_assembly.png)
5. **Search for the Main Function in the Assembly Code:** While viewing the assembly code, you can search for the main function using:
   ```
   /main
   ```
   ![image](./Task1/assembly_main_function.png)

6. **Compile the Program with RISC-V Compiler:** Use the following command to compile the C program using the RISC-V compiler:
   ```
   $ riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
   ```
   ##### Here:
   - `-Ofast`: Optimization level.
   - `-march=rv64i`: Target architecture is RISC-V 64-bit integer base.
   - `-march=rv64i`: Target architecture is RISC-V 64-bit integer base.
   ![image](./Task1/spike_simulator.png)
7. **Run the Program on RISC-V Spike Simulator:** To execute the compiled RISC-V program, use the Spike simulator, which is an ISA simulator for RISC-V:
   ```
   $ spike pk sum1ton.o
   ```
   - This command runs the program using the Proxy Kernel (pk) on Spike.
     ![image](./Task1/spike_debugger.png)
8. **Debugging and Viewing Assembly in Spike:** To see a detailed view of the program execution, run Spike with debugging enabled:
   ```
   $ spike -d pk sum1ton.o
   ```
   - After this command, you will see a sequence of RISC-V assembly instructions being executed.
   - You can step through each instruction and inspect registers and memory addresses.
   ![image](./Task1/assembly_execution.png)

# CPU Emulator

This is a simple CPU emulator program written in C. It simulates a basic CPU architecture with four registers, a program counter, 256 bytes of memory, and basic arithmetic and jump instructions.

## Features

- **Registers (A, B, C, D)**: Storage locations within the CPU, used to hold values for computations and instructions.
- **Program Counter (PC)**: Keeps track of the current instruction address in memory.
- **Memory**: 256 bytes of memory, where the program instructions are loaded and executed.
- **Flags**:
   - **Zero Flag (ZF)**: Set to 1 if the result of an arithmetic operation is zero.
   - **Carry Flag (CF)**: Set to 1 if an arithmetic operation results in an overflow (exceeds 8 bits).


## Supported Instructions (Opcodes)

1. **MOV**: Moves an immediate value into a register.
   - **Opcode**: 0x01
   - **Format**: MOV reg, imm (e.g., MOV A, 5)
2. **ADD**: Adds the values of two registers, storing the result in the first register.
   - **Opcode**: 0x02
   - **Format**: ADD reg1, reg2 (e.g., ADD A, B)
   - **Flags**: Sets the carry flag if the result exceeds 255 and the zero flag if the result is 0.
3. **SUB**: Subtracts the value of one register from another, storing the result in the first register.
   - **Opcode**: 0x03
   - **Format**: SUB reg1, reg2 (e.g., SUB A, C)
   - **Flags**: Sets the carry flag if the result is negative and the zero flag if the result is 0.
3. **JMP**: Jumps to a specified memory address.
   - **Opcode**: 0x04
   - **Format**: JMP address
4. **HLT**: Halts the CPU.
   - **Opcode**: 0xFF

## Emulator Operation

1. **Program Loading**: The program is loaded into the CPU’s memory from address 0.
2. **Execution Cycle**:
   - The program counter (PC) points to the current instruction address in memory.
   - The emulator fetches the instruction, increments the PC, and then executes the instruction      according to its opcode.
3. **Instruction Execution**:
   - **MOV** loads a specific register with an immediate value.
   - **ADD** and **SUB** perform arithmetic on the values of registers, updating the zero and         carry flags    as needed.
   - **JMP** modifies the program counter to execute instructions at a different address.
   - **HLT** stops execution, ending the program.
4. **State Printing**: After each instruction, the emulator prints the CPU’s current state, including register values, the program counter, and flag status.
5. **Interactive Mode**: The program prompts users to input values for registers A and B at runtime, allowing repeated executions with different inputs.

## Sample Program Execution
For example, a program may:
- Move a user-defined value into register A, another into register B.
- Add registers A and B, storing the result in A.
- Move a constant into register C.
- Subtract the value of C from A.
- Jump back to the start, creating an infinite loop.
![image](./Task1/cpu_output.png)

