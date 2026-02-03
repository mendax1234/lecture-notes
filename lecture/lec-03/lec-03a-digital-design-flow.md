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

* All [mask layers](#user-content-fn-1)[^1] are customized
* Digital logic, analog circuits, memories, and I/O are fully hand-designed
* Allows **maximum performance, lowest power, and smallest area**
* Extremely expensive and time-consuming

The full-custom method is more complex and costly, but it can do much more than the gate array method (or called "[programmable](lec-03a-digital-design-flow.md#programmable-asic)" method). The size of the ASIC decreases significantly as the design incorporates only the necessary gates and electronics, and unused gates are deleted. These ASICs are designed for a specific purpose and support a particular function in the end product.

{% hint style="success" %}
This style is used for high-end CPUs, GPUs, RF circuits, analog/mixed-signal ICs, high-speed interfaces.
{% endhint %}

#### Semi-Custom ASIC

> This approach involves **predefining** diffused layers, transistors, and other active devices to minimize initial design efforts, which reduces non-recurring engineering costs. The production cycles are significantly shorter because the metallization process is utilized, which is a relatively swift process compared to full custom design. During the final design phase, engineers manipulate specific switches, opening and closing them to guide the chip's behavior according to the desired specifications.

In **semi-custom** design, the chip is built from **pre-designed logic cells**, but the **interconnections are customized**.

{% stepper %}
{% step %}
#### Cell-Based ASIC

This type of ASIC uses predesigned logic cells called **standard cells**, such as gates, multiplexers, and flip-flops. **Standard cells** are made using **full-custom** design methodology and serve as basic building blocks for ASIC design, ensuring the same performance and flexibility but reducing time and risk.
{% endstep %}

{% step %}
#### Gate-array Based ASIC

In this category of ASIC, transistors, logic gates, and other active devices are created and manufactured on a silicon wafer, while **interconnects** are not formed during fabrication. The pre-established arrangement of transistors on the gate array is referred to as the base array, and the smallest repetitive element forming the gate array is called the **base cel**l. Several advantages accompany this approach, including a shorter turnaround time, higher logic density, and customization of contact layers.
{% endstep %}
{% endstepper %}

#### Programmable ASIC

This type of ASIC can be programmed at the hardware level after manufacturing. Unlike traditional ASICs, which are custom-designed and fabricated for specific applications, programmable ASICs offer a degree of flexibility and reprogramming. Programmable logic devices (PLDs) and [field-programmable gate arrays (FPGAs)](https://app.gitbook.com/s/jTJFBPtKk6NwweAooH53/textbook/digital-building-blocks/logic-arrays#field-programmable-gate-array) are perfect examples of programmable ASICs.

## The Design Flow Lifecycle

### ASIC Design Flow

The ASIC design flow describes the sequence of steps used to transform a **high-level system idea** into a **manufacturable integrated circuit**. Each step progressively adds more implementation detail, moving from abstract functionality to physical silicon.

{% stepper %}
{% step %}
#### Design Entry

> Describe what the chip should do.

The designer enters the circuit into an ASIC design system. Historically, this was done using **schematics**, but modern designs use **Hardware Description Languages (HDLs)** such as Verilog or VHDL, or even higher-level models such as **SystemC**.

This stage defines:

* The functionality of the chip
* The data paths and control logic
* Clocking and reset behavior

**Output:** RTL or high-level behavioral description of the system.
{% endstep %}

{% step %}
#### System Partitioning

> Break a large system into manageable blocks.

A complex chip is divided into smaller **subsystems or modules**, arranged hierarchically. Each block is designed to be small enough to fit within the limits of current ASIC technology and tools.

This step decides:

* Which functions go into which block
* How blocks communicate (interfaces, buses, clocks)
* What is hardware vs software (if applicable)

**Output:** Block-level architecture and interface definitions.
{% endstep %}

{% step %}
#### Logic Synthesis

> Convert abstract logic into real hardware.

The HDL description is converted into a **gate-level netlist** using a logic synthesis tool.\
This netlist contains:

* Standard cells (AND, OR, flip-flops, multiplexers, etc.)
* Their logical connections

The synthesis tool optimizes the design for:

* Performance/Speed (timing)
* Power
* Area

**Output:** Technology-mapped gate-level netlist.
{% endstep %}

{% step %}
#### Pre-layout simulation

> Verify functional correctness before physical design.

The synthesized netlist is simulated to ensure the design still behaves correctly after synthesis. At this stage:

* Only logic delays are considered
* Wire delays are not yet included

**Output:** **Functionally** verified gate-level design.

{% hint style="success" %}
This catches logic errors introduced during synthesis.
{% endhint %}
{% endstep %}

{% step %}
#### Floor Planning

> Define the chip's physical organization.

The overall layout of the chip is planned. This includes:

* Placement of major blocks (CPU, memory, IO, etc.)
* Location of I/O pads
* Power and clock distribution
* Global and local routing regions

Good floorplanning is critical for:

* Performance
* Power distribution
* Routing success

**Output:** Chip-level physical blueprint.
{% endstep %}

{% step %}
#### Placement

> Decide where each cell goes.

All standard cells from the netlist are assigned exact physical locations inside their blocks.\
The placement tool tries to:

* Minimize wire length
* Reduce congestion
* Improve timing

**Output:** Physically placed cells, but not yet wired.
{% endstep %}

{% step %}
#### Routing

> Create all electrical connections.

Metal wires are created to connect:

* Cells inside blocks
* Different blocks
* Power and clock networks

**Output:** Fully routed chip layout.

{% hint style="success" %}
This step produces the full **physical layout** of the chip.
{% endhint %}
{% endstep %}

{% step %}
#### Circuit Extraction

> Find the real electrical behavior of wires.

From the physical layout, tools calculate:

* Resistance (R)
* Capacitance (C)

of every wire and interconnect. These parasitic values affect:

* Delay
* Power
* Signal integrity

**Output:** An extracted RC model of the chip.
{% endstep %}

{% step %}
#### Post-layout Simulation

> Verify that the real chip still works.

The design is simulated again using:

* Gate delays
* Extracted wire delays and capacitances

This checks whether:

* Timing constraints are still met
* The chip operates correctly at its target speed

If problems are found, the design may need to go back to placement or routing for fixes.

**Output:** A design that is ready for fabrication.
{% endstep %}
{% endstepper %}

#### Cell-Based Design Flow

The cell-based flow is a standard industry methodology for taking a design from concept to physical silicon.

{% stepper %}
{% step %}
#### Front-End (Logical Design)

* **Spec Development:** Defining the requirements of the chip. e.g., the throughput, power etc.
* **HDL (RTL) Coding & Simulation:** Writing the design in a HDL and verifying its logical behavior.
* **Preliminary Synthesis:** Converting the code into a generic gate-level netlist.
* **Preliminary Floorplanning:** Estimating the area and initial placement to refine the synthesis.
* **Design for Testability (DFT):** Adding hardware to allow the chip to be tested after manufacturing; involves Test Pattern Generation.
* **Pre-layout Simulation:** Verifying the logic again before physical layout begins.
{% endstep %}

{% step %}
#### Back-End (Physical Design)

* **Layout:** Involves Floorplanning (area allocation), Placement (fixing cell locations), and Routing (connecting wires).
* **Post-layout Simulation & Static Timing Analysis (STA):** Verifying the design with real wire delays to ensure it meets speed requirements. (This will be done in the second half of EE4415)
* **ECO (Engineering Change Order):** Making small, late-stage manual fixes to the design.
* **Layout Verification:** Ensuring the physical file is error-free. This includes:
  * **DRC:** Design Rule Checking (physical spacing rules).
  * **ERC:** Electrical Rule Checking (power/ground rules).
  * **LVS:** Layout vs. Schematic (ensuring the layout matches the logic).
  * **Antenna & Metal Density:** Checks for manufacturing reliability.
{% endstep %}
{% endstepper %}

{% hint style="warning" %}
This is the industry version of the ASIC Design Flow we introduce below/later.
{% endhint %}

### Levels of Abstraction

The levels of abstraction is ASIC Design Flow can be summarized as follows

<figure><img src="../../.gitbook/assets/level-of-abstraction-asic-design-flow.png" alt=""><figcaption></figcaption></figure>

### Logic Design Approaches

There are two primary ways to approach the creation and verification of digital logic:

{% stepper %}
{% step %}
#### Capture-and-Simulation

* **Method:** This involves manually drawing the schematic representation of gates and flip-flops.
* **Verification:** The design is debugged and verified through simulation to ensure the logic gates behave as intended.
* **Context:** This is a more traditional, visual approach to circuit design.
{% endstep %}

{% step %}
#### Describe-and-Synthesis

* **Method:** The designer writes the logic using high-level descriptions such as:
  * Boolean equations
  * Finite State Machines (FSM)
  * Hardware Description Languages (HDL) like Verilog or VHDL
* **Automation:** A software tool called a synthesizer performs the transformation and compilation.
* **Outcome:** The synthesizer automatically converts the high-level description into a gate-level netlist.
{% endstep %}
{% endstepper %}

### Simulation Levels

#### Behavioral Level Simulation

The highest level of abstraction in the design process, focusing on what the system does rather than how it is physically built.

* **Purposes:**
  * **Functionality:** Verifying that the logic performs the intended task.
  * **Algorithmic Correctness:** Ensuring the underlying mathematical or logical algorithms are sound before hardware details are added.
* **Ways to implement:**
  * **System Tools:** System Studio (Synopsys SystemC), MatLab, or SDL (Specification and Description Language).
  * **High-Level Languages:** C, C++, or Java.
  * **HDLs:** SystemVerilog, Verilog, or VHDL.
* **Key Drawback:** No Cycle-Accuracy. It does not necessarily capture the exact clock cycle counts or precise hardware timing of the final product.

#### RTL-Level Simulation

**RTL (Register-Transfer Level)** is the most common level for synthesis, representing the design in terms of registers and the data moving between them.

* **Purposes:**
  * **Validation Model for Structural Code:** Acts as a bridge between high-level algorithms and gate-level implementation.
  * **Full Functionality:** Provides a complete functional description of the hardware.
* **Key Characteristics:**
  * **Register Transfer Operations:** Specifically details how data is stored in and moved between registers (e.g., flip-flops).
  * **Cycle Accurate:** Unlike behavioral simulation, RTL is synchronized with clock cycles, ensuring the design produces results at the correct time.
  * **Synchronous Logic:** Highly dependent on clock signals to guide operations.

#### Logic Synthesis

Logic synthesis is the process that provides a link between a high-level HDL (Verilog or VHDL) and a gate-level netlist.

* **Techniques used:** Two-level/multi-level logic minimization, FSM encoding, and various heuristics.
* **Common Tools:** Design Compiler (Synopsys) and BuildGates (Cadence).

{% stepper %}
{% step %}
#### The Synthesis Process

Synthesis is defined by the formula: **Translation + Optimization + Mapping.**

1. **Translate:** Converts the HDL source code into a "Generic Boolean" format (often called GTECH).
2. **Optimize + Map:** Refines the logic and maps it to the specific gates available in the Target Technology library.

<figure><img src="../../.gitbook/assets/synthesis-process.png" alt=""><figcaption></figcaption></figure>
{% endstep %}

{% step %}
#### Benefits of Using Synthesis

* **Efficiency**: Greatly improves **productivity** (handling millions of gates in months) and provides "**Design Tricks**" by automatically managing loads, fanouts, and library limits.
* **Quality**: Higher **abstraction** allows designers to focus on high-level issues while the tool handles the "dirty work" of meeting constraints.
* **Flexibility**: Promotes **reusability** (parameterized code) and **portability** across different tools and technology-independent designs.
* **Reliability:** The design is more **verifiable** and less error-prone since it is validated and implemented in the same language.
{% endstep %}
{% endstepper %}

## Design Guidelines

Logic synthesis is an **NP-Hard** problem, meaning that as a circuit gets larger, the time and complexity required to find the "perfect" solution grow exponentially.

* **Heuristics:** Because it's too hard to find a perfect solution, tools use "heuristics" (educated guesses/rules of thumb) to find a "near-optimal" result.
* **Optimization vs. Guarantee**: Modern synthesis algorithms do not guarantee the best possible circuit; they simply improve the **starting circuit** we provided.

Two different pieces of code might do the exact same thing (functionally equivalent), but they will yield different synthesis results.

* **Coding Style Matters:** If we write messy or inefficient code, the tool will have a "Poor Start Point" and likely produce a slower or larger chip.
* **The "Fix-it" Myth**: We cannot rely solely on the synthesis tool to "fix" or optimize a poorly designed or poorly coded circuit.

To get the best result, the designer must:

* Deeply understand the circuit being described before writing the code.
* Provide a "Best Start Point" by writing clean, efficient, and hardware-aware HDL to give the tool the best chance at a high-quality final result.

<figure><img src="../../.gitbook/assets/asic-design-flow-starting-point.png" alt=""><figcaption></figcaption></figure>

And below are the two guidelines recommended to follow when designing ASIC

### Think Hardware

Synthesis tools are designed to create physical circuits, so our HDL code must describe actual physical structures rather than abstract software behaviors.

* **Write HDL Hardware Descriptions:**
  * Always think of the **topolog**y (the physical arrangement of components) that our code implies.
  * Our goal is to describe a network of registers, muxes, and gates.
* **Do Not Write HDL Simulation Models:**
  * Avoid using code intended only for software-style testing.
  * **No Explicit Delays**: Commands like "after 20 ns" or "wait 20 ns" cannot be manufactured into hardware gates.
  * **No File I/O**: Hardware cannot "read" or "write" text files the way a computer program does; keep these commands out of synthesizable code.

{% hint style="warning" %}
One essential skill we must master after taking EE4415 is the ability to understand what hardware is inferred from one or more lines of HDL after synthesis.
{% endhint %}

### Think Synchronous

The reliability and ease of manufacturing a chip depend heavily on how the timing is managed.

* **Benefits of Synchronous Designs:**
  * Synchronous designs (where everything is timed to a clock) run smoothly through the entire lifecycle: **synthesis, test, simulation, and layout.**
  * These designs are easier for tools to analyze for timing errors.
* **Challenges of Asynchronous Designs:**
  * Asynchronous logic (logic not tied to a common clock) is much harder to verify and often requires **hand-instantiation**.
  * They require extensive, complex simulations to ensure they work correctly.
* **Design Strategy**: If we must use asynchronous logic, isolate it into separately compiled blocks to prevent it from complicating the rest of the synchronous system.

## References

1. [Synopsys — What is ASIC Design?](https://www.synopsys.com/glossary/what-is-asic-design.html)

[^1]: A mask layer in VLSI is a photolithographic template (photomask) used during fabrication to define specific geometric patterns of materials — such as silicon, polysilicon, or metal — onto a silicon wafer
