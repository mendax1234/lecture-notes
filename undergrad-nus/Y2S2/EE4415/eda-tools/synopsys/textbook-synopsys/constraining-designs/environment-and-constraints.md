---
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/Sp0XaarBjbEX3JIMrRaR/textbook-2-synopsys/constraining-designs/environment-and-constraints
---

# Environment and Constraints

In order to obtain optimum results from DC, designers have to methodically **constrain** their designs by describing

* the design environment,
* target objectives and
* design rules.

The **constraints** may contain **timing** and/or **area** information, usually derived from design specifications. DC uses these constraints to perform synthesis and tries to optimize the design with the aim of meeting target objectives.

## Design Environment

> Up until now, the assumption has been that the design has been partitioned, coded and simulated. In other words, we are doing with the RTL coding.

The next step is to describe the **design environment**. This procedure entails defining for the design, the process parameters, I/O port attributes, and statistical wire load models. Figure 6-1 illustrates the essential DC commands used to describe the design environment.

<figure><img src="../../.gitbook/assets/basic-design-environment (1).png" alt=""><figcaption></figcaption></figure>

### `set_min_library`

The command allows users to simultaneously specify the **worst-case** and the **best-case** libraries. This may be useful during initial compiles, preventing DC from violating the setup-time violations while fixing the hold-time violations.

{% code lineNumbers="true" %}
```tcl
set_min_library <max library filename> –min_version <min library filename>
```
{% endcode %}

For example,

{% code lineNumbers="true" %}
```tcl
set_min_library "ex25_worst.db" –min_version "ex25_best.db"
```
{% endcode %}

{% hint style="success" %}
**Helpful Ideas**

The above command may be used for fixing **hold-time** violations during incremental compile or for in place optimization. In this case, the user should set both minimum and maximum values for the operating conditions.
{% endhint %}

### `set_operating_conditions`

This command describes the process, voltage, and temperature conditions of the design.

{% hint style="warning" %}
The Synopsys library contains the description of these conditions, usually described as WORST, TYPICAL and BEST case. The names of operating conditions are library dependent. Users should check with their library vendor for correct setting.
{% endhint %}

By changing the value of the operating condition command, full ranges of process variations are covered.

* The WORST case operating condition is generally used during pre-layout synthesis phase, thereby optimizing the design for maximum setup-time.
* The BEST case condition is commonly used to fix the hold-time violations.
* The TYPICAL case is mostly ignored, since analysis at WORST and BEST case also covers the TYPICAL case.

{% code lineNumbers="true" %}
```tcl
set_operating_conditions <name of operating conditions>
```
{% endcode %}

For example,

{% code lineNumbers="true" %}
```tcl
set_operating_conditions WORST
```
{% endcode %}

{% hint style="success" %}
**Helpful Ideas**

It is possible to optimize the design both with the WORST and the BEST case, simultaneously. The optimization is achieved by using the `-min` and `-max` options in the above command, as illustrated below. This is very useful for fixing the design for possible hold-time violations.

{% code lineNumbers="true" %}
```tcl
set_operating_conditions -min BEST -max WORST
```
{% endcode %}
{% endhint %}

### `set_wire_load_model`

This command is used to provide estimated statistical wire-load information to DC, which in turn, uses the wire-load information to model net delays as a function of loading.

{% hint style="warning" %}
Generally, a number of wire-load models are present in the Synopsys technology library, each representing a particular size block. In addition, designers may also choose to create their own custom wire-load models to accurately model the net loading of their blocks.
{% endhint %}

{% code lineNumbers="true" %}
```tcl
set_wire_load_model -name <wire-load model>
```
{% endcode %}

For example,

{% code lineNumbers="true" %}
```tcl
 set_wire_load_model -name MEDIUM
```
{% endcode %}

### `set_wire_load_mode`

This command defines the three modes associated for modeling wire loads. These are top, enclosed, and segmented.

{% hint style="info" %}
Generally, only the first two modes are in common use. The segmented wire load mode is not prevalent, since it relies on the wire-load models that are specific to the net segments.
{% endhint %}

{% code lineNumbers="true" %}
```tcl
set_wire_load_mode < top | enclosed | segmented >
```
{% endcode %}

For example,

{% code lineNumbers="true" %}
```tcl
set_wire_load_mode top
```
{% endcode %}

{% hint style="success" %}
**Helpful Ideas**

It is extremely important that designers accurately model the wire loads of their design. Too optimistic or too pessimistic wire-load models result in increased synthesis iterations, in an effort to achieve timing convergence after post-layout. In general, during the prelayout phase, **slightly pessimistic wire-load models are used**. This is done to provide extra timing margin that may get absorbed, by the routed design.
{% endhint %}

#### `top` mode

The mode `top` defines that all nets in the hierarchy will inherit the **same** wireload model as the top-level block. One may choose to use this wire-load model for sub-blocks, if planning to flatten them later for layout. This mode may also be chosen, if the user is synthesizing the design using the bottomup compile method.

#### `enclosed` mode

The second mode, `enclosed` specifies that all nets (of the sub-blocks) will inherit the wire load model of the block that completely encloses the subblocks.

For example, if the designer is synthesizing sub-blocks B and C that are completely enveloped by block A (which in turn is completely enclosed by the top-level), then sub-blocks B and C will inherit the wire-load models defined for block A.

#### `segmented` mode

The last mode, `segmented` is used for wires crossing hierarchical boundaries. In the above example, sub-blocks B and C will inherit the wireload models specific to them, while the nets between sub-block B and C (but, within block A) will inherit the wire-load model specified for block A.

### `set_drive` and `set_driving_cell`

These commands are used at the **input** ports of the block.

#### `set_drive` command

`set_drive` command is used to specify the **drive strength** at the input port. It is typically used to model the external drive resistance to the ports of the block or chip.

{% hint style="warning" %}
The value of 0 signifies highest drive strength and is commonly utilized for **clock** ports.
{% endhint %}

{% code lineNumbers="true" %}
```tcl
set_drive <value> <object list>
```
{% endcode %}

For example,

{% code lineNumbers="true" %}
```tcl
set_drive 0 {CLK RST}
```
{% endcode %}

#### `set_drive_cell` command

Conversely, `set_driving_cell` is used to model the **drive resistance** of the driving cell to the **input** ports. This command takes the name of the driving cell as its argument and applies all design rule constraints of the driving cell to the input ports of the block.

{% code lineNumbers="true" %}
```tcl
set_driving_cell -cell <cell name> -pin <pin name> <object list>
```
{% endcode %}

For example,

{% code lineNumbers="true" %}
```tcl
set_driving_cell -cell BUFF1 -pin X [all_inputs]
```
{% endcode %}

### `set_load`

This command sets the **capacitive load** in the units defined in the technology library (usually pico farads, or pf), to the specified nets or ports of the design. It typically sets capacitive loading on **output** ports of the blocks during pre-layout synthesis, and on nets, for back-annotating the extracted post-layout capacitive information.

{% code lineNumbers="true" %}
```tcl
set_load <value> <object list>
```
{% endcode %}

For example

{% code lineNumbers="true" %}
```tcl
set_load 1.5 [all_outputs]
set_load 0.3 [get_nets blockA/n1234]
```
{% endcode %}

### Design Rule Constraints

**Design Rule Constraints** or DRCs consist of `set_max_transition`, `set_max_fanout` and `set_max_capacitance` commands. These rules are generally set in the technology library and are determined by the process parameters. These rules should not be violated in order to achieve working silicon.

The DRC commands can be applied to **input** ports, **output** ports or on the `current_design`. Furthermore, if the value set in the technology library is not adequate or is too optimistic, then these commands may also be used at the command line, to control the buffering in the design.

{% code lineNumbers="true" %}
```tcl
set_max_transition <value> <object list>
set_max_capacitance <value> <object list>
set_max_fanout <value> <object list>
```
{% endcode %}

For example,

{% code lineNumbers="true" %}
```tcl
set_max_transition 0.3 current_design
set_max_capacitance 1.5 [get_ports out1]
set_max_fanout 3.0 [all_outputs]
```
{% endcode %}

## Design Constraints

**Design constraints** describe the **goals** for the design. They may consist of **timing** or **area** constraints. Depending on how the design is constrained, DC tries to meet the set objectives. The basic commands to constrain a design are shown in Figure 6-2.

<figure><img src="../../.gitbook/assets/design-constraint-for-synthesis (1).png" alt="" width="563"><figcaption><p>Figure 6-2. Design Constraints for Synthesis</p></figcaption></figure>

### `creat_clock`

The `create-clock` command is used to define a clock object with a **particular period** and **waveform**.

* The `-period` option defines the **clock period**, while
* the `-waveform` option controls the **duty cycle** and the starting edge of the clock.

This command is applied to a pin or port, object types. For example,

{% code lineNumbers="true" %}
```tcl
create_clock–period 40 –waveform [list 0 20] CLK
```
{% endcode %}

The example above specifies that the port named `CLK` is of type "clock" that has a period of 40ns, with 50% duty cycle. The positive edge of the clock starts at time, 0 ns, with the falling edge occurring at 20 ns. By changing the falling edge value, the duty cycle of the clock may be altered.

### `create_generated_clock`

The `create_generated_clock` is used to describe **frequency divided/multiplied** clocks as a function of the **primary clock**.

{% code lineNumbers="true" %}
```tcl
create_generated_clock -name <clock_name> \
                       -source <clock_source> \
                       -divide_by <factor> | -multiply_by <factor>
                       ...
```
{% endcode %}

### `set_dont_touch_network`

This command is used to set a `dout_touch` property on a port, or on the net.

{% hint style="warning" %}
Note setting this property will also prevent DC from buffering the net, in order to meet DRCs.
{% endhint %}

For example,

{% code lineNumbers="true" %}
```tcl
set_dont_touch_network {CLK, RST}
```
{% endcode %}

### `set_dont_touch`

This command is used to set a `dont_touch` property on the `current_design`, cells, references or nets. It can also be used for preventing DC from inferring certain types of cells present in the technology library.

{% code lineNumbers="true" %}
```tcl
set_dont_touch current_design
set_dont_touch [get_cells sub1]
set_dont_touch [get_nets gated_rst]
```
{% endcode %}

{% hint style="success" %}
**Helpful Ideas**

For example, this command may be used on the block containing spare gates. The command will then instruct DC not to disturb (or optimize) the instantiation of the spare gates block.
{% endhint %}

### `set_dont_use`

This command is generally set in the `.synopsys_dc.setup` environment file. The command is instrumental in **eliminating certain types of cells** from the **technology library** that the user would not want DC to infer.

For instance, by using the above command, you can filter out the flip-flops in your technology library whose name start with "SDFF" or "RSFF" as illustrated below.

{% code lineNumbers="true" %}
```tcl
set_dont_use [list mylib/SDFF* mylib/RSFF*]
```
{% endcode %}

### `set_input_delay`

This command specifies the **input arrival time** of a signal in relation to the clock. It is used at the **input ports**, to specify the time it takes for the data to be stable after the clock edge. The timing specification of the design usually contains this information, as the setup/hold time requirements for input signals.

{% hint style="warning" %}
Given the top-level timing specification of the design, this information may also be extracted for the sub-blocks of the design, by utilizing the top-down characterize compile method or the design budgeting method, explained in the next chapter.
{% endhint %}

For example,

{% code lineNumbers="true" %}
```tcl
set_input_delay -max 23.0 -clock CLK {datain}
set_input_delay -min 0.0 -clock CLK {datain}
```
{% endcode %}

In Figure 6-3, the maximum input delay constraint of 23ns and the minimum input delay constraint of 0ns is specified for the signal `datain` with respect to the clock signal `CLK`, with a 50% duty cycle and a period of 30ns.

{% hint style="warning" %}
If both `-min` and `-max` options are omitted, the **same** value is used for both the maximum and minimum input delay specifications.
{% endhint %}

<figure><img src="../../.gitbook/assets/input-delay-example (1).png" alt=""><figcaption><p>Figure 6-3. Specification of Input Delay</p></figcaption></figure>

{% hint style="danger" %}
The `set_input_delay` affects our **data arrival time.**
{% endhint %}

#### Setup Timing Report

Below is a **setup timing report** generated using Synopsys's PT.

{% code lineNumbers="true" %}
```
****************************************
Report : timing
        -path full
        -delay max
        -max_paths 1
Design : PRGRM_CNT
Version: P-2019.03-SP4
Date   : Mon Feb 23 13:54:35 2026
****************************************

Operating Conditions: tt1p05v125c   Library: saed32rvt_tt1p05v125c
Wire Load Model Mode: enclosed

  Startpoint: Reset (input port clocked by Clk)
  Endpoint: PC_reg[0] (rising edge-triggered flip-flop clocked by Clk)
  Path Group: Clk
  Path Type: max

  Des/Clust/Port     Wire Load Model       Library
  ------------------------------------------------
  PRGRM_CNT          ForQA                 saed32rvt_tt1p05v125c

  Point                                    Incr       Path
  -----------------------------------------------------------
  clock Clk (rise edge)                    0.00       0.00
  clock network delay (ideal)              0.00       0.00
  input external delay                     1.00       1.00 f
  Reset (in)                               0.01       1.01 f
  U33/Y (INVX1_RVT)                        0.04       1.04 r
  U27/Y (NAND4X0_RVT)                      0.07       1.11 f
  U29/Y (INVX1_RVT)                        0.07       1.18 r
  U24/Y (AO22X1_RVT)                       0.06       1.25 r
  U23/Y (AO221X1_RVT)                      0.06       1.31 r
  PC_reg[0]/D (DFFX1_RVT)                  0.01       1.32 r
  data arrival time                                   1.32

  clock Clk (rise edge)                    1.50       1.50
  clock network delay (ideal)              0.00       1.50
  clock uncertainty                       -0.10       1.40
  PC_reg[0]/CLK (DFFX1_RVT)                0.00       1.40 r
  library setup time                      -0.03       1.37
  data required time                                  1.37
  -----------------------------------------------------------
  data required time                                  1.37
  data arrival time                                  -1.32
  -----------------------------------------------------------
  slack (MET)                                         0.05
```
{% endcode %}

To generate this file, we have the following command in our `constraints.tcl`, where `$all_in_ex_clk` is just a symbol used to represent all the signals except the `Clk`.

{% code lineNumbers="true" %}
```tcl
set_input_delay -max 1 -clock Clk $all_in_ex_clk
```
{% endcode %}

Now we can look at this report from two perspectives:

{% stepper %}
{% step %}
**Data Arrival Time**

As its name suggested, this it the **actual** time that the data arrives. It is calculated starting from the "input external delay" row as we have said that our input will arrive **maximumly** 1ns after the clocking edge. Then we add all the **combinational delay** and get the final data required time, which is 1.32ns.
{% endstep %}

{% step %}
**Data Required Time**

This is the time **required** for the data to arrive. In other words, the **data arrival time cannot be later than the data required time**, otherwise, we say that a timing violation happens! This value is started from the clock period and then minus the **clock uncertainty** value, and then minus the **library setup time**, which is the **real setup time** of a register/flip-flop.
{% endstep %}
{% endstepper %}

{% hint style="success" %}
In the **setup timing report**, the botton line is that **data arrival time** must be **smaller** than the data required time.
{% endhint %}

#### Hold Timing Report

Similarly, below is a **hold timing report** generated using Synopsys's PT.

{% code lineNumbers="true" %}
```
****************************************
Report : timing
        -path full
        -delay min
        -max_paths 1
Design : PRGRM_CNT
Version: P-2019.03-SP4
Date   : Wed Feb 25 11:18:40 2026
****************************************

Operating Conditions: tt1p05v125c   Library: saed32rvt_tt1p05v125c
Wire Load Model Mode: enclosed

  Startpoint: PC_reg[1] (rising edge-triggered flip-flop clocked by Clk)
  Endpoint: PC_reg[1] (rising edge-triggered flip-flop clocked by Clk)
  Path Group: Clk
  Path Type: min

  Des/Clust/Port     Wire Load Model       Library
  ------------------------------------------------
  PRGRM_CNT          ForQA                 saed32rvt_tt1p05v125c

  Point                                    Incr       Path
  -----------------------------------------------------------
  clock Clk (rise edge)                    0.00       0.00
  clock network delay (ideal)              0.00       0.00
  PC_reg[1]/CLK (DFFX1_RVT)                0.00       0.00 r
  PC_reg[1]/Q (DFFX1_RVT)                  0.11       0.11 r
  U21/Y (AO221X1_RVT)                      0.07       0.17 r
  PC_reg[1]/D (DFFX1_RVT)                  0.01       0.19 r
  data arrival time                                   0.19

  clock Clk (rise edge)                    0.00       0.00
  clock network delay (ideal)              0.00       0.00
  clock uncertainty                        0.10       0.10
  PC_reg[1]/CLK (DFFX1_RVT)                0.00       0.10 r
  library hold time                       -0.01       0.09
  data required time                                  0.09
  -----------------------------------------------------------
  data required time                                  0.09
  data arrival time                                  -0.19
  -----------------------------------------------------------
  slack (MET)                                         0.10
```
{% endcode %}

This timing report can also be seen from two perspectives:

{% stepper %}
{% step %}
**Data Arrival Time**

As we didn't set the `-min` input delay, so it's 0 here. To get this data arrival time, we start from the **contamination delay** of the combinational logic, and in this case, it is 0.19ns. So, our data arrival time is 0.19ns.
{% endstep %}

{% step %}
**Data Required Time**

To get the data required time, we start with the clock uncertainty, and the **worst-case** scenario is that this clock uncertainty will **add up to** our equivalent hold time. So, we add 0.10ns first. Then we add the **library hold time**, which represents the register's **hold time** information. In this case, it is -0.01ns, meaning that the hold time for our register is negative! So, at the end, our data arrival time is 0.09ns.

{% hint style="warning" %}
The register's **hold time** can be **negative**, meaning that the input data can safely change **before** the active clock edge.
{% endhint %}
{% endstep %}
{% endstepper %}

{% hint style="success" %}
In the **setup timing report**, the bottom line is that the **data arrival time** must be **greater** than the **data required time**.
{% endhint %}

### `set_output_delay`

THis command is used at the **output** port, to define the time it takes for the data to be available **before** the clock edge.

{% code lineNumbers="true" %}
```tcl
set_output_delay -max 19.0 -clock CLK {dataout}
```
{% endcode %}

In Figure 6-4, the output delay constraint of 19ns is specified for the signal `dataout` with respect to the clock signal `CLK`, with a 50% duty cycle and a period of 30ns. This means that the data is valid for 11ns after the clock edge.

<figure><img src="../../.gitbook/assets/set-output-delay (1).png" alt=""><figcaption><p>FIgure 6-4 Specification of Output Delay</p></figcaption></figure>

{% hint style="danger" %}
The `set_output_delay` affects our **data required time**.
{% endhint %}

#### Setup Timing Report

To see an example of how the **output external delay**, which is set by the command `set_output_delay`, affects the **data required time**, we can see the following timing report.

{% code lineNumbers="true" %}
```
****************************************
Report : timing
        -path full
        -delay max
        -max_paths 1
Design : PRGRM_CNT
Version: P-2019.03-SP4
Date   : Sat Feb 28 17:16:01 2026
****************************************

Operating Conditions: tt1p05v125c   Library: saed32rvt_tt1p05v125c
Wire Load Model Mode: enclosed

Startpoint: PC_reg[0] (rising edge-triggered flip-flop clocked by Clk)
Endpoint: PC[0] (output port clocked by Clk)
Path Group: Clk
Path Type: max

Des/Clust/Port     Wire Load Model       Library
------------------------------------------------
PRGRM_CNT          ForQA                 saed32rvt_tt1p05v125c

Point                                    Incr       Path
-----------------------------------------------------------
clock Clk (rise edge)                    0.00       0.00
clock network delay (ideal)              0.00       0.00
PC_reg[0]/CLK (DFFX1_RVT)                0.00       0.00 r
PC_reg[0]/Q (DFFX1_RVT)                  0.12       0.12 f
PC[0] (out)                              0.12       0.25 f
data arrival time                                   0.25

clock Clk (rise edge)                    1.50       1.50
clock network delay (ideal)              0.00       1.50
clock uncertainty                       -0.10       1.40
output external delay                   -1.00       0.40
data required time                                  0.40
-----------------------------------------------------------
data required time                                  0.40
data arrival time                                  -0.25
-----------------------------------------------------------
slack (MET)                                         0.15
```
{% endcode %}

In this timing report, the **output external delay** is subtracted from the clock period because the output needs to be available at least 1ns before the clock edge.

{% hint style="warning" %}
Here, we don't have the **library setup time** because this path is from **register/flip-flop** to **output port**!
{% endhint %}

### `set_clock_latency`

This command is used to define the **estimated clock insertion delay** during synthesis.

{% code lineNumbers="true" %}
```tcl
set_clock_latency 3.0 [get_clocks CLK]
```
{% endcode %}

### `set_clock_uncertainty`

This command lets the user define the **clock skew** information. Basically this is used to add a certain amount of margin to the clock, both for **setup** and **hold** times.

{% hint style="warning" %}
During the pre-layout phase one can add more margin as compared to the post-layout phase.
{% endhint %}

{% code lineNumbers="true" %}
```tcl
set_clock_uncertainty -setup 0.5 -hold 0.25 [get_clocks CLK]
```
{% endcode %}

{% hint style="success" %}
**Helpful Ideas**

It is strongly recommended that users specify a certain amount of margin both for **pre-layout** and the **post layout** phased. The main reason for doing this is to make the chip less susceptible to the process variations that may occur during manufacturing.
{% endhint %}

## Advanced Constraints

This section describes additional design constraints that go beyond the general constraints covered in the section above. These constraints consist of specifying **false paths, muticycle paths, max and min delays etc**.

{% hint style="danger" %}
The use of too many **timing exceptions**, such as false paths and multicycle paths causes significant impact on the run times.
{% endhint %}

### `set_false_path`

This command is used to instruct DC to **ignore a particular path** for timing or optimization.

{% hint style="warning" %}
Identification of false paths in a design is critical. Failure to do so, compels DC to optimize all paths in order to reduce total negative slack.
{% endhint %}

The valid **startpoint** and **endpoint** to be used for this command are the **input ports** or the **clock pins** of the **sequential elements**, and the **output ports** or the **data pins** of the **sequential cells**. In addition, one can further target a particular path using the `-through` switch.

{% code lineNumbers="true" %}
```tcl
set_false_path -from in1 -through U1/Z -to out1
```
{% endcode %}

{% hint style="success" %}
**Helpful Ideas**

Use this command when the timing critical logic is failing the static timing analysis because of the **false paths**.
{% endhint %}

### `set_multicycle_path`

This command is used to inform DC regarding **the number of clock cycles** a particular path requires in order to reach its endpoint.

{% hint style="warning" %}
DC automatically assumes that all paths are single cycle paths and will unnecessarily try to optimize the multicycle segment in order to achieve the timing. This may have a direct impact on adjacent paths as well as the area.
{% endhint %}

Also, the command provides the `-through` option that facilitates isolating the multicycle segment in a design.

{% code lineNumbers="true" %}
```tcl
set_multicycle_path 2 -from U1/Z -through U2/A -to out1
```
{% endcode %}

### `set_max_delay`

This command defines the **maximum delay** required in terms of time units for a particular **path**. In general, it is used for the blocks that contain combinational logic only.

{% hint style="danger" %}
This command has the precedence over [**DC derived timing requirements**](https://wenbo-notes.gitbook.io/ee4415-icd-notes/textbook-2-aacs/constraining-designs/environment-and-constraints#data-required-time)!
{% endhint %}

For blocks only containing combinational logic, one may use the following command to constrain the total delay from all inputs to all outputs, as shown below:

{% code lineNumbers="true" %}
```tcl
set_max_delay 5 -from [all_inputs] -to [all_outputs]
```
{% endcode %}

### `set_min_delay`

This command is the **opposite** of the [#set\_max\_delay](environment-and-constraints.md#set_max_delay "mention") command, and is used to define the **minimum delay** required in terms of time units for a particular **path**.

{% hint style="danger" %}
This command also has the precedence over [**DC derived timing requirements**](https://wenbo-notes.gitbook.io/ee4415-icd-notes/textbook-2-aacs/constraining-designs/environment-and-constraints#hold-timing-report)!
{% endhint %}

### `group_path`

This command is used to **bundle** together timing critical paths in a design, for cost function calculations.

{% hint style="warning" %}
DC prioritizes the **grouped paths** over other paths in the design.
{% endhint %}

{% code lineNumbers="true" %}
```tcl
group_path -to [list out1 out2] -name grp1
```
{% endcode %}

{% hint style="danger" %}
`group_path` is **not the same as** [`group`](../partitioning-and-coding-styles/partitioning-for-synthesis.md#group-command) and [`ungroup`](../partitioning-and-coding-styles/partitioning-for-synthesis.md#ungroup-command)!
{% endhint %}
