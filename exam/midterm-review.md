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
   1. Review the definition of the [**retiming vector**](https://wenbo-notes.gitbook.io/ee4415-icd-notes/lecture/lec-02/lec-02b-rtl-transformations#the-retiming-vector) $$r(V)$$, it denotes the movement of registers across the opeartor $$V$$.

### RTL Transformation 1

#### Retime to minimize the clock period

In the page 5, Q1 (d), the question askes us to use 2-way time interleaving to minimize the clock period. This problem can be transformed into a classic **cut cake** problem below.

<figure><img src="../.gitbook/assets/cut-cake-1.svg" alt=""><figcaption></figcaption></figure>

Our job is to move the two registers (one at the input and one at the output) between the combinational logic part and find out what's the **minimum sum** that we can achieve. And this minimum sum will be our critical path length.

{% hint style="success" %}
The delay of the multiplier is 20ns and adder is 8ns in this problem.
{% endhint %}

Using the above information on the delay, no matter what method we try, we can minimally achieve 36ns for the critical path.

<figure><img src="../.gitbook/assets/cut-cake-2.svg" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
So, in **time-interleaving**, the minimum clock period is determined by the **min cut-cake problem**. In register insertion problem, the minimum clock period is determined by the **iteration bound**.
{% endhint %}

## Tips

1. For **retiming**, the gaussian surface **can** cross the loop, and we move the **registers** from the **input** to the **output** or vice versa.
2. For register insertion, we can only do **feedforward cutset register insertion**.
3. When the questions says **pipelining**, we can use **feedforward cutset register insertion**. When it says **retiming**, we can use **retiming**!
4. **Always** double check the **critical path**! The path that is found might not be actually a critical path!  This is an eye-sight problem actually.
5. Memorize the time unit table!

> TODO:
>
> 1. Pipelining can we use time interleaving? The registers at input/output can move during the retiming?
> 2. Do the [DICADP question](../textbook-1-dicadp/timing-issues-in-digital-circuits/synchronous-design-an-in-depth-perspective.md#example-propagation-and-contamination-delay-estimation) on the false path and the skew! Very classic one.
> 3. How to draw the parallel DFG elegantly? (RTL Transformation PS 1 (d).
> 4. For the throughput analysis in parallelism, do I need to write exact precise number (RTL Transformation PS 1 (d)?
> 5. Retiming is a technique that we can always do even if the problem not specify (RTL Transformation PS 2(b)?
> 6. The overhead in any path just need to be added once (RTL Transformation PS 2(c))?
> 7. The term "multiply-add" operation is T means that the one multiplier + one adder is takes T? (RTL Transformation PS 5(a)).
> 8. The block architecture, using SIPO/PISO or shifted clock phase? (RTL Transformation PS 5b and 5c).
> 9. The critical path doesn't need to take the skew, jitter and setup, hold into account? Only consider the combinational parts' delay? (RTL Transformation PS 3(a))
> 10. What does the term "constant inputs do not need to be retimed at all"? (RTL Transformation PS 3 (b))
> 11. If a diagram got two inputs, can draw the gaussian that includes one input, or must include two? (RTL Transformation PS 3(b))
> 12. The contamination delay information is missing in RTL Transformation PS 3(c)? (I feel like the contamination delay is always missing).
> 13. RTL Transformation PS page 5, Q1(e), 2-way parallelism will give us double frequency?
> 14. For throughput analysis, we analyze frequency, and latency. But latency is in clock cycles or absolute time?
> 15. RTL Transformation PS Page 7, Q6(a) is worth going through again! Especially know how to get the relationship between the input and the output. (How to denote another new variable?)
> 16. RTL Transformation PS Page 7, Q6(c), the logic imbalance only exists in the loop with 2 registers?
> 17. RTL Transformation PS Page 7, Q6(d), if the question only says 4-slow, not retiming, should we assume we can do retiming?
> 18. Confrim the RTL Transformation PS Page 8 (a), the retiming is valid or not (In Good Notes already).
> 19. Confrim the RTL Transformation PS Page 8 (b), "fine-grained" pipelining means that the adder and multiplier are all pipelined to have a delay of 0.5 clock cycles? Or I have to do it manually?
> 20. So, in **time-interleaving**, the minimum clock period is determined by the **min cut-cake problem**. In register insertion problem, the minimum clock period is determined by the **iteration bound**. Confirm this.
> 21. In RTL Tranformation PS (Big), Q1-a, the critical path just a path, in other words, we just need to find the path and state that path is the critical path?
> 22. We only can bring calculator, no need to print out the Verilog helpsheet.
> 23. RTL Transformation PS (Big) Q1C, the retiming is wrong as it didn't duplicate the register at the input and output? Q1B also a big wrong with the overhead.
> 24. RTL Transformation PS (Big) Q2(b), when calculating loop bound, don't care about the register overhead, but when calculating the maximum frequency that a system can reach, remember to add the register overhead? Also this question has wrong figure and Q2(a) is wrong!
