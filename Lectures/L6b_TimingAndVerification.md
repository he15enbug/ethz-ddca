# Lecture 6b: Timing & Verification

- **Timing in combinational circuits**
  - Propagation delay and contamination delay
  - Glitches
- **Timing in sequential circuits**
  - Setup time and hold time
  - Determining how fast a circuit can operate
- **Circuit Verification**
  - How to make sure a circuit works correctly
  - Functional verification
  - Timing verification

## Tradeoffs in Circuit Design

- Area
- Speed / Throughput
- Power / Energy
- Design Time

## Combinational Circuit Timing

- Digital logic abstraction
  - Output changes immediately with the input
- In reality, circuit delay, caused by
  - Capacitance and resistance in a circuit
  - Finite speed of light (not so fast on a nanosecond scale)

- **Contamination delay (`t_cd`)**: delay until Y starts changing
- **Propagation delay (`t_pd`)**: delay until Y finishes changing
- Calculating Longest & Shortest Delay Paths
  - Critical (Longest) Path: use propagation delay
  - Shortest Path: use contamination delay

## Glitches

- **Glitch**: one input transition causes multiple output transitions

# Reading
