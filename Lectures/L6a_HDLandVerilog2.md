# Lecture 6a: Hardware Description Languages and Verilog II

## Implementing Sequential Logic

- Describe HW that has mem.
  - *Flip-Flops*, *Latches*, *FSM*
- Sequential Logic state transition is triggered by a "CLOCK" signal
  - Latches are sensitive to the level of signal
  - Flip-Flops are sensitive to the transitioning of signal
  - We need new construccts
    - `always`
    - `posedge/negedge`
- Example

    ```verilog
    // Whenever the event in the sensitivity list occurs, the statement is executed
    always @ (sensiivity list)
        statement;
    ```

    ```verilog
    module flop(input          clk,
                input      [3:0] d,
                output reg [3:0] q);
        always @ (posedge clk) // posedge defines a rising edge (0 to 1)
            q <= d; // pronounced "q gets d"
    
    endmodule
    ```

- `assign` is not used in `always`
- `<=` describes a **non-blocking** assignment
- In `always` block, assigned variables need to be declared as `reg`. The name `reg` does not necessarily mean that the value is a register (it could be, but does not have to be)

### Async. and Sync. Reset

- `reset` signals are used to initialize the HW to a known state, usually activated at system start
- Async. reset
  - Sampled independent of clock, gets the highest priority
  - Sensitive to **glitches**, may have **metastability** issues
- Sync. reset
    - Results in **completely synchronous circuit**
- Impl. of Async. reset

    ```verilog
    module flop(input          clk,
                input          reset,
                input      [3:0] d,
                output reg [3:0] q);
        always @ (posedge clk, negedge reset)
            begin
                if (reset == 0) q <= 0; // when reset
                else            q <= d; // when clk
            end
    endmodule
    ```

- Impl. of Sync. reset

    ```verilog
    module flop (input          clk,
                 input          reset,
                 input      [3:0] d,
                 output reg [3:0] q);
        always @ (posedge clk)
            begin
                if (reset == 0) q <= 0; // when reset
                else            q <= d; // when clk
            end
    endmodule
    ```

- Impl. of D latch

    ```verilog
    module latch (input      [3:0] d,
                  input            clk,
                  output reg [3:0] q);

    always @ (clk, d) // triggered when any change of clk or d
        if (clk) q <= d; // latch is transparent when clk is 1

    endmodule
    ```

- Usages

    ```verilog
    module example (input            clk,
                    input      [3:0] d,
                    output reg [3:0] q);

        wire [3:0] normal;  // standard wire
        reg  [3:0] special; // assigned in always

        always @ (posedge clk)
            special <= d;

        assign normal = ~special;

        always @ (posedge clk)
            q <= normal;

    endmodule
    ```

- **Assignment to the same signal in different always blocks is NOT allowed**
- When clk is not rising, the value of `q` is preserved
- But an `always` block does NOT always remember

    ```verilog
    always @ (inv, data)
        if (inv) result <= ~data;
        else     result <= data;
    ```

- An `always` block defines combinational logic if
  - All outputs are always (continuously) updated
  - All right-hand side signals are in the sensitive list
    - Can use `always @*` for short
  - All left-hand side signals get assigned in every possible condition of `if ... else` and `case` blocks

- `always` is NOT always practical/nice

    ```verilog
    reg  [31:0] result;
    wire [31:0] a, b, comb;
    wire        sel;

    // 1
    always @ (a, b, sel)
        if (sel) result <= a;
        else     result <= b;
    
    // 2
    assign comb = sel ? a : b;
    
    // Both 1 and 2 describes the same MUX
    ```

- Case Statements

    ```verilog
    always @ (*)
        case (data)
            4'd0: segments = 7'b111_1110;
            4'd1: segments = 7'b111_0000;
            default: segments = 7'000_0000; // required
        endcase
    ```

- Summary:
  - `if .. else` can ONLY be used in `always` block
  - Use `casex` to check for don't cares (`X`)
  - The `always` block is combinational only if all `regs` within the block are always assigned to a signal

### Non-Blocking vs. Blocking Assignments

- Non-blocking (`<=`)
  - all assignments are made at the end of the block
  - all assignments are made in parallel, process flow is not blocked

    ```verilog
    always @ (a)
    begin
        a <= 2'b01;
        b <= a;
        // all assignments are made here
        // b is not yet 2'b01
    end
    ```

- Blocking (`=`)
  - Each assignment is made immediately
  - Process waits until the first assignment is complete, it block progress
  - Similar to sequential programs

    ```verilog
    always @ (a)
    begin
        a = 2'b01;
        // a is 2'b01
        b = a;
        // b is now 2'b01
    end
    ```

- Why use (Non-)Blocking Statements
  - NB statements allow operating on old values
  - B statements allow a sequence of operations

- Rules for signal assignment
  - Use `always @ (posedge clk)` and non-blocking to model sync. sequential logic
  - Use continous assignments (`assign`) to model simple combinational logic
  - Use `always @ (*)` and blocking assignments (`=`) to model more complicatedd combinational logic
  - CANNOT make assignments to the same signal in more than one `always` block or in a continuous assignment
    
    ```verilog
    (INVALID)
    always @ (*)
        a = b;
    always @ (*)
        a = c;
    ```

    ```verilog
    (INVALID)
    always @ (*)
        a = b;
    assign a = c;
    ```

## Implement FSM using `always`

### Example: output Y is high for one clock cycle out of every 3, i.e., the output divides the frequency of the clock by 3

- At each rising edge of the clock, transit to the next state, output 1 when state is `S0`, otherwise, output 0

    ```
    --reset--> S0 --posedge clk--> S1 --posedge clk--> S2
                |                                       |
                +<-------<------posedge clk------<------+
    ```

- Implementation

    ```verilog
    module freq3 (input clk,
                  input reset,
                  output  y);

    reg [1:0] reg state, nextstate;

    always @ (posedge clk, posedge reset)
    begin
        if (reset) state <= S0;
        else       state <= nextstate;
    end

    always @ (*)
        case (state)
            2'b00:   nextstate = 2'b01;
            2'b01:   nextstate = 2'b02;
            2'b10:   nextstate = 2'b00;
            default: nextstate = 2'b00;
        endcase

    assign y = (state == b'00);

    endmodule
    ```

### Example: the smiling snail

#### Moore FSM

- Very straightforward

#### Mealy FSM

```
State | Input | Output | Next State
------+-------+--------+------------
S0      0       0           S0
S0      1       0           S1
S1      0       0           S0
S1      1       0           S2
S2      0       0           S3
S2      1       0           S2
S3      0       0           S1
S3      1       1           S0
```

```verilog
module smilingsnail(input d,
                    input clk,
                    input reset,
                    output y);

reg [1:0] state, nextstate;

parameter s0 = 2'b00;
parameter s1 = 2'b01;
parameter s2 = 2'b10;
parameter s3 = 2'b11;


always @ (posedge clk, posedge reset)
    if (reset) state <= s0;
    else       state <= nextstate;

always @ (*)
    case  (state)
        s0:      nextstate = input == 1 ? s1 : s0;
        s1:      nextstate = input == 1 ? s2 : s0;
        s2:      nextstate = input == 0 ? s3 : s2;
        s3:      nextstate = input == 1 ? s1 : s0;
        default: nextstate = s0;
    endcase

assign y = (d == 1 & state == s3)

endmodule
```
