# Logic Library Basics

The logic library contains the following information:

1. Library group
2. Library level attributes
3. Environment description
4. Cell description

{% hint style="success" %}
All this information is in the `.lib` file of the technology library!
{% endhint %}

## Library Group

The **library group statment** specifies the name of the library, followed by an open brace and a closing brace. Anything between the open and closing brace, is part of the entire library group description.

{% code overflow="wrap" lineNumbers="true" %}
```verilog
library (ex25) {  /* start of library */
...
    <library description>
...
}  /* end of library */
```
{% endcode %}

{% hint style="success" %}
It is recommended that the file name and the technology library name be the same.
{% endhint %}

## Library Level Attributes

The **library level attributes** are statements that apply to library as a whole. These generally contain library features such as

* Technology type,
* date,
* revision, and
* default values that apply to the entire library

{% code overflow="wrap" lineNumbers="true" %}
```verilog
library (ex25) {

    technology               (cmos);
    delay_model              : table_lookup;
    date                     : "Feb 29,2000";
    revision                 : "1.0";
    current_unit             : "1A";
    time_unit                : "1 ns";
    voltage_unit             : "1V";
    pulling_resistance_unit  : "1kohm";

    capacitive_load_unit     (1.0, pf);

    default_inout_pin_cap    : 1.5;
    default_input_pin_cap    : 1.0;
    default_output_pin_cap   : 0.0;

    default_max_fanout       : 10.0;
    default_max_transition   : 3.0;

    default_operating_conditions : NOMINAL;
    in_place_swap_mode           : match_footprint;
    .....
    .....
}
```
{% endcode %}

{% hint style="success" %}
Note that the **time unit** of the technology library is also given here! This may be useful for EE4415 Lab 01.
{% endhint %}

## Environment Description

**Environment attributes** are defined in the library to model the variations temperature, voltage and manufacturing processes. These consist of

* scaling factors (derating),
* timing range models, and
* operation conditions

In addition, the environment description also contains **wire-load models** that are used by DC to estimate interconnect wiring delays.

### Scaling Factors

The **scaling factors** or **K-factors** are multipliers that provide means for derating the delay values based on the variations in

* process,
* voltage and
* temperature, or simply PVT.

{% code overflow="wrap" lineNumbers="true" %}
```verilog
k_process_fall_transition     : 1.0;
k_process_rise_transition     : 1.2;

k_process_fall_propagation    : 0.4;
k_process_rise_propagation    : 0.4;

k_temp_fall_transition        : 0.03;
k_temp_rise_transition        : 0.04;

k_temp_fall_propagation       : 1.2;
k_temp_rise_propagation       : 1.24;

k_volt_fall_transition        : 0.02;
k_volt_rise_transition        : 0.5;

k_volt_fall_propagation       : 0.9;
k_volt_rise_propagation       : 0.85;
```
{% endcode %}

{% hint style="warning" %}
Only some of the K-factor statements are shown below as an example. Please refer to the library compiler reference manual for full details.
{% endhint %}

### Operating Conditions

Sets of **operating conditions** defined in the library specify

* the process,
* temperature,
* voltage and
* the RC tree model.

These are used during **synthesis** and **timing analysis** of the design. A library is characterized using one set of operating conditions.

{% hint style="success" %}
During synthesis or timing analysis, if another set of operating conditions is specified, then DC uses the [K-factors](logic-library-basics.md#scaling-factors) to derate the delay values based upon the specified operating conditions.
{% endhint %}

Typically the following operating conditions are defined in the technology library:

{% code overflow="wrap" lineNumbers="true" %}
```verilog
operating_conditions (WORST) {
    process     : 1.3;
    temperature : 100.0;
    voltage     : 2.75;
    tree_type   : worst_case_tree;
}

operating_conditions (NOMINAL) {
    process     : 1.0;
    temperature : 25.0;
    voltage     : 3.00;
    tree_type   : balanced_tree;
}

operating_conditions (BEST) {
    process     : 0.7;
    temperature : 0.0;
    voltage     : 3.25;
    tree_type   : best_case_tree;
}
```
{% endcode %}

The process, temperature and voltage attributes have already been explained previously. The `tree_type` attribute defines the environmental interconnect model to be used.

{% hint style="warning" %}
TODO: Complete this after taking EE4415 second half.
{% endhint %}

### Timing Range Models

The `timing_range` models provide additional capability of computing **arrival times** of signals, based upon the specified operating conditions.

{% code overflow="wrap" lineNumbers="true" %}
```verilog
timing_range (BEST) {
    faster_factor : 0.5;
    slower_factor : 0.6;
}

timing_range (WORST) {
    faster_factor : 1.2;
    slower_factor : 1.3;
}
```
{% endcode %}

### Wire-Load Models

The `wire_load` group contains information that DC utilizes to estimate **interconnect wiring delays** during the **pre-layout** phase of the design. Usually, several models appropriate to different sizes of the logic are included in the technology library. These models define the **capacitance**, **resistance** and **area** factors. In addition, the `wire_load` group also specifies **slope** and `fanout_length` for the logic under consideration.

The **capacitance**, **resistance** and **area** factors represent the wire **resistance**, **capacitance** and **area** respectively, **per unit length of interconnect wire**. The `fanout_length` attribute specifies values for the **length** of the wire associated with the **number of fanouts**. For example,

{% code overflow="wrap" lineNumbers="true" %}
```verilog
fanout_length(<num_of_fanout>, <length>
```
{% endcode %}

For nets exceeding the longest length specified in the `fanout_length` attribute, the **slope** value is used to linearly interpolate the existing `fanout_length` value, in order to determine its value.

{% code overflow="wrap" lineNumbers="true" %}
```verilog
wire_load (SMALL) {
    resistance : 0.2;
    capacitance: 1.0;
    area       : 0;
    slope      : 0.5;

    fanout_length (1,    0.020);
    fanout_length (2,    0.042);
    fanout_length (3,    0.064);
    fanout_length (4,    0.087);
    ....
    fanout_length (1000, 20.0);
}

wire_load (MEDIUM) {
    resistance : 0.2;
    capacitance: 1.0;
    area       : 0;
    slope      : 1.0;

    fanout_length (1,    0.022);
    fanout_length (2,    0.046);
    fanout_length (3,    0.070);
    fanout_length (4,    0.095);
    ....
    fanout_length (1000, 30.0);
}

wire_load (LARGE) {
    resistance : 0.2;
    capacitance: 1.0;
    area       : 0;
    slope      : 1.5;

    fanout_length (1,     0.025);
    fanout_length (2,     0.053);
    fanout_length (3,     0.080);
    fanout_length (4,     0.110);
    ....
    fanout_length (1000, 40.0);
}
```
{% endcode %}

In addition to the `wire_load` groups, other attributes are defined in the library to **automatically** select the appropriate `wire_load` group, based on the total **cell area** of the logic under consideration.

{% code overflow="wrap" lineNumbers="true" %}
```verilog
wire_load_selection (AUTO_WL) {

    wire_load_from_area (0,     5000,  "SMALL");
    wire_load_from_area (5000,  10000, "MEDIUM");
    wire_load_from_area (10000, 15000, "LARGE");
}

default_wire_load_selection : AUTO_WL;
default_wire_load_mode      : enclosed;
```
{% endcode %}

{% hint style="warning" %}
It is recommended to set the `default_wire_load_mode` to `enclosed` and the reasons will be introduced later.
{% endhint %}

## Cell Descrption

Each **cell** in the library contains a variety of attributes describing the function, timing and other information related to each cell.

{% code overflow="wrap" lineNumbers="true" %}
```verilog
cell (BUFFD0) {
    area : 5.0;

    pin (Z) {
        direction        : output;
        function         : "I";
        max_capacitance  : 2.2;
        max_fanout       : 4.0;
        related_pin      : "I";

        timing () {
        }

        timing () {
        }
    }

    pin (I) {
        direction      : input;
        capacitance    : 0.04;
        fanout_load    : 2.0;
        max_transition : 1.5;
    }
}
```
{% endcode %}

And the technology library file `.lib` is actuall a text file containing thounsands of these `cell` blocks.

<details>

<summary>A real-world example of the <code>INVX1_RVT</code> provided by Synopsys</summary>

{% code lineNumbers="true" %}
```verilog
cell (INVX1_RVT) {
    cell_footprint        : "AV";
    area                  : 1.27072;
    cell_leakage_power    : 4.117191e+05;
    driver_waveform_rise  : "preDrv";
    driver_waveform_fall  : "preDrv";

    leakage_power () {
        when  : "A";
        value : "4.1171910e+05";
    }

    pg_pin (VDD) {
        voltage_name : "VDD";
        pg_type      : "primary_power";
    }

    pg_pin (VSS) {
        voltage_name : "VSS";
        pg_type      : "primary_ground";
    }

    pin (A) {
        direction              : "input";
        fanout_load            : 0.149000;
        related_power_pin      : "VDD";
        related_ground_pin     : "VSS";

        fall_capacitance       : 0.712448;
        fall_capacitance_range (0.518941, 0.785358);

        rise_capacitance       : 0.728932;
        rise_capacitance_range (0.522198, 0.795257);

        capacitance            : 0.720690;
        max_transition         : 1.024000;
    }

    pin (Y) {
        direction              : "output";
        related_power_pin      : "VDD";
        related_ground_pin     : "VSS";

        output_voltage         : "DC_0";
        power_down_function    : "!VDD + VSS";
        function               : "(A)'";

        max_capacitance        : 8.000000;
        max_transition         : 0.435998;

        internal_power () {
            related_pin : "A";

            rise_power ("power_outputs_1") {
                index_1 ("0.016, 0.032, 0.064, 0.128, 0.256, 0.512, 1.024");
                index_2 ("0.1, 0.25, 0.5, 1, 2, 4, 8");
                values (
                    "1.5015251, 1.4940728, 1.4835776, 1.4730004, 1.4464974, 1.4255433, 1.4244439",
                    "1.7179029, 1.6955728, 1.6568999, 1.6090253, 1.5545881, 1.4928043, 1.4809102",
                    "2.3127857, 2.2640140, 2.1945584, 2.0778936, 1.9086154, 1.7312877, 1.5869129",
                    "3.5850234, 3.5231523, 3.4306935, 3.2603675, 2.9543064, 2.5336931, 2.1479972",
                    "6.2049603, 6.1313869, 6.0214673, 5.8143970, 5.4225002, 4.7742183, 3.8129430",
                    "11.4605020, 11.3977488, 11.2785296, 11.0481709, 10.5762035, 9.7692991, 8.3919080",
                    "22.0584346, 21.9642145, 21.8461629, 21.5175809, 21.1183262, 20.1506851, 18.5581159"
                );
            }

            fall_power ("power_outputs_1") {
                index_1 ("0.016, 0.032, 0.064, 0.128, 0.256, 0.512, 1.024");
                index_2 ("0.1, 0.25, 0.5, 1, 2, 4, 8");
                values (
                    "0.0914703, 0.0885395, 0.0826772, 0.0767556, 0.0693194, 0.0611429, 0.0440872",
                    "0.2641618, 0.2415499, 0.2119845, 0.1756339, 0.1357591, 0.1030426, 0.0728078",
                    "0.7574968, 0.7144648, 0.6440022, 0.5311311, 0.4024765, 0.2763580, 0.1873484",
                    "1.8988170, 1.8330583, 1.7284754, 1.5420229, 1.2551456, 0.8906185, 0.5699127",
                    "4.2729073, 4.1906010, 4.0662635, 3.8289150, 3.4075209, 2.7409003, 1.9042363",
                    "9.0522550, 8.9830148, 8.8629415, 8.5636793, 8.0730658, 7.1779156, 5.7500407",
                    "18.6328124, 18.5670496, 18.4333724, 18.1493909, 17.6237739, 16.5774166, 14.7318788"
                );
            }
        }

        timing () {
            related_pin  : "A";
            timing_type  : "combinational";
            timing_sense : "negative_unate";

            cell_rise ("del_1_7_7") {
                index_1 ("0.016, 0.032, 0.064, 0.128, 0.256, 0.512, 1.024");
                index_2 ("0.1, 0.25, 0.5, 1, 2, 4, 8");
                values (
                    "0.0087797, 0.0096045, 0.0108794, 0.0131400, 0.0169748, 0.0236194, 0.0371733",
                    "0.0109816, 0.0120270, 0.0136279, 0.0165745, 0.0216680, 0.0298900, 0.0433899",
                    "0.0143731, 0.0155586, 0.0176638, 0.0212904, 0.0277878, 0.0384449, 0.0556286",
                    "0.0197840, 0.0210437, 0.0234058, 0.0279143, 0.0359690, 0.0495899, 0.0719645",
                    "0.0288819, 0.0306204, 0.0332002, 0.0382667, 0.0482474, 0.0649652, 0.0930464",
                    "0.0469482, 0.0486424, 0.0513830, 0.0566867, 0.0682595, 0.0877132, 0.1226612",
                    "0.0828885, 0.0837571, 0.0881600, 0.0941400, 0.1043134, 0.1262775, 0.1691171"
                );
            }

            rise_transition ("del_1_7_7") {
                index_1 ("0.016, 0.032, 0.064, 0.128, 0.256, 0.512, 1.024");
                index_2 ("0.1, 0.25, 0.5, 1, 2, 4, 8");
                values (
                    "0.0101055, 0.0107737, 0.0117391, 0.0133749, 0.0174729, 0.0271560, 0.0481203",
                    "0.0164472, 0.0170985, 0.0182157, 0.0204043, 0.0244327, 0.0318271, 0.0509637",
                    "0.0289770, 0.0295816, 0.0308705, 0.0330383, 0.0378907, 0.0461321, 0.0607638",
                    "0.0532195, 0.0545638, 0.0554163, 0.0581748, 0.0627347, 0.0723246, 0.0894182",
                    "0.1023724, 0.1033617, 0.1041137, 0.1069773, 0.1115324, 0.1226965, 0.1409578",
                    "0.2015217, 0.2019733, 0.2031752, 0.2054014, 0.2098910, 0.2195635, 0.2405778",
                    "0.4005276, 0.4016667, 0.4032785, 0.4061975, 0.4079644, 0.4160230, 0.4359979"
                );
            }

            cell_fall ("del_1_7_7") {
                index_1 ("0.016, 0.032, 0.064, 0.128, 0.256, 0.512, 1.024");
                index_2 ("0.1, 0.25, 0.5, 1, 2, 4, 8");
                values (
                    "0.0077469, 0.0087326, 0.0102427, 0.0129549, 0.0176107, 0.0259728, 0.0428904",
                    "0.0076055, 0.0087916, 0.0106520, 0.0141396, 0.0201114, 0.0300807, 0.0469022",
                    "0.0061788, 0.0076340, 0.0099588, 0.0140301, 0.0214756, 0.0343527, 0.0548993",
                    "0.0021361, 0.0038363, 0.0063881, 0.0115308, 0.0203311, 0.0362797, 0.0622572",
                    "-0.0067170, -0.0051545, -0.0022649, 0.0034163, 0.0140264, 0.0332138, 0.0651792",
                    "-0.0248120, -0.0230865, -0.0202635, -0.0142676, -0.0027555, 0.0191153, 0.0574120",
                    "-0.0605867, -0.0588637, -0.0559027, -0.0500634, -0.0389367, -0.0158952, 0.0285694"
                );
            }

            fall_transition ("del_1_7_7") {
                index_1 ("0.016, 0.032, 0.064, 0.128, 0.256, 0.512, 1.024");
                index_2 ("0.1, 0.25, 0.5, 1, 2, 4, 8");
                values (
                    "0.0105007, 0.0113758, 0.0128255, 0.0152280, 0.0206336, 0.0327374, 0.0590534",
                    "0.0158926, 0.0167751, 0.0182678, 0.0212353, 0.0268184, 0.0371553, 0.0606883",
                    "0.0262478, 0.0272555, 0.0288274, 0.0320091, 0.0380664, 0.0496057, 0.0702877",
                    "0.0476128, 0.0482549, 0.0497548, 0.0529413, 0.0596539, 0.0720721, 0.0949232",
                    "0.0918026, 0.0923387, 0.0929549, 0.0949519, 0.1014364, 0.1142587, 0.1397202",
                    "0.1822672, 0.1822672, 0.1822672, 0.1825431, 0.1870756, 0.1977289, 0.2244353",
                    "0.3621092, 0.3621092, 0.3621092, 0.3621092, 0.3627709, 0.3715667, 0.3910986"
                );
            }
        }
    }
}
```
{% endcode %}

</details>

### Area

The `area` attribute defines the **cell area** as a floating point number without any units followed by pin description and their related timing.

### Design Rule Checking (DRC)

In addition, several design rule checking (DRC) attributes may be associated with **each pin** of the cell. These are:

1. `fanout_load` attribute for inputs pins.
2. `max_fanout` attribute for output pints.
3. `max_transition` attribute for input or output pins.
4. `max_capacitance` attribute for output or inout pins.

The DRC conditions are based on the vendor’s process technology and should not be violated. The DRC attributes define the conditions in which the cells of the library operate safely.

{% hint style="danger" %}
Designs violating these conditions may have a severe impact on the normal operation of the cells, thereby causing the fabricated chip to fail.
{% endhint %}

#### `fanout_load` and `max_fanout`

`fanout_load` represents the abstract "cost" or "burden" that this specific input pin places on the net while `max_fanout` represents the maximum total `fanout_load` that this output pin is legally allowed to drive. Thus, the `max_fanout` value at the output of the driver pin cannot exceed the sum of all `fanout_load` values at each input pin of the driven cells.

Consider the example of `BUFFD0` above, this cell contains a `max_fanout` value of 4.0 associated to the output pin Z, while the `fanout_load` value at its input is 2.0. This cell therefore, **cannot** drive more the **2** of its own kind (`BUFFD0`), since

$$
\text{max_fanout}(4)=\text{fanout_load}(2)\text{ of } 1{\text{st}}\text{ cell }+\text{fanout_load}(2)\text{ of }2{\text{nd}}\text{ cell }
$$

{% hint style="danger" %}
If the DRC violations occur, then DC replaces the driving cell with another that has a higher `max_fanout` value.
{% endhint %}

#### `max_transition` and `max_capacitance`

`max_transition` represents the maximum allowable time for a signal to switch between logic states (from 0 to 1, or 1 to 0) while `max_capacitance` the absolute maximum physical capacitance that a specific output pin is legally allowed to drive.

The `max_transition` attribute defines that any net that has a transition time **greater than** the specified `max_transition` value of the load pin, cannot be connected to that pin. The `max_capacitance` at the output pin specifies that the output pin of the driver cell cannot connect to any net that has the **total capacitance** (interconnect and load pin capacitance) greater than, or equal to the maximum value defined at the output pin.

{% hint style="danger" %}
If DRC violations occur, then DC replaces the driving cell with another that has a higher `max_capacitance` value.
{% endhint %}

<details>

<summary>Change the DRC attributes</summary>

The cell's DRC attributes are often the most criticized part of the cell library. In order to accommodate the design requirements, it is possible to change the values of the above DRC attributes on a per cell basis. However, it must be noted that the DRC attributes set in the library can only be tightened, they cannot be loosened.

{% hint style="danger" %}
This can only be done, if the attributes are **pre-specified** in the cell description. Users should realize that if these attributes are not already specified on the pin of the cell in the technology library, it is not be possible to add these attributes on the pin, from `dc_shell`.
{% endhint %}

For instance, to change the `max_fanout` value specified for pin Z of cell `BUFFD0` (of library `ex25` described previously), from 4.0 to 2.0; the following `dc_shell-t` command may be used:

{% code overflow="wrap" lineNumbers="true" %}
```shellscript
set_attribute [get_pins ex25/BUFFD0/Z] max_fanout 2
```
{% endcode %}

</details>

### Other attributes

In addition, the output pin contains attributes defining the **function of the pin**, and the **delay values related to the input pin** (the **capacitance** in the input pin).

{% hint style="warning" %}
The capacitance attribute should not be confused with the `max_capacitance` attribute. DC uses the capacitance attribute to perform delay calculations only, while the `max_capacitance`, as explained above, is used for design rule checking.
{% endhint %}
