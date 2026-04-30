---
description: Thanks for my Lab Tutor Zhang Puyu!
---

# Lab 03 - Assert

Slides:

{% file src="../../.gitbook/assets/Lab3.pdf" %}
Lab 3 Slides
{% endfile %}

## Exercise 1 Review

### Code Style

1. **Initialize** the variable immediately after the declaration.

For example,

```c
// Not Recommended
double num;
num = cs1010_read_double();

// Recommended
double num = cs1010_read_double();
```

2. Do not declare the function and then implement it somewhere else (Usually after `main()`). **Implement your function right after the declaration**.

#### No return after else

Code Example

```c
// Not allowed
if (something)
{
    return something;
}
else
{
    return other_things;
}
```

Out of succient reasons, the `else` is of no use. So, we can simplify the code to below

```c
// Allowed
if (something)
{
    return something;
}
return other_things;
```

#### No nested If

If your code is like below

```c
if (something_1)
{
    if (something_2)
    {
        // do something
    }
}
```

Since there is only one `if` statement inside the "outer" `if`, so there is **no need** to use the **nested `if`.** And we can simplify it into

```c
if (something_1 && something_2)
{
    // do sommething
}
```

## Assert Library

`assert.h` is a library designed to help with debugging procedures.

### Usage

`assert(p)` where `p` is a **boolean expression**. When the condition `p` fails during program execution, the program will halt with an error message.

## Exercise 1 Further Discussion

### odd.c

A solution that uses **no condition** is that $$m=n+(n\%2)*(n\%2)+1$$, where $$m$$ $$f(x) = x * e^{2 pi i \xi x}$$is the smallest odd number that is **strictly** bigger than $$n$$.

#### The Deriving process

<div data-full-width="false"><figure><img src="../../lab/.gitbook/assets/odd.png" alt=""><figcaption><p>odd.c</p></figcaption></figure></div>

### multiple.c

Modulo is not defined when we divide a **non-zero number** by 0. That is $$m\%0$$, where $$m\neq0$$, is **not defined!!!**

### date.c

There is a method not using conditions. Map each $$(m,d)$$ date to the integer $$100m+d$$.

{% hint style="info" %}
The only criteria for coefficient of $$m$$ is that the coefficient must be bigger than the maximum days in a month, which is 31. So $$m\geq31$$ should be okay.

The reason is that we should make sure the priority of the month is higher.
{% endhint %}

### power.c

#### Ways to optimize

1. Remove the useless work, when the base is 0, -1 or 1.

{% code fullWidth="false" %}
```c
long compute_power(long x, long y)
{
    if (x == 0)
    {
        return 0;
    }
    if (x == 1)
    {
        return 1;
    }
    if (x == -1)
    {
        return y % 2 == 0 ? 1 : -1;
    }
    // ...
}
```
{% endcode %}

2. Half the calculation

Recall that

$$
\large x^y=\begin{cases} \left(x^2\right)^{\frac{y}{2}} & \text{if } y \text{ is even} \\ \left(x^2\right)^{\frac{y-1}{2}} \cdot x & \text{if } y \text{ is odd} \end{cases}
$$

Then convert it to code will be intuitive

```c
if (y % 2 == 0)
{
    return compute_power(x * x, y / 2);
}
return x * compute_power(x * x, (y - 1) / 2);
```

### taxi.c

#### A normal way to compute ceil()

```c
long ceil_of_quotient(long n, long m)
{
    if (n % m == 0)
    {
        return n / m;
    }
    return n / m + 1;
}
```

#### A smart way to compute ceil()

Suppose we want to calculate $$\lceil \frac{n}{m} \rceil$$, where $$n$$ and $$m$$ are two possitive integers. First, let us write $$n=mq+r$$, where $$q\geq0$$ and $$0 \leq r \leq m-1$$. Now, let's consider

$$
\large\frac{n+m-1}{m}=\frac{mq+r+m-1}{m}=\frac{m(q+1)+r-1}{m}
$$

If $$r=0$$, then we will get $$q$$ as the output. If $$0<r \leq m-1$$, then the above numerator is at least $$m(q+1)$$ but strictly less than $$m(q+2)$$, so the quotient evaluates to $$q+1$$, which will be our output.

{% hint style="info" %}
The case $$r=0$$ is equivalent to the case that $$n\%m=0$$ The case $$0<r \leq m-1$$ is equivalent to the case that $$n\%m \neq 0$$
{% endhint %}

Thus, convert the result into \`C\` code \`\`\`c long ceil\_of\_quotient(long n, long m) { return (n + m - 1) / m; } \`\`\`
