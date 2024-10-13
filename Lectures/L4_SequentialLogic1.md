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

### Circuits that can store info, with memory

- Sequential Circuit = Combinational Circuit + Storage Elements

#### Cross-Coupled Inverters

- Has 2 stable states: `Q=1` or `Q=0`
- Has a third possible "metastable" state with both outputs oscillating between 0 and 1
- Not useful without a *control mechanism* for setting Q

#### More reliable storage elements

- Latches and Flip-Flops
  - Very fast, parallel access
  - Very expensive, 1 bit costs tens of transistors
- Static RAM
  - Relatively fast
  - Expensive, 1 bit costs 6+ transistors
- Dynamic RAM
  - Slower, reading destroys content (refresh), needs special process for manufacturing
  - Cheap, 1 bit costs only 1 transistor and 1 capacitor
- Other storage technology (flash memory, hard disk, tape)
  - Much slower, **non-volatile**
  - Very cheap

#### The R-S (Reset-Set) Latch

```
Q  = !(S AND Q')
Q' = !(R AND Q )
```

- Cross-coupled NAND gates
  - Data is stored at Q (inverse at Q')
  - S and R are control inputs
    - In *quiescent (idle) state*, both S and R are held at 1
    - S (set): drive S to 0 (keeping R at 1) to change Q to 1
    - R (reset): drive R to 0 (keeping S at 1) to change Q to 0
    - S,R=0,0
      - breaks the invariant that Q=!Q'
      - Q and Q' begin to oscillate (metastability)
      - Thiss eventually settles dependingg on variation in the circuits (more on this in the **Timing Lecture**)

#### The Gated D Latch

- Ensure that people cannot violate the invariant to guarantee correct operation of R-S Latch: add two more NAND gates!

    ```
    D ------------+
        |         | 
        |   +---NAND--- S --- ...
        |   |
        |  Write Enable
        |   |
        |   +---NAND --- R --- ...
        |         |
        +---NOT---+
    ```

- When Write Enable (WE) is set to 1, `S = !D`, `R = D`, `Q = D`
- When WE is set to 0, `S = 1`, `R = 1`, `Q = Q_previous`

#### The Register

- Use D-Latch to store more data: use multiple D latches, but with a single WE signal for all latches for simultaneous writes

#### Memory

- Memory is comprised of locations that can be written to or read from
- Every unique location in mem is indexed with a unique address
- *Addressability*: the number of bits of info stored in each location. E.g., the addressibility is 8 bits, if each mem location can store 8 bits of data
- The entire set of unique locations in mem is referred to as the **Address Space**

##### Reading from mem

- Address decoder + multiplexer (together with the decoder)

##### Writing to mem

```
    Addr[0]                     D_i[n]------+
       |                                    |
       +---NOT--> AND -----+--> D Latch <---|
       |           |                        |
       |   WE----->+                        |
       |           |                        |
       +--------> AND --------> D Latch <---+
```

##### Aside: Implementing Logic Functions Using Memory

##### Memory-Based Lookup Table Example (from H & H)

- Mem arrays can also perform Boolean Logic Functions
  - 2^N-location M-bit memory can perform any N-input, M-output function
  - LUT, Lookup Table: Memory array used to perform logic functions
  - Each address: row in truth table
  - Each data bit: corresponding output value

### State

- **State**: The state of a system is a snapshot of all relevant elements of the system at the moment of the snapshot

### Finite State Machine
