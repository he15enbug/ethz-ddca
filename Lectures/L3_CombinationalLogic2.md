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

## Reading

- Hardware Description Language and Verilog
  - H & H Chapter 4 until 4.3 and 4.5
- Sequential Logic
  - P & P Chapter 3.4 until end
  - H & H Chapter 3 in full