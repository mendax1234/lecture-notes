# Studio 1 - GPIO Programming

## Bit masking

Bit masking is a technique used in programming and computer science to **focus on and** **manipulate specific bits** in a **binary number**. In CG2111A, the manipulation includes:

1. [#set-certain-bits](studio-1-gpio-programming.md#set-certain-bits "mention")
2. [#clear-certain-bits](studio-1-gpio-programming.md#clear-certain-bits "mention")
3. [#extract-certain-bits](studio-1-gpio-programming.md#extract-certain-bits "mention")
4. [#check-certain-bits](studio-1-gpio-programming.md#check-certain-bits "mention")

This technique is done by doing **bitwise logical operation** on the **source binary number `s`** using another **binary number** called **`mask`**. So, for the above three manipulations, the most important part should lie in:

1. which **logical bitwise operation** to use
2. how to form our mask

Before delving into these three manipulations, let's make some clarification on the vairables we will be used in this part first:

1. `storage` : this is our **source** binary number.
2. `mask` : this is the binary number we will use to do bit-masking.

After that, the following step is to know **which bits** we shall manipulate on. And based on this information, we shall create a "pre-mask", **with "1" indicating the bits we are interested in, "0" for the remaining.**

For example, in a 8-bit long binary numebr, we are interested in bit 3, bit 5 and bit 7, the "pre-mask" we shall use is:

<pre><code><strong>Binary number : 1 0 1 0 1 0 0 0
</strong><strong>Position      : 7 6 5 4 3 2 1 0
</strong></code></pre>

{% hint style="info" %}
Note that in CG2111A, we treat the Least Significant Bit (The right-most bit) as bit 0 and start increasing **from right to left**.
{% endhint %}

In CG2111A, to create the pre-mask. We will use the **left-shift operator** `<<` and the **bitwise OR operator** `|`. For example, the code for creating our above example is as follows:

{% code lineNumbers="true" %}
```arduino
#define BIT3 (1 << 3)
#define BIT5 (1 << 5)
#define BIT7 (1 << 7)

void setup()
{
    Serial.println(BIT3 | BIT5 | BIT7);
}
```
{% endcode %}

### Set certain bits

To set certain bits, we will need to form our `mask` and use the bitwise OR operator `|`.

{% stepper %}
{% step %}
**Form the `mask`**

When we **set certain bits**, our `mask` will be the **same** as our "pre-mask".

{% code lineNumbers="true" %}
```arduino
#define BIT3 (1 << 3)
#define BIT5 (1 << 5)
#define BIT7 (1 << 7)
#define MASK (BIT3 | BIT5 | BIT7)
```
{% endcode %}
{% endstep %}

{% step %}
**Do the bitwise logical OR operation**

```arduino
storage |= MASK;
```
{% endstep %}

{% step %}
**Example**

{% code lineNumbers="true" %}
```arduino
#define BIT3 (1 << 3)
#define BIT5 (1 << 5)
#define BIT7 (1 << 7)
#define MASK (BIT3 | BIT5 | BIT7)

void loop() {
    storage |= MASK;
}
```
{% endcode %}
{% endstep %}
{% endstepper %}

### Clear certain bits

To clear certain bits, we still need to form our `mask` and then use the bitwise AND oeprator `&`.

{% stepper %}
{% step %}
**Form the `mask`**

When we **clear certain bits**, our `mask` will be the **inverse** of our "pre-mask". This is done by using bitwise NOT operator `~`.

{% code lineNumbers="true" %}
```arduino
#define BIT3 (1 << 3)
#define BIT5 (1 << 5)
#define BIT7 (1 << 7)
#define MASK ~(BIT3 | BIT5 | BIT7)
```
{% endcode %}
{% endstep %}

{% step %}
**Do the bitwise logical AND operation**

```arduino
storage &= MASK;
```
{% endstep %}

{% step %}
**Example**

```arduino
#define BIT3 (1 << 3)
#define BIT5 (1 << 5)
#define BIT7 (1 << 7)
#define MASK ~(BIT3 | BIT5 | BIT7)

void loop() {
    storage &= MASK;
}
```
{% endstep %}
{% endstepper %}

### Extract certain bits

This is a very useful manipulation and as usual, we will need our `mask` and the bitwise logical AND operator `&`.

{% stepper %}
{% step %}
**Form the `mask`**

When we **set certain bits**, our `mask` will be the **same** as our "pre-mask".

{% code lineNumbers="true" %}
```arduino
#define BIT3 (1 << 3)
#define BIT5 (1 << 5)
#define BIT7 (1 << 7)
#define MASK (BIT3 | BIT5 | BIT7)
```
{% endcode %}
{% endstep %}

{% step %}
**Do the bitwise logical AND operation**

```
storage &= MASK;
```
{% endstep %}

{% step %}
**Example**

```arduino
#define BIT3 (1 << 3)
#define BIT5 (1 << 5)
#define BIT7 (1 << 7)
#define MASK (BIT3 | BIT5 | BIT7)

void loop() {
    storage &= MASK;
}
```
{% endstep %}
{% endstepper %}

### Check certain bits

First, let's define the third binary number as `bits`, which is the binary number we want our source binary number `storage` to be.

Now, to check certain bits, we still need to form our `mask` and then use the bitwise logical AND operator `&`.

{% stepper %}
{% step %}
**Form the `mask`**

When we **set certain bits**, our `mask` will be the **same** as our "pre-mask".

{% code lineNumbers="true" %}
```arduino
#define BIT3 (1 << 3)
#define BIT5 (1 << 5)
#define BIT7 (1 << 7)
#define MASK (BIT3 | BIT5 | BIT7)
```
{% endcode %}
{% endstep %}

{% step %}
**Do the "check" operation**

{% code lineNumbers="true" %}
```arduino
(storage & MASK) == bits;
```
{% endcode %}

Notice that this is similar to [#extract-certain-bits](studio-1-gpio-programming.md#extract-certain-bits "mention"), the steps here are basically:

1. Extract the certain bits in `storage`
2. Compare it with the desired `bits`
{% endstep %}

{% step %}
**Example**

Check if certain bits are what you want.

{% code lineNumbers="true" %}
```arduino
#define BIT3 (1 << 3)
#define BIT5 (1 << 5)
#define BIT7 (1 << 7)
#define MASK (BIT3 | BIT5 | BIT7)

void loop() {
    if ((storage & MASK) == bits) {
        // After extracting, the bits are what you want
        // Then do ...
    } else {
        // After extracting, the btis are not what you what
        // Then do...
}
```
{% endcode %}

In the code above, the conditions in the `else` structure may have the following two conditions:

1. The [_extracted bits_](#user-content-fn-1)[^1] are **not al**l 0.
2. The _extracted bits_ are all 0.

For the second condition, we may want to do something else, so we can use the following code:

```arduino
#define BIT3 (1 << 3)
#define BIT5 (1 << 5)
#define BIT7 (1 << 7)
#define MASK (BIT3 | BIT5 | BIT7)

void loop() {
    if ((storage & MASK) == 0) {
        // ...
    }
    // or
    if (!(storage & MASK)) {
        // ...
    }
}
```
{% endstep %}
{% endstepper %}

[^1]: The result of `storage &= mask`.
