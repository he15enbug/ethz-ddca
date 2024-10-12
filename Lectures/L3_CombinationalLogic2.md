# Lecture 3: Combinational Logic II

## Recall

- Do NOT connect the p-MOS down or the n-MOS up when buiding gates, or they won't work well
- We can only build up an AND gate by combining a NOT gate and an NAND gate
- The general form used to construct any inverting logic gate, such as: NOT, NAND, or NOR

    ```        high V
                |
             +-----+
             |     | ---> pMOS pull-up network
             +-----+
             |     |
    IN (A) --+     +-- OUT (Y)
             |     |
             +-----+
             |     | ---> nMOS pull-down network
             +-----+
                |
                0 V
    ```

- Exactly one network should be ON, and the other should be OFF at any given time
  - If both networks are ON at the same time, there is a **short circuit** -> likely incorrect operation
  - If both OFF, the output is **floating** -> undefined (not necessarily incorrect, in some cases, it is designed to be like this)

## MOS transistors are imperfect switches
- pMOS: pass 1's well but 0's poorly (hole carry charge) (pulling u**p** the output)
- nMOS: pass 0's well but 1's poorly (electrons carry charge) (pulling dow**n** the output)

## Latency

- Transistors in series or parallel, which one is faster?
  - Parallel faster. More transistors in series, more resistance on the wire
- How to alleviate this latency?
  - H & H Section 1.7.8 example: pseudo-nMOS Logic

## Power Consumption

- Dynamic Power Consumption: power used to charge capacitance as signals change: `Capacitane * Voltage^2 * freqency`
- Static Power Consumption: power used when signals do not change: `Voltage * I_leakage`
- Energe Consumption: `Power * Time`

## A logic circuit

- Composed of:
    1. Inputs
    2. Outputs
    3. Functional specification (describes relationship between I & O)
    4. Timing specification (describes the delay between inputs changing and outputs responding)

### Combinational Logic

- Memoryless
- Outputs are strictly dependent on the combination of input values that are being applied to circuit right now
- In some books, called Combinatorial Logic

### Sequential Logic

- Has memory: structure stores history
- Outputs are determined by previous and current values of inputs

### Functional Specification

- Functional spec. of outputs in terms of inputs
- Example: full 1-bit adder

    ```
    S     = F(A, B, C_in) = A XOR B XOR C_in
    C_out = G(A, B, C_in) = AB + BC_in + AC_in
    ```

### Boolean Algebra

#### Axioms

- B contains at least 2 elements 0 and 1, such that `0 != 1`
- Closure
- Commutative Laws
- Identities
- Distributive Laws
- Complement

#### Duality

- All axioms come in dual form
- A dual of a Boolean expression is derived by replacing:
  - AND <-> OR
  - constant 1 <-> constant 0

#### DeMorgan's Law

- `NOT (A1 + A2 + ... + An) = (NOT A1) + (NOT A2) + ... + (NOT An)`

#### Some definitions
  - *Complement*: variable with a bar over it
  - *Literal*: variable or its complement
  - *Implicant*: product (AND) of literals
  - *Minterm*: product (AND) that includes all input variables
  - *Maxterm*: sum (OR) that includes all input variables
- **Truth table** is the unique signature of a Boolean function
- A boolean function can have many alternative boolean expressions, which lead to different logic gate implementations -> different cost, latency, energy properties
- Canonical form: standard form for a boolean expression
  - Provides a unique algebraic signature

#### SOP (sum-of-products) leads to two-level logic: one level to get the products, and another level to get the sum

- Any 1-bit function can be represented as a Sum of Products
- Standard notation for SOP form: if we agree on the order of the variables in the rows of truth table, we can enumerate each row with the decimal number that corresponds to the binary number created by the input pattern
  - E.g., `(A, B, C) = (1, 0, 0)` stands for decimal `4`, and this midterm is denoted as `m4`
  - For SOP `f = (¬A)BC + A(¬B)(¬C) + A(¬B)C + AB(¬C) + ABC`, it can be represented as `f = m3 + m4 + m5 + m6 + m7 = SUM m(3, 4, 5, 6, 7)`

#### POS (product-of-sums), alternative canonical form

- DeMorgan of SOP of (¬f)
- F = AND of all input variable combination that result in a 0
- How to write it
    1. Find truth table rows where F is 0
    2. 0 in input col -> true literal, 1 in input col -> complemented literal
    3. OR the literals to get a maxterm
    4. AND togetehr all the maxterms
- E.g., `F = PRODUCT M(0, 1, 2)`

#### Useful Conversions

- Minterm and maxterm: e.g., `F(A, B, C) = SUM m(3, 4, 5, 6, 7) = PRODUCT M(0, 1, 2)`
- `F` to `¬F`: `F(A, B, C) = SUM m(3, 4, 5, 6, 7), ¬F(A, B, C) = PRODUCT M(3, 4, 5, 6, 7)`

#### Logic Simplification (or Minimization)

#### Basic Combinational Blocks

##### Decoder

- "Input patter detector"
- `n` inputs and `2^n` outputs
- Exactly one of the outputs is 1 and all the rest are 0s
- E.g., a 2-to-4 decoder

    ```
    A0 A1 | Y3 Y2 Y1 Y0
    ------+------------
     0  0   0   0  0  1
     0  1   0   0  1  0
     1  0   0   1  0  0
     1  1   1   0  0  0
    ```
- The decoder is useful in determining how to interpret a bit pattern
  - Could be the address of a location in memory that the processor intends to read from
  - Could be an instruction in the program and the processor needs to decide what action to take (based on instruction opcode)

##### Multiplexter (MUX), or Selector

- Select one of the `N` inputs to connect it to the output
  - based on the value of a log_2(N)-bit control input called `select`
- Example: 2-to-1 MUX

    ```
    S D1 D0 | Y
    --------+---
    0  0  0 | 0
    0  0  1 | 1
    0  1  0 | 0
    0  1  1 | 1
    1  0  0 | 0
    1  0  1 | 0
    1  1  0 | 1
    1  1  1 | 1
    ```
- MUXs can be used as lookup tables to perform logic functions

## Programmable Logic Array, PLA

- A PLA enables the two-level SOP implementation of any N-input M-output function
- An array of AND gates followed by an array of OR gates
- How to determine the number of AND gates?
  - Remember SOP: the number of possible minterms
  - For an n-input logic function, we need a PLA with `2^n` n-input AND gates
- How to determine the number of OR gates?
  - The number of output columns in the truth table

- **Programming a PLA**: we program the connections from AND gate outputs to OR gate inputs to implement desired logic function

    ```
      Inputs
        |
        |
    +-------+                +-------+
    |  AND  |---Implicants---|  OR   |
    | Array |                | Array |
    +-------+                +-------+
                                |
                                |
                              Outputs
    ```

- Any other type of programmable logic? An FPGA
- Implementing a Full Adder Using a PLA

## Reading

- Hardware Description Language and Verilog
  - H & H Chapter 4 until 4.3 and 4.5
- Sequential Logic
  - P & P Chapter 3.4 until end
  - H & H Chapter 3 in full

- More combinational building blocks
  - H & H 
    - Chapter 2 in full, e.g., see Tri-state buffer and Z values in Section 2.6
    - Chapter 5, e.g., Adder, Subtractor, Comparator, Shifter/Rotator, Multiplier, Divider
