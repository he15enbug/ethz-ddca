# Lecture 5a: Sequential Logic II

## Finite State Machine

- 2 tyoes if FSN differ in the output logic
  1. **Moore FSM**: outputs depend only on the current state
  2. **Mealy FSM**: outputs depend on the current state and the e   
- **State Transition Table**

    ```
    Current State | Inputs | Next State
          S       |        |    S'
    --------------+--------+------------
         ...      |  ...   |   ...      

    State | Encoding
    ------+----------
      S0  |   00
    ------+----------
      S1  |   01
    ------+----------
      ... |   ...
    ```
- **The Output Table**
- **FSM Schematic: State Register**
- **Timing Diagram**
  - Reset doesn't depend on the clock
- **State Encoding**
  - 3 common state binary encodings with different tradeoffs
    1. Full encoding (binary encoding)
      - Use the min possible # of bits
      - E.g., `00`, `01`, `10`, `11`
      - **Minimizes** # flip-flops, but not necessarily output logic or next state logic
    2. One-hot encoding
      - Each bit encodes a different state
      - Uses `number_of_states` bits
      - E.g., `00001`, `00010`, ..., `10000`
      - Simplest design process
      - **Maximizes** # flip-flops, **minimizes** next state logic
    3. Output Encoding
      - Output are directly accessible in the state encoding
      - E.g., for 3 outputs, encode state wih 3 bits, where each bit represent an output
      - **Minimizes** output logic
      - **Only work for Moore Machines**
- **Moore vs. Mealy FSM**
- **FSM Design Procedure**
