# Midterm Review

In this part, I will cover some problems in the problem sets provide by Prof. Massimo. Besides, some tips are also provided for helping you review for the EE4415 Midterm.

## Problem Set

### Retiming

> This problem set is from the textbook called [_VLSI Digital Signal Processing Systems: Design and Implementation_](https://www.google.com.sg/books/edition/_/l5dTAAAAMAAJ?hl=en\&kptab=overview\&gbpv=0).

1. **(Ch4.6, Q2)**:
   1. The **maximum sample rate** = **maximum frequency**, and to find this number, we need to find the **critical path**.
   2. The **sample period** = **clock period**, to find the limit of it, we should find the **iteration bound** of the system.
2. **(Ch4.6, Q4)**:
   1. When doing **retiming**, we can draw the **guassian surface** very big so that it can cover the input/output registers.
3. **(Ch4.6, Q7)**:
   1. Review the definition of the [retiming vector](lec-2b-rtl-transformations.md#fundamental-definition) $$r(V)$$, it denotes the movement of registers across the opeartor $$V$$.

### RTL Transformation 1

#### Page 1

{% stepper %}
{% step %}
**Sub-question d**

In the question (d), we are asked to draw the 2-parallel version of the DFG. In the exam, we just **copy and paste** the original DFG and add either the SIPO/PISO or shifted clock cycle block at the input or output.

In the throughput analysis here, we **cannot assume** that the time-interleaving will give us an ideal case of $$n$$ times faster frequency! We must use the exact frequency we calculate after the actual **retiming**.
{% endstep %}

{% step %}
**Sub-question e**

In the exam, if we are not **given** the combinational delay of an operator, just treat the combinational delay to be 0. However, in the EE4415 midterm, it will be given **explicitly**.
{% endstep %}
{% endstepper %}

#### Page 2

In the **calculation** of the loop bound, the **register overhead** should only be added **once**! It doesn't depend on the number of registers in the loop! And whether we can ignore this register overhead or not will be stated explicitly in the question!

{% hint style="warning" %}
If there are **more than 1** registers in the loop, we should get the **balanced** logic by dividing first and then add the register overhead to the result once!
{% endhint %}

#### Page 3

In the question (c), we are asked to pipeline a **parallel** design. In this way, we just need to do the **pipelining** on one of the execution units and then copy and paste to the rest of the execution units.

#### Page 4

{% stepper %}
{% step %}
**Sub-question a**

In this question, we are asked to find the critical path. In this way, we just need to tell which **path** is the critical path, and we don't need to calculate the **minimum clock period** unless the problem explicitly specifies that.
{% endstep %}

{% step %}
**Sub-question b**

In this sub-question, there is one sentence saying that "constant inputs do not need to be retimed at all", what this really means is that we can **add or remove as many registers as we want** at those constant input edges (shown as the red edges below):

<figure><img src="../.gitbook/assets/midterm-1 (1).png" alt=""><figcaption></figcaption></figure>

Also, in this question, we get to know one very important point:

> We treat all the **inputs** as **one node** in
>
> 1. Retiming
> 2. Repipelining
>
> So, if our gaussian surface needs to include the inputs, we **must include all the inputs**!
{% endstep %}
{% endstepper %}

#### Page 5

In the page 5, Q1 (d), the question askes us to use 2-way time interleaving to minimize the clock period. This problem can be transformed into a classic **cut cake** problem below.

<figure><img src="../.gitbook/assets/cut-cake-1 (1).svg" alt=""><figcaption></figcaption></figure>

Our job is to move the two registers (one at the input and one at the output) between the combinational logic part and find out what's the **minimum sum** that we can achieve. And this minimum sum will be our critical path length.

{% hint style="success" %}
The delay of the multiplier is 20ns and adder is 8ns in this problem.
{% endhint %}

Using the above information on the delay, no matter what method we try, we can minimally achieve 36ns for the critical path length.

<figure><img src="../.gitbook/assets/cut-cake-2 (1).svg" alt=""><figcaption></figcaption></figure>

So, in this problem, as there is **no loop**, we can actually use the "min-cut cake" idea. But the overall idea should still be as follows:

> In a system, the **iteration bound** limits its **maximum frequency**.
>
> * In time interleaving, this iteration bound can be reduced by $$n$$ times ideally.
> * In repipelining, this iteration bound **cannot be changed**.

#### Page 7

{% stepper %}
{% step %}
**Sub-question a**

The analytical relationship between inputs and outputs is a **recursion** relationship in this problem. We need to give another name for the signal that coming out from the first adder.

{% hint style="danger" %}
This kind of question **won't** appear in EE4415 AY25/26 Sem 2 midterm!
{% endhint %}
{% endstep %}

{% step %}
**Sub-question c**

The **logic imbalance** only exists in a loop which has **more than or equal to** 2 registers!
{% endstep %}

{% step %}
**Sub-question d**

If the question says do n-slowing, we can also do the **retiming**! In other words, **n-slowing** and **retiming** are always combined and this is known as **time-interleaving**!
{% endstep %}
{% endstepper %}

#### Page 8

In the quesiton (b), "fine-grained pipelining" means that the multipliers and adders **can be internally pipelined** and this is **not suppposed to be done by the question**, but **should be done** by us.

By the way, in the question, by only using **repipelining**, we cannot achieve a clock period that is **smaller** than the loop bound. So, the answer is obviously "no" and no manual retiming needs to be done.

{% hint style="warning" %}
For the question (b), if you are trying to do the **feedforward cutset register insertion** on the DFG, note that the adder (operator) inside the loop (which means it is part of the loop) **cannot be crossed**!
{% endhint %}

### RTL Transformation 2

#### Question 3

In the sub-question, we are asked to discuss the effect of having an **iteration bound**. If there is no loop in the diagram, technically there is no lower bound for the clock frequency, but ultimately, the clock cycle is limited by the overhead and the large area/energy/latency overhead imposed by the registers, when repipelining aggressively.

#### Question 4

{% stepper %}
{% step %}
**Sub-question b**

In question b, when the clock frequency / clock period is given, we also need to **subtract** the **register overhead** from the given clock period to get the **maximum allowable time** for the combinational part.
{% endstep %}

{% step %}
**Sub-question c**

<figure><img src="../.gitbook/assets/midterm-2 (1).png" alt=""><figcaption></figcaption></figure>

This question is the first question I met to write a Verilog module by hand. The convention I used to solve this kind of question is to name/label all the intermediate signals with the following naming convention: `[type]_[stage]_[description]`

* `[type]`: Use `w` for combinational wires and `r` for register outputs.
* `[stage]`: Use `s1`, `s2`, `s3`, etc., to denote which pipeline stage the signal belongs to.
* `[description]`: A brief description of the mathematical operation or path (e.g., `top`, `diff`, `abs_A`).

{% hint style="warning" %}
Sometimes, the question will label the signal. If so there is no need to name it ourselves. In this case, just pay attention the the **width** of each signal.
{% endhint %}

To decide the bits used/width for the intermediate signal, we can use the following table, where `Wa` denotes the width of signal `A` and `Wb` denotes the width of signal `B`.

| Operation      | Expression | Result Bit Width                |
| -------------- | ---------- | ------------------------------- |
| Addition       | `A + B`    | `max(Wa, Wb) + 1`               |
| Subtraction    | `A - B`    | `max(Wa, Wb) + 1`               |
| Multiplication | `A * B`    | `Wa + Wb`                       |
| Division       | `A / B`    | Quotient: `Wa`, Remainder: `Wb` |

Using this convention, the complete code will look like below.

{% code lineNumbers="true" %}
```verilog
module pipelined_multiplier (
    input wire clk,
    input wire [15:0] A_in,
    input wire [15:0] B_in,
    output wire [31:0] Out
);
    // -------------------------------------------------------------------------
    // INPUT REGISTERS (REGA, REGB)
    // -------------------------------------------------------------------------
    reg [15:0] r_in_A;
    reg [15:0] r_in_B;

    always @(posedge clk) begin
        r_in_A <= A_in;
        r_in_B <= B_in;
    end

    // -------------------------------------------------------------------------
    // STAGE 1: Combinational Logic
    // -------------------------------------------------------------------------
    wire [15:0] w_s1_top_d1;
    wire [16:0] w_s1_diff;       // 17-bit as labeled in diagram
    wire [16:0] w_s1_diff_abs;
    wire [15:0] w_s1_and;
    wire [15:0] w_s1_bot_d1;

    assign w_s1_top_d1   = D1(ABS(r_in_A));
    assign w_s1_diff     = {1'b0, r_in_A} - {1'b0, r_in_B}; // Extended to 17-bit to prevent overflow
    assign w_s1_diff_abs = ABS(w_s1_diff); 
    assign w_s1_and      = r_in_A & r_in_B; // Bitwise AND
    assign w_s1_bot_d1   = D1(ABS(r_in_B));

    // STAGE 1: Pipeline Registers (First column of grey blocks)
    reg [15:0] r_s1_top;
    reg [16:0] r_s1_midtop;
    reg [15:0] r_s1_midbot;
    reg [15:0] r_s1_bot;

    always @(posedge clk) begin
        r_s1_top    <= w_s1_top_d1;
        r_s1_midtop <= w_s1_diff_abs;
        r_s1_midbot <= w_s1_and;
        r_s1_bot    <= w_s1_bot_d1;
    end

    // -------------------------------------------------------------------------
    // STAGE 2: Combinational Logic
    // -------------------------------------------------------------------------
    wire [31:0] w_s2_top_sqr;
    wire [31:0] w_s2_midtop_sqr;
    wire [31:0] w_s2_bot_sqr;
    wire [31:0] w_s2_sub;
    wire [31:0] w_s2_add;

    // Expanding to 32 bits for the final mathematical operations
    assign w_s2_top_sqr    = SQR_HALF(r_s1_top);
    assign w_s2_midtop_sqr = SQR_HALF(r_s1_midtop);
    assign w_s2_bot_sqr    = SQR_HALF(r_s1_bot);

    assign w_s2_sub = w_s2_top_sqr - w_s2_midtop_sqr;
    // Note: r_s1_midbot passes through from the previous stage, needs padding to 32 bits
    assign w_s2_add = {16'b0, r_s1_midbot} + w_s2_bot_sqr; 

    // STAGE 2: Pipeline Registers (Second column of grey blocks)
    reg [31:0] r_s2_sub;
    reg [31:0] r_s2_add;

    always @(posedge clk) begin
        r_s2_sub <= w_s2_sub;
        r_s2_add <= w_s2_add;
    end

    // -------------------------------------------------------------------------
    // STAGE 3: Final Combinational Logic (Output)
    // -------------------------------------------------------------------------
    // 32-bit final adder as labeled in the diagram
    assign Out = r_s2_sub + r_s2_add;

endmodule
```
{% endcode %}

{% hint style="warning" %}
When adding two signals, pay attention to the sign extension!
{% endhint %}
{% endstep %}
{% endstepper %}

#### Question 5

In question b, the **actual minimum clock** imposed by the loop is the **most unbalanced stage** in the loop (before retiming, if the question doesn't say that we can retime, just don't retime)!

#### Question 6

In question b, the correct code should be as follows.

{% code lineNumbers="true" %}
```verilog
module mac (
    input [15:0] A,
    input [15:0] B,
    input clk, 
    input RSTACC,          // Added reset signal here
    output [39:0] OUT 
);

    // Registers for inputs
    reg [15:0] Amult, Bmult;
    
    // NEW: Pipeline register for the multiplier output
    reg [31:0] OUTmult_reg; 
    
    // Accumulator register
    reg [39:0] OUT_previous;
    
    // Combinational wires for math
    wire [31:0] mult_result;

    // 1. Combinational Multiplier
    assign mult_result = Amult * Bmult;

    assign OUT = {8'b00000000, OUTmult_reg} + OUT_previous;

    // Sequential Logic (Registers)
    always @(posedge clk) begin
        // Input registers
        Amult <= A;
        Bmult <= B;
        
        // NEW PIPELINE STAGE: Register the multiplier output
        OUTmult_reg <= mult_result; 
        
        // ACCUMULATOR with Active-High Synchronous Reset
        if (RSTACC == 1'b1) begin
            OUT_previous <= 40'd0; // Clear the accumulator
        end else begin
            OUT_previous <= OUT; // Accumulate
        end
    end

endmodule
```
{% endcode %}

## Tips

1. For **retiming**, the gaussian surface **can** cross the loop, and we can move the **registers** from the **input** to the **output** or vice versa.
2. For register insertion, we can only do **feedforward cutset register insertion**.
3. When the questions says **pipelining**, we can use **feedforward cutset register insertion** and **retiming**. When it says **retiming**, we can only use **retiming**!
4. In the DFG, we assume there is always one register at the **input** and **output**. These two registers **can be moved** in retiming.
   1. In the midterm, the registers at the input and output will be added in the question. And whether these registers can be moved or not in retiming will be stated explicitly in the question.
5. **Always** double check the **critical path**! The path that is found might not be actually a critical path! This is an eye-sight problem actually.
6. Memorize the [time unit table](https://app.gitbook.com/s/6nPr3SObC3azazbFhfgF/lec/lec-01-the-devices#unit-table)!
7. In midterms, we can bring any number of calculators we want and don't need to print out the Verilog reference sheet by ourselves as it will be provided.
8. Usually, the sub questions in the midterm will be based on the original DFG and not be dependent on the other questions.
9. Do the [DICADP question](https://app.gitbook.com/s/poTRQsFwJ5BwDDy8paeZ/dicadp/timing-issues-in-digital-circuits/synchronous-design-an-in-depth-perspective#example-propagation-and-contamination-delay-estimation) on the false path and the skew! Very classic one.
10. (AY25/26 Sem 2 Midterm): The energy per operation of a design is to sum the energy consumed by **all the components** (registers, combinational operators)!
