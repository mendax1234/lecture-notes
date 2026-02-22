# Synthesis Environment

As with most EDA products, Synopsys tools require a **setup file** that specifies the technology library location and other parameters used for synthesis. Synopsys also defines its own format for storing and processing the information. This section highlights such details.

## Startup Files

There is a common startup file called `.synopsys_dc.setup` for **all tools of the DC and PhyC family**. A separate startup file is required for PT which is named `.synopsys_pt.setup`. These files are in Tcl format, and contain **path information** to

* the technology libraries and
* other environment variables.

The **default** startup files for PhyC, DC and PT reside in the Synopsys installation directory, and are automatically loaded upon invocation of these tools. These default files do not contain the design dependent data. Their function is to load the Synopsys **technology independent libraries** and other parameters. The user in the startup files specifies the design dependent data. During startup, these tools read the files in the following order:

1. Synopsys installation directory.
2. Users home directory.
3. Project working directory.

The settings specified in the startup files, residing in the project working directory, override the ones specified in the home directory and so forth, i.e., the configuration specified in the project working directory takes precedence over all other settings.

{% hint style="warning" %}
It is up to the discretion of the user to keep these files wherever it is convenient. However, it is recommended that the design dependent startup files be kept in the **working directory**.
{% endhint %}

The typical start files for DC and PT are shown as follows

{% tabs %}
{% tab title="Example 3.1: Setup Files (DC)" %}
{% code title=".synopsys_dc.setup" lineNumbers="true" %}
```tcl
# Search path
set search_path [list \
    /usr/golden/library/std_cells \
    /usr/golden/library/pads \
]

# Target library
set target_library [list std_cells_lib.db]

# Physical library
set physical_library [list \
    std_cells_lib.pdb \
    pad_lib.pdb \
]

# Link library
set link_library [list {*} \
    std_cells_lib.db \
    pad_lib.db \
]

# Symbol library
set symbol_library [list \
    std_cells_lib.sdb \
    pad_lib.sdb \
]
```
{% endcode %}
{% endtab %}

{% tab title="Example 3.1: Setup Files (PT)" %}
{% code title=".synopsys_pt.setup" lineNumbers="true" %}
```tcl
# Search path
set search_path [list \
    /usr/golden/library/std_cells \
    /usr/golden/library/pads \
]

# Link path
set link_path [list {*} \
    std_cells_lib.db \
    pad_lib.db \
]
```
{% endcode %}
{% endtab %}
{% endtabs %}

## System Library Variables

In the setup files, we have seen a lot of libraries, it's time to explain what each of the library is used for.

### Target Library

The `target_library` specifies the name of the technology library that corresponds to the library whose cells the designers want DC to infer and finally map to.

{% hint style="success" %}
It will be good to review the [logic synthesis](../../lecture/lec-03/lec-03a-digital-design-flow.md#logic-synthesis-1) process from EE4415 Lec 03a! The logic synthesis includes:

1. Translating the HDL code into **generic boolean** format (called GTECH)
2. This GTECH is then optimized and mapped to the specific gates in the **technology library**.
{% endhint %}

### Link Library

The `link_library` defines the name of the library that refers to the library of cells used solely for reference, i.e., cells in the `link_library` are not inferred by DC.

<details>

<summary>Example of using target library and link library</summary>

For example, you may specify a **standard cell technology** library as the `target_library`, while specifying the **pad technology library** name and **all other macros** (RAMs, ROMs etc.) in the `link_library` list. This means that the user would synthesize the design that targets the cells present in the standard cell library, while **linking** to the pads and macros that are instantiated in the design.

{% hint style="info" %}
If the pad library is included in the `target_library` list, then DC may use the pads to synthesize the core logic.
{% endhint %}

</details>

The **target library name** should also be included in the `link_library` list, as shown in [Example 3.1](synthesis-environment.md#example-3.1-setup-files-dc). This is important while reading the **gate-level netlist** in DC. DC will not be able to link to the mapped cells in the netlist, if the target library name is not included in the link library list.

#### Remapping of the cells

The **target library** and **link library** provide a useful means to **remap** a gate-level netlist from **one technology** to the other. In this case

* The `link_library` may contain the **old technology library** name, while
* The `target_library` may contain the **new technology library**.

Remapping can be performed by the `translate` command in `dc_shell`.

### Symbol Library

The `symbol_library` system variable holds the **name of the library**, containing **graphical** representation of the **cells** in the **technology library**.

### Physical Library

In addition to specifying logical libraries, **physical libraries** also need to be identified if using `psyn_shell`. These libraries contain the **physical information** (like physical dimensions of cells, orientation, layer information etc.) needed by PhyC. The physical libraries are identified with a “pdb” extension and are referenced by the `physical_library` system variable as shown in the [above example](synthesis-environment.md#example-3.1-setup-files-dc).
