# Synopsys Design Fundamentals

This part will be pretty useful for the `Tcl` scripting in Synopsys tools.

## Objects, Variables and Attributes

Synopsys supports a number of **objects**, **variables** and **attributes** in order to streamline the synthesis process. Using these, designers can write powerful **scripts** to automate the synthesis process. It is therefore essential for designers to familiarize themselves with these terms.

### Design Objects

There are eight different types of design objects categorized by DC. These are:

{% stepper %}
{% step %}
#### Design

It corresponds to the **circuit description** that performs some logical function. The design may be stand-alone or may include other sub-designs.

{% hint style="warning" %}
Although sub-designs may be part of the design, it is treated as another design by Synopsys.
{% endhint %}
{% endstep %}

{% step %}
#### Cell

It is the **instantiated** **name** of the **sub-design** in the design. In Synopsys terminology, there is no differentiation between the **cell** and **instance**; both are treated as cell.

{% hint style="warning" %}
Think of the module instantiation in Verilog.
{% endhint %}
{% endstep %}

{% step %}
#### Reference

This is the **definition** of the **original design** to which the cell or instance refers. For example, a leaf cell in the netlist must be referenced from the link library, which contains the functional description of the cell. Similarly, a sub-design instantiated (called **cell** by Synopsys) must be referenced in the design, which contains functional description of the instantiated sub-design.
{% endstep %}

{% step %}
#### Port

These are the primary **inputs, outputs** or **IO's** of the **design**.
{% endstep %}

{% step %}
#### Pin

It corresponds to the **inputs, outputs** or **IO's** of the **cells** in the design.
{% endstep %}

{% step %}
#### Net

These are the **signals names**, e.g., the wires that hook up the design together connecting by ports to pins and/or pins to each other.
{% endstep %}

{% step %}
#### Clock

The **port or pin** that is identified as a **clock source**. The identification may be internal to the library or it may be done using `dc_shell-t` commands.
{% endstep %}

{% step %}
#### Library

Corresponds to the collection of **technology specific cells** that the design is targeting for synthesis; or linking for reference.
{% endstep %}
{% endstepper %}

### Variables

**Variables** are placeholders used by DC for the purpose of storing information. Some variables are pre-defined by DC and may be used by the designer to obtain the current value stored in the variable.

{% hint style="danger" %}
All variables are **global** and **last only during the session**.
{% endhint %}

A list of all DC variables may be obtained by using the following DC command:

```tcl
printvar *
```

### Attributes

**Attributes** are similar in nature to [variables](synopsys-design-fundamentals.md#variables). Both store information. However, attributes store information on a **particular design object** such as **nets, cells** or **clocks**.

Attributes are set on, retrieved and removed from the design object by using the following commands.

{% code lineNumbers="true" %}
```tcl
set_attribute    <object_list>
                 <attribute_name>
                 <attribute_value>

get_attribute    <object_list>
                 <attribute_name>

remove_attribute <attribute_name>
```
{% endcode %}

## Finding Design Objects

One of the most useful commands provided by DC & PT is the `get_*` command. The `get_*` commands are used to locate a **list of designs** or **library objects** in DC. Several types of `get_*` commands are provided. Examples are:

```tcl
get_ports, get_nets, get_designs, get_lib_cells, get_cells, get_clocks, ...
```

{% hint style="success" %}
A full list of `get` commands may be found by typing `help get_*` in the `dc_shell-t` command line.
{% endhint %}

## Synopsys Format

Most synopsys products support and share a common internal structure called the `db` format. The `db` files are the **binary compiled forms** representing the **text data**, be it

1. the RTL code,
2. the mapped gate-level designs, or
3. the Synopsys library itself.

The `db` files may also contain any **constraints** that have been applied to the design.

## Data Organization

It is a good practice to organize files according to their formats. This facilitates automating the synthesis process. A common practice is to organize them using the following file extensions.

{% code lineNumbers="true" %}
```tcl
Script file:                    <filename>.scr
RTL Verilog file:               <filename>.v
Synthesized Verilog netlist:    <filename>.sv
RTL VHDL file:                  <filename>.vhd
Synthesized VHDL netlist:       <filename>.svhd
EDIF file:                      <filename>.edf
Synopsys database file:         <filename>.db
Report file:                    <filename>.rpt
Log file:                       <filename>.log
```
{% endcode %}

{% hint style="danger" %}
At the point of time this textbook is written, **SystemVerilog** (with file extension of `.sv`) is not popular. Thus, in this textbook, `.sv` extension is used to synthesized verilog netlist.
{% endhint %}

## Design Entry

Before sythesis, the design must be entered in DC in the RTL format (although other formats also exist). DC provides the following two methods of design entry.

1. `read` command.
2. `analyze/elaborate` command.

In EE4415, we will only use `read` command and one example for reading the Verilog files is:

```tcl
read_verilog prgrm_cnt.v
```
