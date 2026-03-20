# Lec 09 - Physical Synthesis

In this section, we are going to talk about physical synthesis where

1. the **input** is a [**netlist**](#user-content-fn-1)[^1] of gates (or blcoks) and their interconnections[^2]
2. the **output** is a [geometrical layout](#user-content-fn-3)[^3] of the netlist within an area constraint.

In the physical synthesis stage, we are trying to minimize signal delays, inter-connection area, number of layers, power and cross-talk.

## Physical Design Styles

{% hint style="info" %}
This part is covered in EE4415 [Lec 03 — ASIC Design Style](https://app.gitbook.com/s/Sp0XaarBjbEX3JIMrRaR/part-1-lec-digital-design-flow/lec-3/lec-3a-digital-design-flow#design-styles).
{% endhint %}

## Physical Design Flow

{% hint style="info" %}
This part is covered in EE4415 [Lec 03 — ASIC Design Flow](https://app.gitbook.com/s/Sp0XaarBjbEX3JIMrRaR/part-1-lec-digital-design-flow/lec-3/lec-3a-digital-design-flow#design-styles).
{% endhint %}

[^1]: This netlist is also in HDL and in EE4415 Lab 02, we can see that this netlist is nothing but the structural HDL using the standard cells in the technology library. But they are nothing but some HDL module instantiations.

[^2]: The interconnections here are more like how the instantiations in the netlist are **virtually connected**. The physical connection is done in routing.

[^3]: Can think of it as a rectangle canva with transistors organized into standard cells, and standard cells grouped into blocks and the wires connecting the standard cells.
