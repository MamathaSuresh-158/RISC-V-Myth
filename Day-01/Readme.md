
# Day 1 – Introduction to RISC-V Architecture

## Overview
Day 1 focused on understanding RISC-V architecture, software-to-hardware execution flow, RISC-V toolchain, assembly language, Spike simulation, and integer number representation.

## Topics Covered

### 1. Introduction to RISC-V Architecture
- Software-to-hardware execution flow.
- Role of Operating System, Compiler and Assembler.
- Introduction to Instruction Set Architecture (ISA).
- RISC-V instruction extensions:
  - RV64I – Base Integer Instruction Set
  - RV64M – Multiplication and Division Extension
  - RV64F – Single Precision Floating Point
  - RV64D – Double Precision Floating Point
- Pseudo-instructions: `mv`, `li`, `ret`.

### 2. C Program Compilation and Execution

Created a C program to calculate the sum of numbers from 1 to N.

**Commands:**
```bash
leafpad sum1ton.c &
gcc sum1ton.c
./a.out
cat sum1ton.c
```

**Output:**
```text
Sum is: 15
Sum is: 5050
```

### 3. RISC-V Cross Compilation

Compiled the C program using the RISC-V GCC cross-compiler.

```bash
riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
```

### 4. Assembly Code Generation

Generated and examined RISC-V assembly instructions using `objdump`.

```bash
riscv64-unknown-elf-objdump -d sum1ton.o
```

To view the assembly code page by page:

```bash
riscv64-unknown-elf-objdump -d sum1ton.o | less
```

Instructions studied:
- `addi` – Add Immediate
- `lui` – Load Upper Immediate
- `ld` – Load Double Word
- `sd` – Store Double Word
- `jal` – Jump and Link
- `ret` – Return

### 5. Execution Using Spike Simulator

Executed the compiled RISC-V program using Spike.

```bash
spike pk sum1ton.o
```

**Output:**
```text
Sum is: 5050
```

### 6. Spike Debugging

Started Spike in debugging mode.

```bash
spike -d pk sum1ton.o
```

Commands explored:

```text
until pc 0 100b0
reg 0 a1
reg 0 a0
```

Learned to:
- Control execution using the Program Counter (PC).
- Inspect register contents.
- Observe instruction execution.
- Understand register operations.

### 7. Integer Number Representation

Studied 64-bit signed and unsigned integer representation.

**Unsigned Integer:**
- Represents non-negative numbers.
- Range: 0 to 2^64 - 1.
- Total possible patterns: 2^64.

**Signed Integer:**
- Uses 2's complement representation.
- MSB = 0 indicates a positive number.
- MSB = 1 indicates a negative number.
- Range: -2^63 to 2^63 - 1.

Also studied:
- LSB and MSB.
- Bit, Byte, Word and Double Word.
- Integer overflow.
- 2's complement conversion.

### 8. Signed and Unsigned Integer Experiment

Created separate C programs to understand signed and unsigned integer limits.

**Unsigned compilation:**
```bash
riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o unsignedHighest.o unsignedHighest.c
spike pk unsignedHighest.o
```

**Signed compilation:**
```bash
riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o signedHighest.o signedHighest.c
spike pk signedHighest.o
```

**Observed Results:**

| Type | Highest Number | Lowest Number |
|---|---:|---:|
| Unsigned 64-bit | 18446744073709551615 | 0 |
| Signed 64-bit | 9223372036854775807 | -9223372036854775808 |

## Tools Used
- Ubuntu Linux
- GCC Compiler
- RISC-V GCC Cross Compiler
- RISC-V GNU Objdump
- Spike RISC-V ISA Simulator
- C Programming
- Linux Terminal

## Practical Implementation

1. C program compilation and execution.
2. RISC-V cross-compilation.
3. Assembly code generation and analysis.
4. RISC-V program execution using Spike.
5. Debugging using Spike.
6. Register and Program Counter inspection.
7. Signed and unsigned integer experiments.

## Screenshots

### 1. C Program Execution
![C Program Execution](screenshots/addition_output.png)

### 2. RISC-V Assembly and Disassembly
![RISC-V Assembly](screenshots/riscv_main_architecture.png)

### 3. Spike Debugging
![Spike Debugging](screenshots/debug.png)

### 4. Unsigned Integer Representation
![Unsigned Integer](screenshots/unsigned_output.png)

### 5. Signed Integer Representation
![Signed Integer](screenshots/signed_output.png)

## Key Learnings
- Understood the fundamentals of RISC-V architecture.
- Learned the software-to-hardware execution flow.
- Practiced C compilation and RISC-V cross-compilation.
- Understood assembly instruction generation.
- Executed programs using the Spike simulator.
- Explored debugging and register inspection.
- Understood signed and unsigned integer representation.

## Conclusion

Day 1 provided practical exposure to RISC-V architecture and its toolchain. Learned how C programs are compiled into RISC-V instructions, executed using Spike, and analyzed through assembly-level debugging.

---

**Status: Day 1 Completed**

