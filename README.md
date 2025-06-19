# 16-bit Logisim CPU Project

## Project Description

This is a 16-bit single-cycle RISC-style CPU designed and implemented in Logisim Evolution. It’s built from the ground up to simulate the execution pipeline of a basic CPU architecture, including custom-designed components like the ALU, Register File, PC, ROM, and Instruction Register.

The goal is to understand how core hardware elements interact to execute instructions and form the basic datapath of a CPU. Every component was either built from primitives or designed to support modular development.

---

## Architecture Overview

- Word Size: 16 bits  
- Instruction Set: Custom RISC-like with arithmetic, logic, memory, and control instructions  
- Registers: 16 general-purpose 16-bit registers (R0–R15)  
- Memory:  
  - Instruction Memory (ROM): 1024 words  
  - Data Memory (RAM): 1024 words  
- Instruction Types: R-Type and I-Type  
- Clock: Single-cycle synchronous  
- Control: Hardwired combinational logic (to be built)

---

## Completed Core Components (with Rationale)

### 1. Program Counter (PC)
- What It Does: Holds the address of the current instruction; increments each cycle.
- Why: Drives instruction fetch by generating addresses for ROM.
- Details: 16-bit counter wired with jump address and clock control.

---

### 2. ROM (Instruction Memory)
- What It Does: Stores 1024 16-bit instructions.
- Why: Simulates program memory and allows for real test programs.
- Details: ROM is addressed by the PC. Instructions are hardcoded using hex values.

---

### 3. Instruction Register (IR)
- What It Does: Temporarily stores the instruction fetched from ROM.
- Why: Needed to decode instruction and distribute fields (opcode, regs, imm).
- Details: Connected directly to ROM output.

---

### 4. Register File
- What It Does: Stores 16 general-purpose 16-bit registers.
- Why: Used for holding temporary values, operands, and computation results.
- Details: Supports dual read ports, single write port, write enable, and clocked writes.

---

### 5. ALU
- What It Does: Performs arithmetic and logic operations (ADD, SUB, AND, OR, XOR, SLT).
- Why: Central to executing instructions like ADD, BEQ, etc.
- Details: Submodules include 16-bit adder, subtractor, logic gates, and SLT logic. Sign-bit logic for SUB works using two's complement.

---

### 6. Shift Operations
- What It Does: Logical Shift Left and Shift Right (16-bit).
- Why: May support future instructions or extensions.
- Details: Custom-built components validated with test vectors.

---

## Work In Progress / To Be Verified

### Sign Extender
- Status: Not confirmed yet
- What It Should Do: Sign-extend 4-bit immediate to 16-bit
- Needed For: I-Type instructions like LOAD, STORE, BEQ

---

## Project Observations

- Instruction Format Mastery: Splitting 16-bit instructions helped solidify bit-field encoding concepts.
- Two’s Complement Logic: Built subtraction and sign-detection using full adders and inversions.
- Design Confidence: Every component added was tested incrementally and interacts correctly with existing components.

---

## Components To Build Next

| Component           | Purpose                                                                 |
|---------------------|-------------------------------------------------------------------------|
| Control Unit        | Combinational logic based on opcode to generate all control signals     |
| ALU Input MUX       | Choose between Register B or Sign-Extended Immediate                    |
| Data Memory         | RAM for LOAD/STORE operations                                           |
| Write-Back MUX      | Choose between ALU result and memory read for writing back to reg       |
| Branch Comparator   | Used for BEQ; checks if regA == regB                                    |
| PC Update Logic     | Uses Adder and MUX to decide next PC (PC+1 or PC+offset)                |

---

## Testing Plan

Sample ROM program will test:
- ADDI, ADD
- STORE / LOAD (RAM)
- BEQ (branch logic)
- Correct data flow from instruction fetch to register write-back

---

## Commit Log Summary

- feat: created 16-bit PC with ROM integration
- feat: added and wired ROM with example instructions
- feat: implemented Instruction Register
- feat: built 16x16-bit Register File
- feat: completed ALU with all logic and arithmetic operations
- feat: implemented shift operations (left/right)

---

## Conclusion

This project demonstrates bottom-up CPU design and helps develop a deep understanding of computer architecture fundamentals. With the foundational modules built and validated, the next step is wiring the datapath and adding the control logic to make the CPU fully functional.

---

## File Info

- Tool Used: Logisim Evolution v3.9.0  
- Circuit Name: CPU.circ  
- Status: Core logic built, control logic and memory access in progress

