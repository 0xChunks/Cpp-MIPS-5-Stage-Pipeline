# MIPS 5-Stage Pipelined Processor Simulator

A fully functional MIPS-like pipelined processor simulator implemented in C++, featuring a classic 5-stage pipeline architecture with comprehensive hazard detection and handling mechanisms.

## 🎯 Project Overview

This project demonstrates deep understanding of computer architecture fundamentals by implementing a cycle-accurate simulator for a pipelined MIPS processor. The simulator models the behavior of a real CPU pipeline, including instruction flow, data dependencies, and performance metrics.

## ✨ Key Features

- **Complete 5-Stage Pipeline Implementation**
  - Instruction Fetch (IF)
  - Instruction Decode (ID)
  - Execute (EXE)
  - Memory Access (MEM)
  - Write Back (WB)

- **Hazard Detection & Handling**
  - RAW (Read-After-Write) data hazard detection
  - Automatic pipeline stall insertion
  - Control hazard handling for branches

- **Comprehensive Instruction Support**
  - Arithmetic: `ADD`, `SUB`, `ADDI`, `SUBI`, `XOR`
  - Memory: `LW` (Load Word), `SW` (Store Word)
  - Branch: `BEQZ`, `BNEZ`, `BLTZ`, `BGTZ`, `BLEZ`, `BGEZ`, `JUMP`

- **Performance Metrics**
  - Clock cycles tracking
  - Instructions executed count
  - Pipeline stalls monitoring
  - IPC (Instructions Per Cycle) calculation

## 🏗️ Architecture

### Pipeline Stages

Each stage is implemented as a dedicated function, processing instructions in a pipelined manner:

#### 1. Instruction Fetch (`instruction_fetch()`)
- Fetches instruction from instruction memory
- Updates Program Counter (PC)
- Passes instruction to IF/ID pipeline register

#### 2. Instruction Decode (`instruction_decode()`)
- Decodes instruction opcode
- Reads source operands from register file
- Detects data hazards and inserts stalls when necessary
- Extracts immediate values
- Passes decoded information to ID/EXE pipeline register

#### 3. Execute (`execute_stage()`)
- Performs ALU operations (arithmetic/logic)
- Calculates memory addresses for load/store
- Evaluates branch conditions
- Computes branch target addresses
- Passes results to EXE/MEM pipeline register

#### 4. Memory Access (`memory_stage()`)
- Handles load/store operations
- Reads from or writes to data memory
- Passes memory data or ALU results to MEM/WB pipeline register

#### 5. Write Back (`write_back()`)
- Writes results back to register file
- Completes instruction execution
- Updates destination registers

### Pipeline Registers

Inter-stage pipeline registers maintain instruction state:
- **IF/ID**: Program Counter, Next PC, Instruction Register
- **ID/EXE**: Source operands (A, B), Immediate, Next PC
- **EXE/MEM**: ALU Output, Branch condition, Source operand B
- **MEM/WB**: ALU Output, Load Memory Data (LMD)

### Data Hazard Handling

The simulator implements stall-based hazard resolution:
- Detects RAW dependencies between instructions
- Automatically inserts pipeline bubbles (NOPs) when hazards are detected
- Tracks stall cycles for performance analysis

## 🛠️ Technical Implementation

**Language**: C++  
**Key Components**:
- `sim_pipe.h` - Class definition and data structures
- `sim_pipe.cc` - Pipeline stage implementations and simulator logic
- Custom instruction memory and data memory models
- 32 general-purpose registers (R0-R31)
- Pipeline registers for inter-stage communication

## 📦 Building the Project

```bash
cd project1_code/c++
make testcase1  # Build specific test case
make all        # Build all test cases
```

## 🚀 Usage Example

```cpp
// Create simulator with 1MB data memory, 0 cycle latency
sim_pipe *mips = new sim_pipe(1024*1024, 0);

// Load assembly program
mips->load_program("asm/no_dep.asm", 0x10000000);

// Initialize registers
for (int i=0; i<7; i++) 
    mips->set_gp_register(i, i);

// Run for specific number of cycles
mips->run(10);  // Run 10 cycles

// Or run to completion
mips->run();    // Run until EOP (End of Program)

// Get performance metrics
float ipc = mips->get_IPC();
unsigned cycles = mips->get_clock_cycles();
unsigned stalls = mips->get_stalls();
```

## 📊 Sample Output

The simulator provides detailed cycle-by-cycle execution traces showing:
- Special purpose register values at each pipeline stage
- General purpose register contents
- Memory state before and after execution
- Performance statistics (IPC, cycles, stalls)

## 📝 Assembly Language Format

Instructions follow standard MIPS assembly syntax:
```assembly
LW    R1 0(R0)      # Load word
ADD   R2 R3 R4      # Add registers
ADDI  R3 R3 10      # Add immediate
SW    R2 0(R0)      # Store word
BEQZ  R1 label      # Branch if equal to zero
EOP                  # End of program
```

## 🎓 Learning Outcomes

This project demonstrates:
- Deep understanding of pipelined processor architecture
- Knowledge of instruction set architecture (ISA)
- Implementation of hazard detection logic
- C++ proficiency with complex data structures
- Low-level system programming skills
- Performance analysis and optimization concepts

## 🔧 Key Algorithms Implemented

- **Pipeline Control Logic**: Manages instruction flow through all stages
- **Hazard Detection**: Identifies data dependencies between instructions
- **Stall Insertion**: Resolves hazards by inserting pipeline bubbles
- **Branch Resolution**: Evaluates branch conditions and updates PC
- **Memory Management**: Handles load/store with little-endian format

---

**Tech Stack**: C++, Computer Architecture, MIPS ISA, Pipeline Design, Hazard Detection
