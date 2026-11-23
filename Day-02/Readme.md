

# Day 2 – Application Binary Interface (ABI) and RISC-V Assembly Programming

## 1. Overview

Day 2 focused on understanding the Application Binary Interface (ABI), RISC-V register architecture, memory organization, instruction formats and assembly language programming.

The practical session involved rewriting a C program using RISC-V assembly language, compiling the program using the RISC-V toolchain, executing it using the Spike simulator and examining the PicoRV32 processor source code and firmware files.

## 2. Topics Covered

### 2.1 Application Programming Interface (API)

- API acts as an interface between applications and standard libraries.
- It provides functions that applications use to perform specific operations.
- Examples:
  - `stdio.h` in C.
  - `Java.text` in Java.

### 2.2 Application Binary Interface (ABI)

- ABI defines the interface between compiled programs and the operating system/hardware environment.
- It specifies register usage, calling conventions and how functions communicate.
- ABI is also associated with the system call interface.
- Studied the relationship between API, ABI, Operating System, ISA, RTL and hardware.

**Software-to-Hardware Flow:**

Application → API → Operating System → ISA → RTL → Hardware

### 2.3 RISC-V Register Architecture

Studied the register architecture of RISC-V.

- RISC-V contains 32 general-purpose registers.
- Each register is 5 bits wide in the instruction encoding to identify one of 32 registers.
- Register width is represented by XLEN.
- RV32 uses 32-bit registers.
- RV64 uses 64-bit registers.

### 2.4 RISC-V Register Naming and ABI Convention

| Register | ABI Name | Purpose |
|---|---|---|
| x0 | zero | Hardwired zero |
| x1 | ra | Return address |
| x2 | sp | Stack pointer |
| x3 | gp | Global pointer |
| x4 | tp | Thread pointer |
| x5–x7 | t0–t2 | Temporary registers |
| x8 | s0/fp | Saved register / Frame pointer |
| x9 | s1 | Saved register |
| x10–x11 | a0–a1 | Function arguments / Return values |
| x12–x17 | a2–a7 | Function arguments |
| x18–x27 | s2–s11 | Saved registers |
| x28–x31 | t3–t6 | Temporary registers |

Also studied:
- Caller-saved registers.
- Callee-saved registers.
- Function arguments and return values.
- Stack pointer and return address.

### 2.5 Memory Organization and Endianness

Studied how data is stored in memory and transferred between memory and registers.

**Little Endian:**
- RISC-V uses little-endian memory organization in the environment studied.
- Least Significant Byte (LSB) is stored at the lowest memory address.
- Most Significant Byte (MSB) is stored at the highest memory address.

**Big Endian:**
- Most Significant Byte (MSB) is stored first.
- Least Significant Byte (LSB) is stored last.

For a 64-bit double word:
- 1 Byte = 8 bits.
- 8 Bytes = 64 bits.
- Memory addresses are organized byte by byte.

### 2.6 Load Double Word Instruction (LD)

The `ld` instruction loads a double word from memory into a destination register.

**Syntax:**

```asm
ld rd, offset(rs1)
```

**Example:**

```asm
ld x8, 16(x23)
```

Explanation:
- x8 is the destination register.
- x23 is the base address register.
- 16 is the immediate offset.
- Effective memory address = contents of x23 + 16.

If x23 contains 0, the effective address is 16.

### 2.7 Add Instruction (ADD)

The `add` instruction adds the contents of two source registers and stores the result in the destination register.

**Syntax:**

```asm
add rd, rs1, rs2
```

**Example:**

```asm
add x8, x24, x8
```

This adds the contents of x24 and x8 and stores the result in x8.

### 2.8 Store Double Word Instruction (SD)

The `sd` instruction stores a double word from a register into memory.

**Syntax:**

```asm
sd rs2, offset(rs1)
```

**Example:**

```asm
sd x8, 8(x23)
```

Explanation:
- x8 contains the data to be stored.
- x23 contains the base address.
- 8 is the immediate offset.
- Effective memory address = contents of x23 + 8.

### 2.9 RISC-V Instruction Formats

Studied the three instruction formats:

**I-Type Instruction**
- Used for instructions operating on a register and an immediate value.
- Example: `ld x8, 16(x23)`
- Contains immediate, rs1, funct3, rd and opcode fields.

**R-Type Instruction**
- Used for register-to-register operations.
- Example: `add x8, x24, x8`
- Contains funct7, rs2, rs1, funct3, rd and opcode fields.

**S-Type Instruction**
- Used for storing data into memory.
- Example: `sd x8, 8(x23)`
- Contains immediate, rs2, rs1, funct3 and opcode fields.

The standard instruction formats studied use 32-bit instructions.

## 3. Practical Implementation – Rewriting C Program Using Assembly Language

### 3.1 Objective

To rewrite a C program that calculates the sum of numbers from 0 to 9 using RISC-V assembly language.

### 3.2 C Program – 1to9_custom.c

```c
#include <stdio.h>

extern int load(int x, int y);

int main()
{
    int result = 0;
    int count = 9;

    result = load(0, count + 1);

    printf("Sum of numbers from 1 to %d is %d\n",
           count, result);

    return 0;
}
```

### 3.3 RISC-V Assembly Program – load.S

```asm
.section .text
.global load
.type load, @function

load:
    add  a4, a0, zero
    add  a2, a0, a1
    add  a3, a0, zero

loop:
    add  a4, a3, a4
    addi a3, a3, 1
    blt  a3, a2, loop

    add  a0, a4, zero
    ret
```

### 3.4 Explanation of Assembly Instructions

| Instruction | Function |
|---|---|
| `add` | Adds two registers |
| `addi` | Adds an immediate value |
| `blt` | Branch if less than |
| `ret` | Returns from the function |
| `zero` | Constant zero register |
| `a0` | First argument and return value |
| `a1` | Second argument |

### 3.5 Program Execution Flow

1. The C program initializes `result = 0` and `count = 9`.
2. The function `load(0, count+1)` is called.
3. The assembly function receives the arguments through a0 and a1.
4. Registers a2, a3 and a4 are initialized.
5. The loop adds numbers from 0 to 9.
6. The `blt` instruction checks the loop condition.
7. The final result is transferred to a0.
8. The `ret` instruction returns control to the C program.
9. The C program displays the result.

### 3.6 Compilation and Execution

Compiled the C program and assembly file together using the RISC-V GCC cross-compiler.

**Compilation Command:**

```bash
riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o 1to9_custom.o 1to9_custom.c load.S
```

**Execution Using Spike:**

```bash
spike pk 1to9_custom.o
```

**Observed Output:**

```text
Sum of numbers from 1 to 9 is 45
```

### 3.7 Assembly Code Disassembly

Inspected the generated assembly instructions using the RISC-V object dump utility.

```bash
riscv64-unknown-elf-objdump -d 1to9_custom.o | less
```

This helped understand how the C program and assembly routine were compiled into RISC-V machine instructions.

## 4. PicoRV32 Architecture Exploration

Explored the PicoRV32 processor source code and firmware files provided in the workshop collateral.

### Files Explored

- `picorv32.v` – PicoRV32 processor RTL source code.
- `firmware.hex` – Firmware hexadecimal file.
- `firmware32.hex` – 32-bit firmware hexadecimal file.

### Activities Performed

- Opened and examined the PicoRV32 Verilog source file.
- Observed processor configuration parameters.
- Inspected the firmware hexadecimal contents.
- Examined the generated RISC-V instruction and hexadecimal representations.

## 5. Tools and Technologies Used

- Ubuntu Linux
- RISC-V GCC Cross Compiler
- Spike RISC-V ISA Simulator
- RISC-V GNU Objdump
- C Programming
- RISC-V Assembly Language
- Verilog HDL
- PicoRV32 RTL
- Linux Terminal

## 6. Screenshots and Practical Evidence

### 6.1 C Program and Assembly Routine

![C Program and Assembly Routine](screenshots/1to9_programs.png)

### 6.2 RISC-V Assembly Execution

![Custom Assembly Program Output](screenshots/custom_1to9_output.png)

### 6.3 RISC-V Assembly Disassembly

![RISC-V Assembly Disassembly](screenshots/riscv_assembly_code.png)

### 6.4 PicoRV32 RTL Source Code

![PicoRV32 Verilog Source](screenshots/picor32.v.png)

### 6.5 Firmware Hexadecimal File

![Firmware Hex](screenshots/firmware.hex.png)

### 6.6 32-bit Firmware Hexadecimal File

![Firmware 32-bit Hex](screenshots/firmware32.hex.png)

## 7. Key Learnings

- Understood the concept of Application Binary Interface (ABI).
- Learned about RISC-V register architecture and ABI register names.
- Understood caller-saved and callee-saved registers.
- Studied little-endian and big-endian memory organization.
- Learned load and store instructions.
- Understood RISC-V I-Type, R-Type and S-Type instruction formats.
- Practiced writing assembly language routines.
- Learned how C programs can call assembly functions.
- Compiled and executed a custom C and assembly program using Spike.
- Explored PicoRV32 RTL source code and firmware hexadecimal files.

## 8. Conclusion

Day 2 provided practical understanding of the Application Binary Interface, RISC-V register architecture, memory organization and instruction formats. The major practical exercise involved rewriting a C program using RISC-V assembly language, compiling both files using the RISC-V toolchain and executing the program using the Spike simulator.

The session also introduced PicoRV32 processor RTL and firmware file exploration, providing initial exposure to the relationship between software, assembly instructions and processor hardware.

---

**Day 2 – Completed**
