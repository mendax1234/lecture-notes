# Lec 4 - Automated Synthesis

The big picture of this lecture is that in ASIC design, we must explicitly partition the overall system into smaller blocks and manually define the input and output timing constraints for each partition. These constraints correspond to setup and hold time requirements at the interfaces between blocks.

Unlike FPGA design flows, where much of the partitioning and timing management is handled automatically by tools like Vivado, ASIC design requires designers to carefully plan hierarchical boundaries and specify timing budgets themselves. Timing constraints are therefore central to ASIC implementation.

{% hint style="warning" %}
While area is certainly an important optimization objective in ASIC design, it is typically handled through synthesis and physical design optimization rather than through manually imposed area constraints at the partition level.
{% endhint %}

<details>

<summary>Timing in FPGA vs. ASIC</summary>

Unlike FPGA flows where pre-manufactured fabric allows tools like Vivado to globally manage hierarchy and timing, ASIC design mandates a "Divide and Conquer" approach due to the sheer scale of modern chips. This requires manual **Time Budgeting**, where designers explicitly allocate timing portions to partitioned blocks because the tools cannot optimize the entire design flat. Without these specific boundary constraints, ASIC synthesis tools default to unrealistic assumptions — such as infinite drive strength and unloaded outputs — resulting in silicon that fails in reality despite passing simulation.

</details>
