# Tut 00 - Introduction to MIPS Assembly

Computers represent information using **binary (0s and 1s)**, so programs must ultimately be translated into **machine code**, a sequence of binary instructions that the CPU can execute. However, machine code is difficult for humans to read and write.

To make programming at the hardware level easier, we use **assembly language**, which represents machine instructions using readable mnemonics and registers. In general, each assembly instruction corresponds to a machine instruction.

<details>

<summary>Assembly Language vs. Instruction Set Architecture</summary>

The **ISA** defines the instructions and rules that a CPU supports — it is the **specification/interface between software and hardware**. **Assembly language** is the **human-readable notation used to write those instructions**.

{% hint style="success" %}
In other words, ISA is the **specification** and assembly language is the human-readable way to express that specification.
{% endhint %}

</details>

In this tutorial, we will learn the basics of **MIPS assembly**, a simpler assembly language easier than ARM assembly. Below is the table summarizing all the basic MIPS assembly that we are going to use in CG2271:

| Category            | Instruction              | Meaning                                                                |
| ------------------- | ------------------------ | ---------------------------------------------------------------------- |
| **Memory**          | `lw $r1, offset($r2)`    | Load the word from memory address **\[**`$r2`**] + offset** into `$r1` |
| **Memory**          | `sw $r1, offset($r2)`    | Store `$r1` to memory address **\[**`$r2`**] + offset**                |
| **Data-Processing** | `li $r1, const`          | Load constant value `const` into `$r1`. `const` can be negative        |
| **Data-Processing** | `la $r1, var`            | Load the **address** of variable `var` into `$r1`                      |
| **Data-Processing** | `mov $r1, $r2`           | Copy the value in `$r2` into `$r1`                                     |
| **Data-Processing** | `add $r1, $r2, $r3`      | `$r1 = $r2 + $r3`                                                      |
| **Data-Processing** | `add $r1, $r2, const`    | `$r1 = $r2 + const`                                                    |
| **Data-Processing** | `sub $r1, $r2, $r3`      | `$r1 = $r2 - $r3`                                                      |
| **Data-Processing** | `mul $r1, $r2, $r3`      | `$r1 = $r2 * $r3`                                                      |
| **Data-Processing** | `mul $r1, $r2, const`    | `$r1 = $r2 * const`                                                    |
| **Data-Processing** | `div $r1, $r2, $r3`      | `$r1 = $r2 / $r3`                                                      |
| **Data-Processing** | `div $r1, $r2, const`    | `$r1 = $r2 / const`                                                    |
| **Data-Processing** | `slt $r1, $r2, $r3`      | `$r1 = 1` if `$r2 < $r3`, otherwise `$r1 = 0`                          |
| **Data-Processing** | `slt $r1, $r2, const`    | `$r1 = 1` if `$r2 < const`, otherwise `$r1 = 0`                        |
| **Control**         | `beq $r1, $r2, target`   | Branch to `target` if `$r1 == $r2`                                     |
| **Control**         | `beq $r1, const, target` | Branch to `target` if `$r1 == const`                                   |
| **Control**         | `bne $r1, $r2, target`   | Branch to `target` if `$r1 != $r2`                                     |
| **Control**         | `bne $r1, const, target` | Branch to `target` if `$r1 != const`                                   |
| **Control**         | `j target`               | Jump to `target`                                                       |

**MIPS instructions**, similar to RISC-V instructions, can be divided into three groups:

1. **Data-Processing (DP) Instructions**
   1. For **DP instructions**, the usual convention is: **destination register, left operand, right operand**.
2. **Memory Instructions**
3. **Control Instructions**
   1. For **control instructions**, `target` usually refers to a **label**, which identifies the location to jump or branch to.

## Registers and Load/Store Models

### 1. General Purpose Registers

**General-Purpose Registers (GPRs)** are small, very fast storage locations inside the CPU that hold data and intermediate results during processing. They are typically **one word in size** and can be accessed at about the same speed as the CPU. Since processors have a **limited number of GPRs** (commonly around 4-32), they must be used efficiently.

### 2. Load/Store Architecture

Basically, in the MIPS assembly that we learned in CG2271, `lw` is used to read a variable’s value from memory into a register. After modifying the value, `sw` is used to write the updated value back to memory. This can be seen from the following example:

{% tabs %}
{% tab title="C Code" %}
{% code lineNumbers="true" expandable="true" %}
```c
// Example 1
x = x + 1;

// Example 2
x = y + z - 2;
```
{% endcode %}
{% endtab %}

{% tab title="MIPS Assembly" %}
{% code lineNumbers="true" expandable="true" %}
```mipsasm
# Example 1: x = x + 1
lw   $1, x           # Load x into register $1
add  $1, $1, 1       # Increment $1
sw   $1, x           # Store $1 back into x

# Example 2: x = y + z - 2
lw   $1, y           # Load y into $1
lw   $2, z           # Load z into $2
add  $2, $1, $2      # $2 = $1 + $2
sub  $2, $2, -2      # $2 = $2 - 2
sw   $2, x           # x = $2
```
{% endcode %}
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
MIPS, like RISC-V, does not have a specific instruction for **subtracting a constant** from a value. Instead, we can **add the negative of that constant** using `add`. This keeps the instruction set simple and avoids unnecessary instructions.
{% endhint %}

## Decisions

Decisions in programs, such as `if-else` structures, are implemented using **control instructions** such as `beq` (branch if equal) and `bne` (branch if not equal). We will see how it works in detail from the following example.

{% tabs %}
{% tab title="C Code" %}
{% code lineNumbers="true" expandable="true" %}
```c
if (c < 5) {
    x = x * 2;
} else {
    y = y / 2;
}
```
{% endcode %}
{% endtab %}

{% tab title="MIPS Assembly" %}
{% code lineNumbers="true" expandable="true" %}
```mipsasm
        lw   $1, c           # Logic: c < 5 means we branch to
        slt  $1, $1, 5       # else part if c >= 5. I.e. slt $r1, $r2, 5
        beq  $1, 0, else     # results in $r1 == 0
        lw   $1, x           # IF part: Load x into $1
        mul  $1, $1, 2       # IF part: Multiply $1 by 2
        sw   $1, x           # IF part: Store $1 to x
        j    cont            # IF part: Jump to rest of code
else:   lw   $1, y           # ELSE part: Load y into $1
        div  $1, $1, 2       # ELSE part: Divide $1 by 2
        sw   $1, y           # ELSE part: Store $1 to y
cont:   ...                  # Rest of code
```
{% endcode %}
{% endtab %}
{% endtabs %}

The usual flow for writing a **decision in assembly** is:

1. **Set up the condition check.**
2. **Branch to the** `else` **part** if the condition is false.
3. **Execute the** `if` **part** if the condition is true and remember to `jump` to skip the `else` part.
4. **Continue after the** `else` **part** using another label.

Here, `else` and `cont` are **labels** that can be used as the `target` of control instructions.

## Loops

Similarly, we can see how to write the assembly code for loops from the following example.

{% tabs %}
{% tab title="C Code" %}
{% code lineNumbers="true" expandable="true" %}
```c
for (i = 0; i < 10; i++) {
    y = y - 1;
    if (y < 0) {
        z = z + y;
    } else {
        z = z - y;
    }
}
```
{% endcode %}
{% endtab %}

{% tab title="MIPS Assembly" %}
{% code lineNumbers="true" expandable="true" %}
```mipsasm
li   $1, 0           # Initialize i to 0
        sw   $1, i
start:  slt  $2, $1, 10      # Test if i < 10
        beq  $2, 0, exit     # i >= 10, exit
        lw   $2, y           # Load y
        addi $2, $2, -1      # Subtract 1
        sw   $2, y           # Write back to y
        slt  $3, $2, 0       # Is y < 0
        beq  $3, $0, else    # No, y >= 0, branch to else
        lw   $3, z           # Yes, y < 0. Load z
        add  $3, $3, $2      # Add y to z
        sw   $3, z           # Write back to z
        j    cont            # Jump over else part
else:   lw   $3, z           # y >= 0. Load z
        sub  $3, $3, $2      # Subtract y from z
        sw   $3, z           # Write back to z
cont:   add  $1, $1, 1       # Increment i
        sw   $1, i           # Update i
        j    start           # Go for next iteration
exit:   ...                  # Rest of program ...
```
{% endcode %}

{% hint style="success" %}
Here, for efficiency, the loop variable is only **written back to**, and **not read**, since we always have the latest value in `$1`.
{% endhint %}
{% endtab %}
{% endtabs %}
