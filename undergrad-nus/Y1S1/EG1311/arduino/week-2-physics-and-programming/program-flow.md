# Program Flow

Remember the Program Flow part we have discussed in the [#machine-code-assembly-instruction-set](digital-computers.md#machine-code-assembly-instruction-set "mention")? Here, our **Program Flow** can be divided into three parts

1. Branching ([#conditionals](program-flow.md#conditionals "mention"))
2. Repetition ([#loops](program-flow.md#loops "mention"))
3. Abstraction (functions)

## Conditionals

Change program flow based on Boolean.

```c
if (a < b)
{
    // do something
}
else
{
    // do something else
}
```

## Loops

Repeat Lines. In C, we have three types of loops, `for`, `while` and `do-while` loops.

### An interesting Question

What number is stored in variable `b`?

```c
int a = 100;
while ((a >= 0) || (a % 2)) {
    a--;
}
int b = a;
```

This problem is a bit tricky since it utilizes the **short-circuiting** property of the `||` operator in C, that is `a || b`, if `a` is already `true`, we won't go to statement `b`. So, in this problem, we will always enter `a >= 0` until `a` is `-1`. Then we evaluate `a % 2`, which will be `-1`. (In C, every non-zero value is treated as **true**, so `a--` will still be executed and now our `a` is `-2`) Then we will exit the loop, so our final value for `b` will be `-2`.
