# Studio 4 - PWM Programming

## PWM Concepts

In previous studios, we learnt how to generate digital signals using the GPIO pins. This studio will introduce _Pulse-Width Modulation_ (PWM), a digital technique used to simulate an analog signal by rapidly toggling a signal between a HIGH (1) and a LOW (0) state at a controlled frequency. By adjusting the proportion of time the signal remains HIGH versus LOW, we can approximate different analog voltages.

The PWM signal has two key parameters:

1. Period
2. Duty Cycle

<figure><img src="../.gitbook/assets/studio4-pwm-demo.png" alt="" width="375"><figcaption><p>A typical PWM waveform with one period zoom in</p></figcaption></figure>

### Period

**Period** is the total duration of one cycle.

The **period** of the PWM signal (measured in seconds) indicates the amount of time it takes for the signal to complete **one cycle**. The **frequency** of the PWM signal is $$\frac{1}{\text{period(s)}}$$.

{% hint style="info" %}
Note the unit for **period** is **seconds**!
{% endhint %}

### Duty Cycle

The **Duty Cycle** represents the percentage of time the signal is HIGH (ON Time, a.k.a _pulse width_) **during a cycle**. The relationship between duty cycle and ON Time is characterized by the Equation below:

$$
\text{Duty Cycle(%)}=\frac{\text{On Time (s)}}{\text{Period (s)}}\times 100\%
$$

When observing a PWM signal closely, the ON and OFF states within a single period appear distinct and separate. However, as the PWM signal's frequency increases, it becomes more difficult to distinguish between these states, making the signal appear more continuous and averaged over time. A peripheral, such as a servo motor, interprets the PWM signal as an effective analog voltage between 0 V and VCC, depending on the duty cycle. The value of this average analog signal can be determined by the Duty Cycle in the PWM waveform:

$$
\text{Average Analog Signal (V)}=\text{Duty Cycle (%)} \times \text{Vcc (V)}
$$

### PWM vs. DAC

We have already seen how PWM works above. Now, let's take a look at how modern DAC (**D**igital **A**nalog **C**onverter) works.

#### **How Modern DAC Works**

A Digital-to-Analog Converter (DAC) converts a digital value (e.g., an 8-bit or 16-bit number) directly into a corresponding analog voltage or current. Modern DACs employ techniques such as:

* **Resistor Ladder (R-2R)**: Uses a network of resistors to produce an analog voltage proportional to the binary input.
* **Sigma-Delta Modulation**: Employs oversampling and noise shaping for high-precision output.

Unlike PWM, a DAC generates a smooth, continuous analog signal without relying on time-averaging. The output voltage scales linearly with the digital input, offering fine control and high resolution depending on the bit depth (e.g., 12-bit or 16-bit).

#### **Key Differences**

Here are the primary distinctions between PWM and DAC, with additional context integrated:

* **Signal Nature**:
  * **PWM**: Produces a **digital square wave**. The analog effect arises from **averaging the signal over time**, making it an **approximation** rather than a true analog output.
  * **DAC**: Outputs a genuine analog signal with **continuous voltage levels**, requiring no averaging.
* **Hardware Complexity**:
  * **PWM**: Requires minimal hardware—just a digital pin on a microcontroller or a simple timer circuit—making it cost-effective and easy to implement.
  * **DAC**: Involves dedicated analog circuitry, which is more complex and typically more expensive.
* **Precision and Smoothness**:
  * **PWM**: The perceived smoothness depends on the switching frequency and the device’s response. At lower frequencies, the digital pulses may cause noticeable effects (e.g., flickering in LEDs). Adding a low-pass filter can smooth the signal into a near-continuous voltage, but it’s still not as seamless as a DAC.
  * **DAC**: Delivers a smooth, stable analog output with no inherent switching, providing higher precision and consistency.
* **Applications**:
  * **PWM**: Commonly used for power control in devices like motors, LEDs, and heaters, where the averaging effect suffices and cost is a concern.
  * **DAC**: Suited for applications needing precise analog signals, such as audio reproduction, scientific instrumentation, or communication systems.

#### Why PWM Is Considered Analog-Like

Despite being a digital signal, PWM is often described as simulating an analog output because:

* Devices respond to the _average voltage_ rather than the individual pulses.
* At high frequencies, the switching occurs too quickly for many systems to detect, mimicking a continuous voltage.
* With a low-pass filter, the PWM signal can be physically smoothed into a nearly continuous analog waveform.

However, PWM remains a **digital approximation**—its effectiveness as an "analog" signal depends on the frequency and the connected device’s characteristics. In contrast, a DAC provides a true analog output without such dependencies.

> PWM is considered "analog" in this context because of its _output effect,_ not its waveform.

This is also why we mentioned this sentence above because the role of **frequency** in PWM signal generation,

> However, as the PWM signal's frequency increases, it becomes more difficult to distinguish between these states, making the signal appear more continuous and averaged over time.&#x20;

## Bare Metal Programming

> Suppose in this section, we want to setup a Phase-Correct PWM waveform using Timer 0 (hence, the register number `n` = 0), with the duty cycle of 75% and a frequency of 490 Hz as an example.

### Timer/Counter

In ATmega328p, the core concept to generate a PWM waveform involves utilizing the 8-bit Timer/Counter to precisely control the duration of the signal’s high and low states, enabling us to achieve **the specified period** and **duty cycle** for the PWM waveform.

<div align="center"><figure><img src="../.gitbook/assets/studio4-timer-counter-block-diagram.png" alt="" width="563"><figcaption><p>Timer/Counter Block Diagram (P126)</p></figcaption></figure></div>

{% hint style="info" %}
Note that many register and bit references are presented in a generalized form: `n` represents the register number, `x` = `A`, `B` denotes Unit `A` or `B`.
{% endhint %}

#### Definition

<table><thead><tr><th width="100">Constant</th><th>Description</th></tr></thead><tbody><tr><td>BOTTOM</td><td>The counter reaches the BOTTOM when it becomes zero (<code>0x00</code> for 8-bit counters, or <code>0x0000</code> for 16-bit counters).</td></tr><tr><td>MAX</td><td>The counter reaches its Maximum when it becomes <code>0xFF</code> (decimal 255, for 8-bit counters) or <code>0xFFFF</code> (decimal 65535, for 16-bit counters).</td></tr><tr><td>TOP</td><td>The counter reaches the TOP when it becomes equal to the <strong>highest value in the count sequence</strong>. The TOP value can be assigned to be the fixed value MAX or the value stored in the <code>OCR0A</code> Register. The assignment is dependent on the mode of operation.</td></tr></tbody></table>

### Select Clock Source

> This step is to setup to get the **frequency** of the PWM waveform we want.

The <mark style="color:green;">green</mark> section in the figure about timer/counter block diagram represents the clock source. The Timer/Counter can be driven by either an internal or external clock source. The clock source is selected by configuring the Clock Select bits `CS0[2:0]` in the Timer/Counter Control Register (`TCCRnB`), highlighted in the <mark style="color:yellow;">yellow</mark> section of same figure above.

<figure><img src="../.gitbook/assets/studio4-tccr0b-register.png" alt=""><figcaption><p>TCCR0B Resgiter (P141)</p></figcaption></figure>

<figure><img src="../.gitbook/assets/studio4-clock-select-bit-description.png" alt=""><figcaption><p>Table 19.10: Clock Select Bit Description (P142)</p></figcaption></figure>

The clock source that you select is directly linked to the desired PWM frequency. The relationship between the clock source frequency and the prescaler is described by the following equation:

$$
f_{\text{OCnxPCPWM}}=\frac{f_{\text{clk_I/O}}}{\text{N}\cdot 2\cdot \text{TOP}}
$$

where

1. $$f_{\text{OCnxPCPWM}}$$ refers to the frequency of the **P**hase-**C**orrect **PWM**.
2. $$f_{\text{clk_I/O}}$$ is the clock frequency, in Arduino, it is 16MHz.
3. $$N$$ represents the prescaler factor (1, 8, 64, 256 or 1024) that we can configuer from Clock Select bits `CS0[2:0]` in `TCCRnB`, see the Table 19.10 above.

For setting a desired frequency of 490Hz, the equation will be

$$
f_{\text{OCnxPCPWM}}=\frac{16\times 10^6\text{ (Hz)}}{\text{N}\cdot 2 \cdot 255}=490\text{ (Hz)}
$$

From the equation above, the prescaler factor `N` is calculated to be approximately 64.025. Since the prescaler must be a standard value, the closest available `N` is 64. Therefore, the value that needs to be set in the Clock Select bits `CS0[2:0]` in `TCCRnB` is `0b011`.

### Setting Duty Cycle

The **duty cycle** is determined by **configuring the Counter Unit** and the **Output Compare Unit**. The Counter Register `TCNTn` increments at the frequency configured in [Section above](studio-4-pwm-programming.md#select-clock-source). The value in the `TCNTn` register **is continuously compared with the value in the** `OCRnx` **register in the Output Compare Unit**. The output signal is set to HIGH if the value in `TCNTn` is **less than** the value in `OCRnx`. Consequently, the duration of the HIGH state corresponds to the desired duty cycle. Below is a detailed step-by-step illustration of the complete setup process:

#### Counter Unit

The <mark style="color:blue;">blue section</mark> in the figure above about timer block diagram represents the **Counter Unit**. Its equivalent block diagram (Figure 1.3) is shown below for reference:

<figure><img src="../.gitbook/assets/studio4-counter-unit-block-diagram.png" alt=""><figcaption><p>Couter Unit Block Diagram (P127)</p></figcaption></figure>

Once the clock is set, the `TCNTn` register **increments at a rate determined by the configured frequency**. For generating a phase-correct PWM, the initial value of the Counter Register `TCNT0` must be **set to 0** at the start of the timer. This guarantees that the timer begins counting from the correct baseline, enabling accurate waveform generation.

#### Output Compare Unit

> This is where you will set the **duty cycle** of your PWM waveform.

The pink section in the figure above about timer block diagram represents the **Output Compare Unit**. Its equivalent block diagram is shown as follows,

<figure><img src="../.gitbook/assets/studio4-output-compare-unit-block-diagram.png" alt="" width="375"><figcaption><p>Output Compare Unit Block Diagram (P129)</p></figcaption></figure>

The **duty cycle** is determined by the value stored in the `OCRnA` register, based on the following formula.

$$
\text{Duty Cycle (%)}=\frac{\text{OCR0A}}{255}\times 100\%
$$

For generating the desired PWM waveform with 75% duty cycle, we have:

$$
\text{Duty Cycle (%)}=\frac{\text{OCR0A}}{255}\times 100\%=75\%
$$

Thus, the value we want to write to `OCR0A` is 191.

### Interrupts During PWM

The timer can be configured to generate interrupts:

1. **whenever there is an output compare match** or
2. when `TCNT0` rolls over.

{% hint style="info" %}
"TCNT0 roll over" means when the Timer/Counter 0 (TCNT0) register on the ATmega328P reaches its **maximum value** and resets back to zero. So, the roll over signals **the end of one full period**. That's why we have the following useful application. :smile:
{% endhint %}

This is useful when **setting a new PWM duty cycle** at the end of the current PWM signal.

***

To enable the interrupts on Timer 0, we can use the `TIMSK0` register,

<figure><img src="../.gitbook/assets/studio4-timsk0-resgiter.png" alt=""><figcaption><p>TIMSK0 register (P143)</p></figcaption></figure>

Since we are using the `OCR0A` in the previous step, here we will set `OCIEA` to 1. Thus, we should write `0b10` to `TIMSK0`.

### Selected Desired PWM Mode

There are two PWM types in Atmega328:

1. Fast PWM
2. Phase-Correct PWM.

Fast PWM allows higher frequencies. Phase-Correct PWM generates symmetric waveforms, which have better resolution at the expense of maximum frequency. Because the Phase-Correct PWM is more suitable for motors, in CG2111A, **we will only use Phase-Correct PWM**.

***

The `TCCRnx` register is used to configure PWM types and modes in the Atmega328. There are two PWM modes:

1. one where the value in `OCR0x` is updated as the counter increments from BOTTOM to TOP, and&#x20;
2. another where the value is updated as the counter decrements from TOP to BOTTOM.

In the former mode, the PWM waveform starts with a **rising duty cycle before falling**, while in the latter mode, the PWM waveform starts with a **falling duty cycle before rising**.

<figure><img src="../.gitbook/assets/studio4-tccr0a-register.png" alt=""><figcaption><p>TCCR0A Register (P138)</p></figcaption></figure>

<figure><img src="../.gitbook/assets/studio4-waveform-generation-mode.png" alt=""><figcaption><p>Waveform Generation Mode Bit Representation (P140)</p></figcaption></figure>

According to the two tables above, we need to configure `WGM[2:0]` bits in `TCCR0A` register to configure the desired type of PWM. There are two Phase-Correct PWM modes. We will be using mode 1.

<figure><img src="../.gitbook/assets/studio4-compare-output-mode-phase-correct-pwm.png" alt=""><figcaption><p>Compare Output Mode, Phase Correct PWM Mode (P140)</p></figcaption></figure>

We also need to set `COM0A[1:0]` to `0b10`. This will ensure that when `TCNT0` counts up from 0 to `OCR0A`, it **clears** the `OC0A` (which is our output pin) and **sets** it when `TCNT0` counts down from 255 to `OCR0A`.

***

Below is the whole code demo,

{% code overflow="wrap" lineNumbers="true" %}
```cpp
void setup() {
	TCNT0 = 0;
	TCCR0A = 0b10000001; // Set OCOM0A to 10 and WGM to 01, Phase-Correct Mode and certain behavior in COMA
	TIMSK0 |= 0b10; // Enable Int for Output Compare Match, OCFA Flag
	OCR0A = 191; // This is used to change the duty cycle
	TCCR0B = 0b00000011; // Set clk source to clk/64, this is used to change the frequency/period
	//Set PORTD Pin 6 (Arduino Pin 6) as Output
	DDRD |= PIN6;
	sei();
}
```
{% endcode %}

### Summary

Here’s a table summarizing the registers and output pins for configuring Phase-Correct PWM on Timer 0, Timer 1, and Timer 2 on the ATmega328P:

| **Feature**                        | **Timer 0 (8-bit)**       | **Timer 1 (16-bit)**                  | **Timer 2 (8-bit)**          |
| ---------------------------------- | ------------------------- | ------------------------------------- | ---------------------------- |
| **Control Register A**             | `TCCR0A`                  | `TCCR1A`                              | `TCCR2A`                     |
| **Control Register B**             | `TCCR0B`                  | `TCCR1B`                              | `TCCR2B`                     |
| **Counter Register**               | `TCNT0`                   | `TCNT1`                               | `TCNT2`                      |
| **Output Compare A**               | `OCR0A` (Duty Cycle)      | `OCR1A` (Duty Cycle)                  | `OCR2A` (Duty Cycle)         |
| **Output Compare B**               | `OCR0B` (Optional)        | `OCR1B` (Optional)                    | `OCR2B` (Optional)           |
| **Interrupt Mask**                 | `TIMSK0` (e.g., OCIE0A)   | `TIMSK1` (e.g., OCIE1A)               | `TIMSK2` (e.g., OCIE2A)      |
| **Output Pin A**                   | OC0A (PD6, Arduino Pin 6) | OC1A (PB1, Arduino Pin 9)             | OC2A (PB3, Arduino Pin 11)   |
| **Output Pin B**                   | OC0B (PD5, Arduino Pin 5) | OC1B (PB2, Arduino Pin 10)            | OC2B (PD3, Arduino Pin 3)    |
| **Clock Select Bits**              | CS0\[2:0] (in TCCR0B)     | CS1\[2:0] (in TCCR1B)                 | CS2\[2:0] (in TCCR2B)        |
| **Prescaler Options**              | 1, 8, 64, 256, 1024       | 1, 8, 64, 256, 1024                   | 1, 8, 32, 64, 128, 256, 1024 |
| **WGM Bits for Phase-Correct PWM** | WGM0\[2:0] = 001          | WGM1\[3:0] = 0001 or 1010 (with ICR1) | WGM2\[2:0] = 001             |

And below is the demo setup code for Timer 2 (8-bit) and Timer 1 (16-bit)

{% tabs %}
{% tab title="Timer 2 (8-bit)" %}
{% code overflow="wrap" lineNumbers="true" %}
```cpp
void setup() {
    TCNT2 = 0;                    // Initialize counter to 0
    TCCR2A = 0b10000001;          // COM2A[1:0] = 10 (clear on up, set on down), WGM2[1:0] = 01 (Phase-Correct PWM)
    TIMSK2 |= 0b10;               // Enable interrupt on OCR2A compare match (OCIE2A = 1)
    OCR2A = 191;                  // Set 75% duty cycle (0.75 * 255 = 191)
    TCCR2B = 0b00000011;          // CS2[2:0] = 011 (prescaler = 64), WGM2[2] = 0 (Phase-Correct PWM)
    DDRB |= (1 << PB3);           // Set PB3 (Arduino Pin 11, OC2A) as output
    sei();                        // Enable global interrupts
}
```
{% endcode %}
{% endtab %}

{% tab title="Timer 1 (16-bit)" %}
{% code overflow="wrap" lineNumbers="true" %}
```cpp
void setup() {
    TCNT1 = 0;                    // Initialize counter to 0
    TCCR1A = 0b10000010;          // COM1A[1:0] = 10 (clear on up, set on down), WGM1[1:0] = 10 (Phase-Correct PWM with ICR1)
    TCCR1B = 0b00010011;          // WGM1[3:2] = 10, CS1[2:0] = 011 (prescaler = 64)
    TIMSK1 |= 0b10;               // Enable interrupt on OCR1A compare match (OCIE1A = 1)
    ICR1 = 255;                   // Set TOP to 255 for frequency calculation (matches 8-bit timers)
    OCR1A = 191;                  // Set 75% duty cycle (0.75 * 255 = 191)
    DDRB |= (1 << PB1);           // Set PB1 (Arduino Pin 9, OC1A) as output
    sei();                        // Enable global interrupts
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
