# Tut 2 - Frequency Response

The main goal of this tutorial is to build the intuition needed to quickly identify the number and locations of poles and zeros in a circuit.

1. Finding Pole: Use the Association of Poles
   1. Number of poles: The number of poles is approximately the number of high-impedance nodes in the circuit.
      1. An input node with no resistance associated with it is not counted as a high-impedance node.
   2. Pole frequency: For each high-impedance node, use the association of poles method to find the equivalent resistance and capacitance seen from that node: $$\omega_p \approx \frac{1}{R_{\mathrm{eq}}C_{\mathrm{eq}}}$$. The node with the lowest pole frequency is the dominant pole.
2. Finding Zeros
   1. Where zeros come from: Zeros typically arise from input capacitances and feedback/feedforward paths that create an additional path from input to output.
   2. DC zero: An input coupling capacitor introduces a zero at DC: $$\omega_z = 0$$
   3. High-frequency zero: For a zero caused by a feedback/feedforward path, it can often be found quickly by setting $$V_{\mathrm{out}}=0$$ and solving for the frequency (a.k.a, $$s$$ and since $$s=j\omega$$, so it's okay to say frequency here) at which the transfer function becomes zero.

After finding the **poles and zeros**, we can draw the **Bode plot** to visualize the circuit’s frequency response. To do so:

1. **Express the transfer function** in factored form using its poles and zeros:

$$
H(s)=K\frac{(s-z_1)(s-z_2)\cdots}{(s-p_1)(s-p_2)\cdots}
$$

2. **Draw the magnitude and phase plots systematically** by applying the [rules introduced earlier](https://app.gitbook.com/s/FkaT0aUkbzwIwL8qtYDI/razavi/frequency-response-of-amplifiers/general-considerations#general-rules-for-drawing-bode-plots) for **LHP/RHP poles and zeros**.

## Problems

The following problems are worth going through again before midterms:

1. Q1 (3)
2. Q2 (especially the third question to draw the bode plot)
3. Q4

{% hint style="warning" %}
In Q4(b), even though these is a DC zero, there is actually an implicit low-frequency pole introduced by the coupling capacitor. However, in the magnitude bode plot, we just usually don't draw the effect of a DC zero.
{% endhint %}
