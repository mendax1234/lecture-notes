---
metaLinks:
  alternates:
    - /broken/spaces/o4JqYVwVk7U2q66ddB5L/pages/EN5IgwJplfTP1jP1b4Fr
---

# Classification of Digital Systems

In digital systems, signals can be classified depending on how they are related to a local clock.

1. Signals that transition only at predetermined periods in time can be classified as synchronous, mesochronous, or plesiochronous with respect to a system clock.
2. A signal that can transition at arbitrary times is considered asynchronous.

## Synchronous Interconnect

In digital systems, a **synchronous signal** is one that has the exact same [frequency](classification-of-digital-systems.md#whats-the-frequency-of-a-signal-that-is-not-a-clk-signal) and a known fixed phase offset with respect to the local clock. In such a timing methodology, the signal is “synchronized” with the clock, and the data can be sampled directly without any uncertainty. In digital logic design, synchronous systems are the most straightforward type of interconnect, where the flow of data in a circuit proceeds in lockstep with the system clock as shown below.

<figure><img src="../../.gitbook/assets/dicadp-synchronous-interconnect-methadology.png" alt=""><figcaption></figcaption></figure>

Here, the input data signal `In` is sampled with register R<sub>1</sub> to give signal C<sub>in</sub>, which is synchronous with the system clock and then passed along to the combinational logic block. After a [suitable setting period](#user-content-fn-1)[^1], the output C<sub>out</sub> becomes valid and can be sampled by R<sub>2</sub> which synchronizes the output with the clock. In a sense, the "certainty period" of signal C<sub>out</sub>, or the period where data is valid is synchronized with the system clock, which allows register R<sub>2</sub> to sample the data with complete confidence. The length of the “uncertainty period,” or the period where data is not valid, places an _upper bound on how fast a synchronous interconnect system can be clocked_.

In short, in digital logic, **synchronization** turns "random arrival" into "scheduled arrival".

1. The input `In` might arrive at any random time. It is "asynchronous." However, once `In` goes into R<sub>1</sub>, the output C<sub>in</sub> is now synchronized. C<sub>in</sub> is _only_ allowed to change its value immediately after the rising edge of the `CLK`.
2. However, there is another problem, which is with the _combination logic_ in between. When C<sub>in</sub> changes, the logic gates inside take time to calculate. During this calculation, the signal C<sub>out</sub> might glitch, toggle randomly, or be invalid for a few nanoseconds. This is the "uncertainty." To solve this problem, we use "synchronization"
   1. The system ignores everything happening in the middle of the clock cycle (the messy part).
   2. R<sub>2</sub> is trained to only look at the data at the _very end_ of the cycle (the next clock edge).
   3. By that time, the "messy" calculation must be finished, and the data must be stable ("valid").
3. So, when we say a signal is synchronized, we are effectively saying: "I guarantee that this signal will be stable and valid setup time before the next clock edge arrives." This also explains why the data can be sampled directly without any uncertainty if the timing constraint is met.

<details>

<summary>What's the frequency of a signal that is not a CLK signal?</summary>

In the context of that definition, the "frequency of the signal" actually refers to the **frequency of the clock domain that generates/launches that signal.**

For example, if we have a signal called `valid_flag`. And `valid_flag` is output by a register triggered by a 100 MHz clock. Then, for the purpose of synchronization and timing analysis, `valid_flag` is considered a 100 MHz signal.

</details>

[^1]: This is the **propagation delay** of the combinational logic.
