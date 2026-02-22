# Compiler Directives

Sometimes it is necessary to control the **synthesis process** from the **HDL source** itself. This control is primarily needed because of the following three reasons:

1. the difference that may exist between the synthesis and the simulation environments.
2. to direct DC to map to **certain types** of components.
3. for embedding the **constraints** and **attributes** directly in the HDL source code.

DC provides a number of **compiler directives** targeted specifically for Verilog and VHDL design entry formats. The **directives** are specified as "comments" in the HDL code, but have specific meaning for DC. These special comments alter the **synthesis process**, but have no effect on the **simulation**.

{% hint style="success" %}
For a complete list of directives, users are advised to refer to the Design Compiler Reference Manual.
{% endhint %}

## HDL Compiler Directives

The HDL compiler directives refer to the translation process of RTL in Verilog format to the internal format used by Design Compiler. These directives are used to **hide simulation only constructs**.

{% hint style="success" %}
With the Synopsys tool chain, we can use `// synopsys <directives>` in the HDL code to apply the HDL compiler directives.
{% endhint %}

### `translate_off` and `translate_on` Directives

These are some of the most useful and frequently used directives. They provide the means to instruct DC to **stop translation** of the Verilog source code from the **start of** `// synopsys translate_off`, and **start** the translation again after it reaches the next directive, `// synopsys translate_on`.

{% hint style="danger" %}
These directives must be used in pairs, with the `translate_off` directive taking the lead.
{% endhint %}
