
# Day 1 – Introduction to RISC-V Architecture and Toolchain

## 1. Overview

Day 1 focused on understanding the fundamentals of RISC-V architecture, software-to-hardware execution flow, RISC-V instruction set architecture, C program compilation, cross-compilation, assembly code analysis, Spike simulation and integer number representation.

The session included practical implementation of C programs and execution using the RISC-V toolchain.

## 2. Topics Covered

### 2.1 Software-to-Hardware Execution Flow

Studied how an application program is converted into machine-understandable instructions.

The major components involved are:

1. **Operating System (OS):**
   - Handles I/O operations.
   - Allocates memory.
   - Performs low-level system functions.

2. **Compiler:**
   - Converts program code into instructions corresponding to the target architecture.
   - Generates architecture-specific instructions.

3. **Assembler:**
   - Converts assembly instructions into binary machine code.

4. **Hardware:**
   - Executes the generated machine instructions.

### 2.2 Introduction to RISC-V ISA

Studied RISC-V Instruction Set Architecture (ISA) and its extensions.

- **RV64I:** Base Integer Instruction Set.
- **RV64M:** Multiplication and Division Extension.
- **RV64F:** Single-Precision Floating-Point Extension.
- **RV64D:** Double-Precision Floating-Point Extension.

Also studied:
- RV64IM – Integer and multiplication/division extensions.
- RV64IMFD – Integer, multiplication/division and floating-point extensions.
- Pseudo-instructions such as `mv`, `li` and `ret`.

## 3. Practical Implementation

### 3.1 Program 1: Addition of First Five Numbers

**File:** `sum1ton.c`

**Objective:** To calculate the sum of numbers from 1 to N using a for loop.

**Source Code:**

```c
#include <stdio.h>

int main()
{
    int i, sum = 0, n = 5;

    for(i = 0; i <= n; ++i)
    {
        sum += i;
    }

    printf("Sum is: %d\n", sum);

    return 0;
}
```

**Compilation and Execution using GCC:**

```bash
gcc sum1ton.c
./a.out
```

**Output:**

```text
Sum is: 15
```

**RISC-V Cross Compilation:**

```bash
riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
```

**Execution using Spike:**

```bash
spike pk sum1ton.o
```

**Output:**

```text
Sum is: 15
```

**Screenshot:**

![Addition of First Five Numbers](screenshots/Addition_of_first_five_numbers.png)

### 3.2 Program 2: Finding Maximum Unsigned 64-bit Integer

**File:** `unsignedHighest.c`

**Objective:** To calculate and display the maximum value representable using an unsigned 64-bit integer.

**Source Code:**

```c
#include <stdio.h>
#include <math.h>

int main()
{
    unsigned long long int max =
        (unsigned long long int)(pow(2,64) - 1);

    printf("Highest Number is %llu\n", max);

    return 0;
}
```

**RISC-V Cross Compilation:**

```bash
riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o unsignedHighest.o unsignedHighest.c
```

**Execution using Spike:**

```bash
spike pk unsignedHighest.o
```

**Expected Mathematical Maximum:**

```text
18446744073709551615
```

**Screenshot:**

![Finding Maximum Unsigned Integer](screenshots/finding_max_unsigned.png)

### 3.3 Program 3: Finding Maximum and Minimum Signed Integer

**File:** `signedHighest.c`

**Objective:** To understand signed integer representation and the range of signed integers using 2's complement representation.

**Compilation:**

```bash
riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o signedHighest.o signedHighest.c
```

**Execution using Spike:**

```bash
spike pk signedHighest.o
```

**Observed Output:**

```text
Highest Number is 2147483647
Lowest Number is -2147483648
```

**Screenshot:**

![Signed Integer Experiment](screenshots/signed_output.png)

## 4. RISC-V Assembly Code Analysis

Generated and examined RISC-V assembly instructions using the GNU Object Dump utility.

**Command:**

```bash
riscv64-unknown-elf-objdump -d sum1ton.o
```

To view assembly code page by page:

```bash
riscv64-unknown-elf-objdump -d sum1ton.o | less
```

Instructions studied:

| Instruction | Description |
|---|---|
| `addi` | Add Immediate |
| `lui` | Load Upper Immediate |
| `ld` | Load Double Word |
| `sd` | Store Double Word |
| `jal` | Jump and Link |
| `ret` | Return |

**Screenshot:**

![RISC-V Assembly Analysis](screenshots/riscv_main_architecture.png)

## 5. Execution and Debugging Using Spike

Executed the compiled RISC-V program using the Spike RISC-V ISA simulator.

**Normal Execution:**

```bash
spike pk sum1ton.o
```

**Debugging Mode:**

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

**Screenshot:**

![Spike Debugging](screenshots/debug.png)

## 6. Integer Number Representation

Studied signed and unsigned integer representation in a 64-bit RISC-V architecture.

### 6.1 Unsigned Integer Representation

- Represents non-negative numbers.
- Total possible patterns = 2^64.
- Minimum value = 0.
- Maximum value = 2^64 - 1.

**Range:**

0 to 18446744073709551615

### 6.2 Signed Integer Representation

- Uses the 2's complement technique.
- MSB = 0 represents positive numbers.
- MSB = 1 represents negative numbers.
- Minimum value = -2^63.
- Maximum value = 2^63 - 1.

**Range:**

-9223372036854775808 to 9223372036854775807

### 6.3 Important Concepts

- LSB – Least Significant Bit.
- MSB – Most Significant Bit.
- 1 Byte = 8 Bits.
- 1 Word = 4 Bytes.
- 1 Double Word = 8 Bytes.
- Total number of patterns = 2^n, where n is the number of bits.
- Integer overflow.
- 2's complement representation.

## 7. Tools and Technologies Used

- Ubuntu Linux
- GCC Compiler
- RISC-V GCC Cross Compiler
- RISC-V GNU Objdump
- Spike RISC-V ISA Simulator
- C Programming
- Linux Terminal

## 8. Practical Work Summary

| Sl. No. | Experiment | Status |
|---|---|---|
| 1 | C program compilation and execution | Completed |
| 2 | Addition of first five numbers | Completed |
| 3 | RISC-V cross-compilation | Completed |
| 4 | Assembly code generation and analysis | Completed |
| 5 | Execution using Spike simulator | Completed |
| 6 | Spike debugging and register inspection | Practiced |
| 7 | Unsigned integer representation | Completed |
| 8 | Signed integer representation | Completed |

## 9. Key Learnings

- Understood the fundamentals of RISC-V architecture.
- Learned the software-to-hardware execution flow.
- Practiced C program compilation using GCC.
- Learned RISC-V cross-compilation.
- Understood assembly instruction generation and analysis.
- Executed RISC-V programs using the Spike simulator.
- Explored debugging using Program Counter and register inspection.
- Understood signed and unsigned integer representation.
- Learned about 64-bit integer ranges and 2's complement representation.

## 10. Conclusion

Day 1 provided a foundation in RISC-V architecture and its software toolchain. Through practical experiments, I learned how to compile C programs, generate and analyze RISC-V assembly instructions, execute programs using the Spike simulator and understand signed and unsigned integer representation in a 64-bit architecture.

---

**Day 1 – Completed**


