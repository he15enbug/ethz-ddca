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

- **Glitch**: one input transition causes multiple output transitions (the output changes multiple times)
  - The delay of each path is different, the fast path changes the output first
- **(Opional) Avoiding Glitches Using K-Maps**
- Q: Do we always care about glitches?
  - Fixing glitches is undeirable
  - The circuit is eventually guaranteed to converge the right value regardless of glitchiness
- A: No, not always
  - If we only care about the long-term steady state output, we can safely ignore glitches

## Sequential Circuit Timing

- The D Flip-Flop
  - D must be stable when sampled (i.e., at active clock edge)
  - **Setup time `t_setup`**: time before the clock edge that data must be stable (i.e., not changing)
  - **Hold time `t_hold`**: time after the clock edge that data must be stable
  - **Aperture time `t_a`**: time around clock edge that data must be stable (`t_a = t_setup + t_hold`)
- Violating input timing: metastability
- Filp-Flop Output Timing
  - Contamination delay clock-to-q (`t_ccq`)
  - Propagation delay clock-to-q (`t_pcq`)


# Reading

- HDL and Verilog
  - H & H Chapter 4
- Timing and verification
  - H & H Chapter 2.9 and 3.5 + (start Chapter 5)
