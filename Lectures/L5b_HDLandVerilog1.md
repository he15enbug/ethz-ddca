# Lecture 5b: Hardware Description Languages and Verilog I

- Hardware Description Languages, HDL
  - What we need for hardware design
    - describe/specify ccomplex designs
    - simulate their behavior (funcctional & timing)
    - synthesize (auto design) portions of it
      - have an error-free path to impl.
  - There are similarly-featured HDLs (e.g., Verilog, VHDL, ...)
  - All hardware logicc operates concurrently

## Key Design Principle: Hierarchical Design

- Design a hierarchy of modules
  - Predefined "primative" gates (AND, OR, ...)
  - Simple modules built by instantiating these gates (e.g., cmponents like MUXes)
  - Complex modules built by instantiating simple modules
- Hierarchy controls complexities
- Top-Down Design Methodology
  - Define the top-level module, and identify the sub-modules
  - Subdivide the sub-modules unil we come to leaf cells
- Bottom-Up Design Methodology
  - First identify building blocks
  - Build bigger modules

## Defining a Module in Verilog

- A *module* is the main building block in Verilog
  - Name of the module
  - Directions of its ports (e.g., input, output)
  - Names of its ports
    
    ```verilog
    module example (a, b, y);
        input a;
        input b;
        output y;
    endmodule
    ```

    ```verilog
    module example (
        input a, 
        input b, 
        output y);
    endmodule
    ```

- Define multi-bit Input/Output (Bus)
  - `[range_end : range_start]`
  - Number of bits: `end - start + 1`

    ```verilog
    input [31:0]   a;
    output [15:8] b1; // b1[15], b1[14], ..., b1[8]
    output [7:0]  b2; // b2[7], ..., b2[0]
    input          c; // single signal
    ```

- Manipulating bits

    ```verilog
    // We can assign partial buses
    wire [15:0] longbus;
    wire [7:0] shortbus;
    assign shortbus = longbus[12:5]

    // Concatenating is by {}
    assign y = {a[2], a[1], a[0], a[0]}

    // Possible to define multiple copies
    assign x = {a[0], a[0], a[0], a[0]}
    assign y = { 4{a[0]} }
    ```

- Verilog is case sensitive, cannot start with numbers

## Two Main Styles of HDL Implementation

- **Structural (Gate-Level)**
  - The module body contains *gate-level description* of the circuit
  - Describe how modules are interconnected
  - Each module contains other modules (instances) and interconnection between those modules
  - Describes a hierarchy of modules defined as gates
  - Example:

    ```verilog
    module small (
            input A,
            input B,
            output Y);

    endmodule

    module top (
            input A,
            input SEL,
            input C,
            output Y,
            wire n1);

        // instantiate small once
        small i_first ( .A(A),
                        .B(SEL),
                        .Y(n1));
        // small i_first ( A, SEL, n1 ); // The short form

        // instantiate small twice
        small i_second ( .A(n1),
                        .B(C),
                        .Y(Y));

    endmodule
    ```

  - Primitives

    ```verilog
    module mux2 (
            input d0, d1,
            input s,
            output y);
        wire ns, y1, y2;

        not g1 (ns, s);
        and g2 (y1, d0, ns);
        and g3 (y2, d1, s);
        or  g4 (y, y1, y2);

    endmodule
    ```

- **Behavioral**
  - The module body contains *functional description* of the circuit
  - Contains logical and mathematical *operators*
  - Level of abstraction is higher than gate-level
    - Many possible gate realization of a behavioral description
  - E.g.,

    ```verilog
    module example (a, b, c, y);
        input a, b, c;
        output y;

        assign y = ~a & ~b & ~c |
                    a & ~b & ~c |
                    a & ~b &  c;

    endmodule
    ```

    ```verilog
    module (
        input [3:0] a, b,
        input s,
        output [3:0] y1, y2, y3, y4, y5, y6);

        assign y1 = a & b;
        assign y2 = a | b;
        assign y3 = a ^ b;    // XOR
        assign y4 = ~(a & b); // NAND
        assign y5 = ~(a | b); // NOR
        
        assign y6 = &a; // AND each bit of a, a[3] & a[2] & a[1] & a[0]

        assign y7 = s ? a : b; // if (s) then y7 = a else y7 = b

    endmodule
    ```

    ```verilog
    module (
            input [3:0] d0, d1, d2, d3
            input s,
            output [3:0] y);

    assign y = (s == 2'b11) ? d3 :
               (s == 2'b10) ? d2 :
               (s == 2'b01) ? d1 :
               d0;

    endmodule
    ```

- Many practical designs use a combination of both
- Express numbers: `N'Bxx`
  - `N`: # of bits
  - `B`: base, binary `b`, hexadecimal `h`, decimal `d`, octal `o`
  - `xx`: number, can also have `X` (invalid) and `Z` (floating), as values
    - **Floating signals (Z)**: signals that are not driven by any circuit (open circuit, floating wire), aka. high impedance, hi-Z, tri-stated signals

## What happens with HDL code

- **Synthesis (i.e., HW Synthesis)**
  - Modern tools are able to map synthesizable HDL code into low-level *cell libraries* -> netlist describing gates and wires
  - They can perform many optimizations
  - But cannot guarantee that a solution is optimal
    - Mainly due to ccomputationally expensive placement and routing algorithms
  - Most common way of Digital Design these days
- **Simulation**
    - Allows the behavior of the circuit to be verified without actually manufacturing the circuit
    - Works on structural or behavioral HDL
    - Essential for functional and timing verification

## Parameterized Modules

```verilog
module mux2
    # (parameter width = 8) // name and default value
      ( input [width-1:0] d0, d1,
        input s,
        output [width-1:0] y);

    assign y = s ? d1 : d0;

endmodule
```
```verilog
module example (...)
  mux2 i_mux (d0, d1, s, out);
  mux2 #(12) i_mux_b (d0, d1, s, out);
  mux2 #(.width(10)) i_mux_c (.d0(d0), .d1(d1), .s(s), .out(out));
endmodule
```

## Timing

- It's possible to define timing relations in Verilog. BUT
  - ONLY for simulation
  - CANNOT be synthesized
  - Used for modeling delays in a circcuit
  - Example

    ```verilog
    'timescale 1ns/1ps // time unit: 1 nanosecond , time precision: 1 picosecond
    module simple (input a, output z1, z2);

        assign #5 z1 = ~a; // output z1 will get the inverted value of a 5ns after a changes
        assign #9 z2 = a;  // output z2 will get the value of 9ns after a changes

    endmodule
    ```

## Good Practices

- One module per file, file name matches modulee name

# Readings

- Von Neuman Model, LC-3, and MIPS
  - P & P
    - Chapter 4, 5
    - Appendices A and C (ISA and microarchitecture of LC-3)
  - H & H
    - Chapter 6
    - Appendix (MIPS instructions)
- Programming
  - P & P Chapter 6
- Recommended: Digital Building Blocks
  - H & H Chapter 5
