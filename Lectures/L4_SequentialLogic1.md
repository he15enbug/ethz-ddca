# Lecture 4: Sequental Logic I

## Recall: Combinational Logic Blocks

- Basic logic gates (AND, OR, NOT, NAND, NOR, XOR)
- Decoder
- Multiplexer, MUX
- Full Adder
- PLA
- Standard form representation: SOP & POS
- Logic simplification via Boolean Algebra

## Logic ( Functional) Completeness

- Any logic function we wish to implement couldd be accomplished with a PLA
  - PLA cconsists of only AND gates, OR gates, and inverts
  - We just have to program connections based on SOP of the intended logic function
- A set of gates {AND, OR, NOT} is logically complete
- **NAND is also logiccally complete. So is NOR**

## Equality Checker (Comparator, Compare if Equal)

- Checks if two N-input values are exactly the same
- `Equal = (A0 XNOR B0) AND (A1 XNOR B1) AND ... AND (An XNOR Bn)`

## ALU, Arithmetic Logic Unit

- Combines a variety of arithmetic and logical operations into a single unit (that performs only one function at a time)

## Tri-State Buffer

- A **tri-state** buffer enables gating of different signals onto a wire
- Example: (E is a special input (enable input), If E is 0, the buffer is not enabled, aand Y is not connected to A, Y is floting. If E is 1, Y is A)

    ```
        E
        |
       |\
    A--| >--Y
       |/

    E A | Y
    ----+---
    0 0 | Z (floating)
    0 1 | Z
    1 0 | 0
    1 0 | 1
    ```

- Floating signal (Z): signal that is not driven by any ciruit
  - Open circuit, floating wire

## Logic Simplification

- Key tool: **The Uniting Theorem**
- E.g., `F = A¬B + AB = A`, `F = ¬A¬B + A¬B = ¬B`

### Priority Circuit

- Inputs: Requestors with priority levels
- Outputs: Grant signal for each requetor
- X (Don't Care) means I don't care what the value of this input is

### Karnaugh Maps

- A pictorial way of minimizing circuits by visualizing opportuniies for simplification (H & H Chapter 2.7)

## Sequental Logic

- Circuits that can store info
- Finite State Machine

