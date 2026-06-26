---
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/Sp0XaarBjbEX3JIMrRaR/textbook-2-synopsys/partitioning-and-coding-styles/partitioning-for-synthesis
---

# Partitioning for Synthesis

**Partitioning** can be viewed as, utilizing the "divide and conquer" concept to reduce complex designs into simpler and manageable blocks. Promoting design reuse is one of the most significant advantages to partitioning the design.

## Group and Ungroup Commands

The `group` and `ungroup` DC commands provide the designer with capability of altering the partitions in DC, after the **design hierarchy** has already been defined by the previously written HDL code.

### Group Command

The `group` command combines the specified instances into a separate block. In Figure 5-1, instances U1 and U2 are grouped together to form a sub-block named sub1, using the following command:

{% code lineNumbers="true" %}
```bash
current_design top
group {U1 U2} -design_name sub1
```
{% endcode %}

<figure><img src="../../.gitbook/assets/group (1).png" alt=""><figcaption><p>Figure 5-1 Changing Partitions (Group)</p></figcaption></figure>

### Ungroup Command

The `ungroup` command performs the opposite function. It is used to remove the hierarchy, as shown in Figure 5-1, by using the following command.

{% code lineNumbers="true" %}
```bash
current_design top
ungroup -all
```
{% endcode %}

<figure><img src="../../.gitbook/assets/ungroup (1).png" alt=""><figcaption><p>Figure 5-1 Changing Partitions (Ungroup)</p></figcaption></figure>

The designer can also use the `ungroup` command along with the `-flatten` and `-all` options to flatten the entier hierarchy. This is illustrated below:

{% code lineNumbers="true" %}
```bash
ungroup -flatten -all
```
{% endcode %}
