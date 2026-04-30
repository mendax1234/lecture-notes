---
description: Thanks for my tutor Eric Han!
---

# Tut 03

## Problem Set 9

### Problem 9.1

#### Problem 9.1(a)

In the snippet of code, we can easily find out that a counterexample is $$a=b=c\neq0$$. Our program will output 0. That's incorrect.

#### Problem 9.1(b)

To find all the test cases that the function would fail, we can start from the counterexample we have given in [#problem-9.1-a](tut-03.md#problem-9.1-a "mention").

> What if any two of these three numbers are equal and not zero?

Let's say $$a=b\neq0$$. That means all the if conditions that contain `a==b` will output false and due the the **short-circuiting** property of the `&&` operator, the first two conditions won't pass and max is still 0. Now we have only one condition remaining. How to make it **"false"**?

If $$c$$ is bigger than both $$a$$ and $$b$$, then max will be $$c$$, our function will output the correct answer. But think it reversely, if $$c$$ is less than $$a$$ and $$b$$, our max will be zero and the actual max should be either $$a$$ or $$b$$.

Now, we have come to one of our solutions, that's when $$0\neq a=b>c$$, our function will output wrongly.

Similarly, we can get the other two conditions

1. $$0\neq b=c>a$$
2. $$0\neq a=c>b$$

Adding the counterexample from [#problem-9.1-a](tut-03.md#problem-9.1-a "mention"), we have written all the cases when our function will output wrongly.

### Problem 9.2

To solve this question, we should form our "truth table" first

<table data-full-width="false"><thead><tr><th>vac(p)</th><th>vac(q)</th><th>child(p)</th><th>child(q)</th><th>same_household(p,q)</th></tr></thead><tbody><tr><td>yes</td><td>yes</td><td>dc</td><td>dc</td><td>dc</td></tr><tr><td>yes</td><td>no</td><td>dc</td><td>yes</td><td>yes</td></tr><tr><td>no</td><td>yes</td><td>yes</td><td>dc</td><td>yes</td></tr><tr><td>no</td><td>no</td><td>yes</td><td>yes</td><td>yes</td></tr></tbody></table>

Then, we use this table to form our code

```c
if (vac(p) && vac(q))
    return true;
if (same_household(p, q))
{
    if (vac(p) && child(q))
        return true;
    if (vac(q) && child(p))
        return true;
    if (child(p) && (child(q))
        return true;
}
return false;
```

## Problem Set 10

### Problem 10.1

1. $$(x\leq1)||(y==10)$$
2. $$eating || !drinking$$
3. $$(!has\_cs2030$$ `&&` $$!has\_cs2113) || !has\_cs2040c$$

### Problem 10.2

```c
long score = 4;
if (something) {
  score = 10;
} else {
  score = 0;
}
// { score == 0 || score == 10 }

if (score == 4) {
    score = 1;
} else {
    score += 10;
}
// { score == 20 || score == 10 }

if (score >= 10) {
    cs1010_println_string("ok");
} else {
    cs1010_println_string("failed");
}
```

Based on the assertion we have derived in the comment in the code above, we can safely say the string "ok" will be printed.

<details>

<summary>What's the use of assertion</summary>

Assertion can not only help us get a better view of our code flow, but can also help us find the **dead code** (the code that will never be executed) in our program. For example, in [#problem-10.2](tut-03.md#problem-10.2 "mention"), our dead code will be the condition that judge whether our score will be 4.

</details>
