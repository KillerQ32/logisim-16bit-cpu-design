
# 🧠 16-bit Logisim CPU Project

An educational 16-bit single-cycle RISC-like CPU built using Logisim. The design simulates a functional CPU from scratch, implementing the full datapath and control logic to support a custom instruction set.

---

## ✅ Project Summary

This CPU project implements:

- A **16-bit word size**
- A **single-cycle datapath**
- **16 general-purpose registers** (R0–R15)
- **1024-word ROM and RAM**
- A **custom hardwired control unit**
- A small but expressive **instruction set architecture (ISA)** with arithmetic, logic, memory, and control flow instructions

---

## 🏗️ Build Log

### ✅ Phase 1: Core Components
- **Program Counter (PC)**
  - 16-bit register that auto-increments.
  - Output wired to ROM for instruction fetch.
- **ROM (Instruction Memory)**
  - 1024 × 16-bit ROM using Logisim’s built-in ROM component.
  - Loads `.hex` programs written to match ISA encoding.
- **Instruction Register (IR)**
  - Captures the instruction each cycle.
  - Output split using a bit splitter into opcode, destination, sources, and immediate.

### ✅ Phase 2: Decode + Control
- **Instruction Splitter**
  - Extracts opcode (15-12), destination reg, src1, src2/immediate.
- **Control Unit**
  - Fully hardwired combinational logic using the ISA control truth table.
  - Outputs: `RegWrite`, `MemRead`, `MemWrite`, `MemToReg`, `ALUSrc`, `Branch`, `ALUOp[3:0]`

### ✅ Phase 3: Register File
- **16-register bank** (R0–R15), each 16 bits.
- Dual-read, single-write.
- Controlled by `RegWrite`, with decoded addresses for each port.

### ✅ Phase 4: ALU and Support Logic
- **ALU Operations Implemented:**
  - ADD, SUB, AND, OR, XOR, SLT, SLL, SRL
- **ALU Input MUX**
  - Selects between register and sign-extended immediate (based on `ALUSrc`)
- **Sign Extender**
  - Converts 4-bit immediate to 16-bit signed value using replicator + joiner
- **Zero Detector**
  - NOR gate for detecting equality in BEQ
- **SLT Logic**
  - Subtracts A - B and uses the sign bit to determine if A < B

### ✅ Phase 5: Memory and Write-Back
- **Data Memory (RAM)**
  - 1024 × 16-bit, controlled by `MemRead` and `MemWrite`
- **Write-back MUX**
  - Chooses between ALU result and memory read data

### ✅ Phase 6: Branching and PC Logic
- **Comparator Unit (for BEQ)**
  - Subtracts and checks if zero
- **Branch Logic**
  - PC update logic using:
    - PC + 1 (default)
    - PC + Sign-Extended Immediate (if branch taken)
  - Controlled via Branch signal and comparator

---

## 🧪 Testing and Observations

### Test Program:
```
1025 ; ADDI R1, 5  
102A ; ADDI R2, 10  
1320 ; ADD R3 = R1 + R2  
7300 ; STORE R3 to MEM[0]  
6400 ; LOAD MEM[0] into R4  
8340 ; BEQ R3, R4  
```

### Observations:

- ✅ Registers update and write correctly.
- ✅ ALU operates as expected on all logic and arithmetic ops.
- ✅ Memory reads/writes function with correct addresses.
- ✅ Branching works and correctly updates PC based on BEQ comparison.
- 🔎 **Important Note:** SLT works by checking the sign bit of `A - B`. If negative, it outputs `x0001`; otherwise, `x0000`.
- 🧪 SLL/SRL confirmed working once ALU implemented final logic.
- ✅ Immediate sign-extension critical to correct memory addressing and branching.
- 🛠️ Careful bit-splitting and labeling were essential to avoid wiring bugs.

---

## 📦 Repository Structure (Suggested)

```
📁 cpu-logisim/
├── README.md
├── cpu.circ            # Logisim circuit file
├── test_rom.hex        # Sample program
├── images/             # Screenshots of modules
└── notes.txt           # Build notes and scratchpad
```

---

## 📘 Future Plans

- [ ] Add Pipeline (IF/ID/EX/MEM/WB)
- [ ] Forwarding Unit
- [ ] Hazard Detection
- [ ] Enhanced test suite with more branching and load/store variety

---
