# Diagnostic Quiz

## Problem

### Branch Test Coverage

Suppose you have the following branch

```java
if (Condition A) {         // Decision Point 1
    // Block 1
} else if (Condition B) {    // Decision Point 2
    // Block 2
} else {
    // Block 3
}
```

And you have two test cases which enter the `else-if` block and the `else` block, what will be your branch test coverage?

***

**Ans**: 75%. The formula to calculate the branch test coverage is as follows,

$$
\text{Branch Coverage} = \frac{\text{Number of Executed Branches}}{\text{Total Number of Branches}} \times 100\%
$$

There are 4 distinct branches (outcomes) in this problem:

1. Branch 1: `Condition A` is True (Enters Block 1).
2. Branch 2: `Condition A` is False (Proceeds to check `else if`).
3. Branch 3: `Condition B` is True (Enters Block 2).
4. Branch 4: `Condition B` is False (Enters Block 3/Else).

Now, let's map our specific test cases to these branches.

1. Test Case 1: Executes the `else if` block
   * For the code to reach the `else if` block, `Condition A` must be False. (Covers Branch 2)
   * Then, `Condition B` must be True. (Covers Branch 3)
2. Test Case 2: Executes the `else` block
   * For the code to reach the `else`, `Condition A` must be False. (Covers Branch 2 again)
   * Then, `Condition B` must be False. (Covers Branch 4)

So, the number of executed branches is **3**, and as the total is 4, the **branch** **test coverage** will be 75%.

{% hint style="warning" %}
This question has appeared in CS2113 AY25/26 Sem 1 Final!
{% endhint %}
