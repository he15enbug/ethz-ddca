# Lab

## What will we learn

- The Transformation Hierarcchy

```
+-------------------------+
|          Problem        |
|-------------------------|
|         Algorithm       |
|-------------------------|
|     Program/Language    |
|-------------------------|
|      System Software    |
|-------------------------|
|      SW/HW Interface    | <------+
|-------------------------|        |
|    Micro-architecture   | <------+---The lab will focus on
|-------------------------|        |
|          Logic          | <------+
|-------------------------|
|          Devices        |
|-------------------------|
|         Electrons       |
+-------------------------+
```

- Trade-offs between performance and area/complexity
- Hardware Prototyping FPGA
- Debugging hardware implementation
- HDL, Hardware Description Language
- Hardware Design Flow
- Computer-Aided Design (CAD) Tools

## What is FPGA

- **Field Programmable Gate Array**, a software-reconfigurable hardware substrate

- Flexibility <-- CPUs -- GPUs -- **FPGAs** -- ASICs --> Efficiency

## High level Lab Summary

- Build a 32-bit microprocessor running on the FPGA board

## High level Overview of FPGAs

- We configure logic blocks, their connections, and IO blocks to create hardwware circuits and map program onto those circuits

## FPGA Architecture

- Two main building blocks:
  1. LUT, Look-Up Tables
  2. Switches
- Modern FPGA Arch.
  - Typically use LUTs with 6-bit select input (6-LUT), thousands of them
  - MegaBytes of distributed on-chip mem
  - Hard-coded special-purpose hardware blocks for high-performance operations
    - Mem interface
    - Low latency and high bandwidth off-chip I/O
  - Even a general-purpose processor embedded within the FPGA chip
