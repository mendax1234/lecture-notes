# Synopsys Products

This section briefly describes all relevant Synopsys products related to this book.

1. Library Compiler
2. Design Compiler and Design Vision
3. Physical Compiler
4. PrimeTime
5. DFT Compiler
6. Formality

## Library Compiler

The core of any ASIC design is the **technology library** containing **a set of logic cells**. The library may contain **functional description, timing, area** and other pertinent information of each cell. Library Compiler (LC) parses this textual information for completeness and correctness, before converting it to a format, used globally by all Synopsys applications.

{% hint style="success" %}
Library Compiler is invoked by typing `lc_shell` in a UNIX shell. All the capabilities of the LC can also be utilized within `dc_shell`.
{% endhint %}

## Design Compiler and Design Vision

The Synopsys **Design Compiler** (DC) and **Design Vision** (DV) comprise a powerful suite of logic synthesis products, designed to provide an optimal gate-level synthesized netlist based on

* the design specifications, and
* timing constraints.

In addition to high-level synthesis capabilities, it also incorporates a static timing analysis engine, along with solutions for FPGA synthesis and links-to-layout (LTL).

### Design Compiler

Design Compiler is the **command line interface** of Synopsys synthesis tool and is invoked by either typing `dc_shell` or `dc_shell-t` in a UNIX shell.

* The `dc_shell` is the original format that is based on Synopsys’s own language
* while `dc_shell-t` uses the standard Tcl language.

This book focuses only on the Tcl version of DC because of the commonality with other Synopsys tools, like PrimeTime.

### Design Vision

The **Design Vision** is the **graphical front-end** version of DC and is launched by typing `design_vision`. Design Vision also supports schematic generation, with critical path analysis through point-to-point highlighting.

{% hint style="success" %}
Although, beginners may initially prefer using DV, they quickly migrate to using DC, as they become more familiar with Synopsys commands.
{% endhint %}

## Physical Compiler

**Physical Compiler** (or PhyC) is a new tool by Synopsys that is a superset of DC. In addition to incorporating all the synthesis and optimization capabilities of DC, it also provides the ability to **concurrently place cells optimally**, based on the timing and/or area constraints of the design.

PhyC is invoked by typing `psyn_shell`. A separate GUI version is also available, which is launched by typing `psyn_gui`. Although slow by comparison, `psyn_gui` provides the users the ability to traverse between the logical and the schematic view of the design.

{% hint style="warning" %}
It must be noted that PhyC being the superset of DC, all `dc_shell` commands are available within `psyn_shell`. The reverse is not true. You cannot use `psyn_shell` commands within `dc_shell`.
{% endhint %}

## PrimeTime

**PrimeTime** (PT) is the Synopsys sign-off quality, full chip, gate-level static timing analysis tool. In addition, it also allows for comprehensive modeling capabilities, often required by large designs.

PT is a **stand-alone** tool and can be invoked as a command line interface or graphically.

* To use the command line interface, type `pt_shell` in the UNIX window, or
* type `primetime` for the graphical version.

<details>

<summary>PrimeTime vs. the internal static timing analysis tool in DC</summary>

PT is **faster** compared to DC's internal static timing analysis engine. It also provides enhanced analysis capabilities, both textually and graphically. In contrast to the rest of Synopsys tools, this tool is Tcl language based, therefore providing powerful features of that language to promote the analysis and debugging of the design.

</details>

## DFT Compiler

The **DFT Compiler** (DFTC) is the Synopsys test insertion tool that is incorporated within the DC suite of tools. The DFTC is used to insert DFT features like **scan insertion** and **boundary scan**, to the design. All DFTC commands are directly invoked from `dc_shell` or `psyn_shell`.

## Formality

**Formality** is the Synopsys formal verification or more precisely a **logic equivalence checking tool**. The tool features enhanced graphical debugging capabilities that include schematic representation of logic under verification, and visual suggestions annotated to the schematic as pointers of possible incorrect logic. It also provides suggestions for possible fixes to the design.
