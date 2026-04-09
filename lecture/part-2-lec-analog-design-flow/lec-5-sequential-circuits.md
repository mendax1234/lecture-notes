# Lec 5 - Sequential Circuits

In this lecture, we will first review the timing metrics of registers covered in EE4415 Part 1 [Lec 1b](../part-1-lec-digital-design-flow/lec-1b-timing-synchronous.md). And then, we will focus on the static vs. dynamic storage. In summary, the difference between static storage and dynamic storage is shown below

* **Static storage** (Latches, Flip-flop)
  * Preserve state as long as the power is on
  * Have positive feedback (regeneration) with an internal connection between the output and input
  * Useful when updates are infrequent
* **Dynamic Storage**
  * Store state on parasitic capacitors (**gate capacitances**)
  * Only hold state for **short periods** of time (milliseconds)
  * Require periodic refresh
  * Usually simpler, higher speed and lower power

## Static Latches and Registers

