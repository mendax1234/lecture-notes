---
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/Sp0XaarBjbEX3JIMrRaR/textbook-2-synopsys/synopsys-technology-library/technology-libraries
---

# Technology Libraries

The Synopsys **technology libraries** can be separated in two broad classes:

1. Logic library
2. Physical library

## Logic Library

The **logic library** contains information relevant only to the **synthesis process** and is used by **DC** for **synthesis** and **optimization of the design**. This information may include

1. pin-to-pin timing,
2. area, pin types and
3. power along with other necessary data needed by DC.

{% hint style="warning" %}
No physical information is present in the logic library.
{% endhint %}

The logic library is a text file (usually with extension `.lib`), which is compiled using the Library Compiler (LC) to generate a binary format with [`.db`](../basic-concepts/synopsys-design-fundamentals.md#synopsys-format) extension.

## Physical Library

The **physical library** contains the physical characteristics of the cell along with other necessary information relevant to **Physical Compiler**. Such information may contain

1. data relating to the physical dimensions of cells,
2. layer information,
3. orientation of cells etc.

{% hint style="warning" %}
For each **logical cell**, a corresponding **physical cell** should also be present.
{% endhint %}

The physical library is also a text file (usually with extension `.plib`) and is compiled by LC to generate a binary format with a `.pdb` extension.
