# Lec 03a - Digital Design Flow

## ASIC Fundamentals

### What is an ASIC

ASIC design, or Application-Specific Integrated Circuit design, is the specialized process of developing integrated circuits (ICs) that are custom-built for a specific application, product, or use case. Unlike general-purpose chips, such as microprocessors or FPGAs (Field Programmable Gate Arrays), ASICs are optimized to perform a predefined function with maximum efficiency, high performance, and low power consumption

In modern practice, most ASICs are **not designed at the transistor level**. Instead, they are built by assembling **pre-designed and pre-characterized logic cells** from a **standard-cell library**. These cells include gates, flip-flops, multiplexers, memories, and other functional blocks, each with known area, timing, and power properties.

Therefore, a more practical way to define an ASIC is by its **design style** rather than by its application:

> An ASIC is a VLSI design that is implemented using a **cell library-based methodology**, where a digital system is synthesized, placed, and routed using standard cells provided by a semiconductor foundry or IP vendor.

### Design Styles

ASICs can be classified by **how much of the silicon is customized** and **when the functionality is fixed**.

<figure><img src="../../.gitbook/assets/types-of-asics.png" alt=""><figcaption></figcaption></figure>

#### Full-Custom ASIC

In a **full-custom** design, the entire chip is designed at the **transistor and layout level**.

* All mask layers are customized
* Digital logic, analog circuits, memories, and I/O are fully hand-designed
* Allows **maximum performance, lowest power, and smallest area**
* Extremely expensive and time-consuming

The full-custom method is more complex and costly, but it can do much more than the gate array method. The size of the ASIC decreases significantly as the design incorporates only the necessary gates and electronics, and unused gates are deleted. These ASICs are designed for a specific purpose and support a particular function in the end product.

{% hint style="info" %}
This style is used for high-end CPUs, GPUs, RF circuits, analog/mixed-signal ICs, high-speed interfaces.
{% endhint %}

#### Semi-Custom ASIC

> This approach involves predefining diffused layers, transistors, and other active devices to minimize initial design efforts, which reduces non-recurring engineering costs. The production cycles are significantly shorter because the metallization process is utilized, which is a relatively swift process compared to full custom design. During the final design phase, engineers manipulate specific switches, opening and closing them to guide the chip's behavior according to the desired specifications.

In **semi-custom** design, the chip is built from **pre-designed logic cells**, but the **interconnections are customized**.

{% stepper %}
{% step %}
#### Cell-Based ASIC

This type of ASIC uses predesigned logic cells called standard cells, such as gates, multiplexers, and flip-flops. Standard cells are made using full-custom design methodology and serve as basic building blocks for ASIC design, ensuring the same performance and flexibility but reducing time and risk.
{% endstep %}

{% step %}
#### Gate-array Based ASIC

In this category of ASIC, transistors, logic gates, and other active devices are created and manufactured on a silicon wafer, while interconnects are not formed during fabrication. The pre-established arrangement of transistors on the gate array is referred to as the base array, and the smallest repetitive element forming the gate array is called the base cell. Several advantages accompany this approach, including a shorter turnaround time, higher logic density, and customization of contact layers.
{% endstep %}
{% endstepper %}

#### Programmable ASIC

This type of ASIC can be programmed at the hardware level after manufacturing. Unlike traditional ASICs, which are custom-designed and fabricated for specific applications, programmable ASICs offer a degree of flexibility and reprogramming. Programmable logic devices (PLDs) and [field-programmable gate arrays (FPGAs)](https://app.gitbook.com/s/jTJFBPtKk6NwweAooH53/textbook/digital-building-blocks/logic-arrays#field-programmable-gate-array) are perfect examples of programmable ASICs.

> What is mask layer?

## The Design Flow Lifecycle

### Cell-Based Design Flow

The cell-based flow is a standard industry methodology for taking a design from concept to physical silicon.

{% stepper %}
{% step %}
#### Front-End (Logical Design)

* **Spec Development:** Defining the requirements of the chip.
* **HDL (RTL) Coding & Simulation:** Writing the design in a HDL and verifying its logical behavior.
* **Preliminary Synthesis:** Converting the code into a generic gate-level netlist.
* **Preliminary Floorplanning:** Estimating the area and initial placement to refine the synthesis.
* **Design for Testability (DFT):** Adding hardware to allow the chip to be tested after manufacturing; involves Test Pattern Generation.
* **Pre-layout Simulation:** Verifying the logic again before physical layout begins.
{% endstep %}

{% step %}
#### Back-End (Physical Design)

* **Layout:** Involves Floorplanning (area allocation), Placement (fixing cell locations), and Routing (connecting wires).
* **Post-layout Simulation & Static Timing Analysis (STA):** Verifying the design with real wire delays to ensure it meets speed requirements.
* **ECO (Engineering Change Order):** Making small, late-stage manual fixes to the design.
* **Layout Verification:** Ensuring the physical file is error-free. This includes:
  * **DRC:** Design Rule Checking (physical spacing rules).
  * **ERC:** Electrical Rule Checking (power/ground rules).
  * **LVS:** Layout vs. Schematic (ensuring the layout matches the logic).
  * **Antenna & Metal Density:** Checks for manufacturing reliability.
{% endstep %}
{% endstepper %}

## References

1. [Synopsys — What is ASIC Design?](https://www.synopsys.com/glossary/what-is-asic-design.html)
