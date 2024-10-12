# Lecture 2

## A: Tradeoffs, Metrics, Mindset

- Data is key for future workloads
- Data movement overwhelms modern machines

### Novel concepts investigated today

- New computing paradigms (rethinking the full stack)
  - Processing in memory, processing near data
  - Neuromorphic computing, quantum computing
  - Fundamentally secure and dependable computers
- New accelerators and systems (algorithm-hardware co-designs)
  - AI & ML
  - Graph & Data analytics, vision, video
  - Genome analysis
- New memories, storage systems, interconnects, devices
  - Non-volatile main memory, intelligent memory systems
  - High-speed interconnects, disaggregated systems

### Increasingly complex systems

### Evaluation Criteria for the Designs

### Basic Building Blocks

- Electrons
- Transistors: a **transistor** is a semiconductor device used to amplify or switch electrical signals and power
- Logic Gates
- Combinational Logic Circuits
- Sequential Logic Circuits
  - Storage elements and memory
- ...
- Cores
- Caches
- Interconnect
- Memories
- ...

## B: Combinational Logic I

### 3 key components of a computer

1. Computation
2. Communication
3. Storage/memory

### General to Special Purpose

- CPUs, GPUs, FPGAs, ASICs (Application Specific Integrated Circuits)

### Build a microprocessor on FPGA

- Microprocessor: common building block of computers
- FPGAs: reconfigurable hardware, flexible
- ASICs: you customize everything

### Transistors

- Computers are built from simple structures: transistors
- MOS (Metal-Oxide-Semiconducto) transistor
  - Combining
    1. Conductors (Metal)
    2. Insulators (Oxide)
    3. Semiconductors
- Source - Gate - Drain
- 2 types of MOS transistors
  1. *n-type*: if the gate of an n-type transistor is supplied with a high voltage, the connection from source to drain acts like a piece of wire (i.e., the circuit is closed) (Depending on the tehcnology, high voltage range from 0.3V to 3V). If ... zero voltage, connection broken (i.e., the circuit is open)
  2. *p-type*: exactly opposite: zero voltage -> connected; high voltage -> not connected
  - Both operate logically

### Build Logic Structures out of MOS transistors

- Construct basic logical units out of individual MOS transistors, these **logical units** are called **logic gates**, they implement simple boolean functions
- Modern computers use both n- and p-type transistors, i.e., complementary MOS (CMOS) technology
- The simplest logic structure that exists in a modern computer: the *inverter* (the *CMOS NOT Gate*)

    ```        3 V
                |
             +--p--+
             |     |
    IN (A) --+     +-- OUT (Y)
             |     |
             +--n--+
                |
               0 V
    ```
- Digital circuit: interpret 0V as logical 0 value, and 3V (high voltage) as logical 1 value

## Reading Assignments

- Combinational Logic chapters from both books
  - Patt & Patel, Chapter 3
  - Harrison & Harris, Chapter 2
