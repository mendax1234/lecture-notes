# Lec 07 - Timing

{% hint style="info" %}
Most of this part is covered in EE4415, but for the final, just print out the lecture slides as cheatsheet!
{% endhint %}

## Timing Constraints

The timing constraints is provided to the synthesis tool so that it knows what to optimize. More specifically, the timing constraints we provide are in the unit of **nanoseconds**. In particular, we will be talking about the constraints file used in Vivado.

{% hint style="warning" %}
The timing constraints provided to the synthesis tool is different from the constraint used in scheduling. The latter is usually in the unit of clock cycles.
{% endhint %}

### `set_max/min_delay`

Similar to the [`set_max/min_delay`](https://app.gitbook.com/s/Sp0XaarBjbEX3JIMrRaR/textbook-2-synopsys/constraining-designs/environment-and-constraints#set_max_delay) command we have seen in Synopsys, in Vivado, this command is to set maximum/minimum delays between any two points.

#### CDC Issue

The most interesting and important usage of the `set_max/min_delay` mentioned in this lecture is on the [CDC](https://en.wikipedia.org/wiki/Clock_domain_crossing) Issue of metastability. Basically, CDC exists when two parts of our system uses [**totally different**](#user-content-fn-1)[^1] clocks but they are still communicating the data with each other. To understand this issue better, we will use the follwing timing diagram.

<figure><picture><source srcset="../.gitbook/assets/cdc-issue-dark.png" media="(prefers-color-scheme: dark)"><img src="../.gitbook/assets/cdc-issue-light.png" alt=""></picture><figcaption></figcaption></figure>

[^1]: Here, "totally different" means that the two clocks **shouldn't** come from one clock source (PLL). Note that only differ in clock frequency but coming from the same clock source cannot be regarded as "totally different".
