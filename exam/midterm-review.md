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

## Tips

1. For **retiming**, the gaussian surface **can** cross the loop, and we move the **registers** from the **input** to the **output** or vice versa.
2. For register insertion, we can only do **feedforward cutset register insertion**.
3. When the questions says **pipelining**, we can use **feedforward cutset register insertion**. When it says **retiming**, we can use **retiming**!

> TODO: Pipelining can we use time interleaving? The registers at input/output can move during the retiming?
