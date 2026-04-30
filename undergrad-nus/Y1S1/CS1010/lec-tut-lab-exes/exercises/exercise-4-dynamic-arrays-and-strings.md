# Exercise 4 - Dynamic Arrays and Strings

## Problems

### 4. List

In this question, pay attention to **how to traverse through a list using `size_t` type variable `i`.**

### 8. Search

> A really awesome question to practice "divide-and-conquer" thinking



### 9. Substract

> A complex problem, don't know whether there will be a faster solution

#### My naive solution

Suppose before we do the subtraction, we already know which number is bigger. Let's say `num1` is bigger than `num2`. The first thing we can derive is that the result (subtraction) will be at most the same length with `num1`. Then let's do the main part (substraction)

1. We start from the last digit of each number, denote the last digit for `num1` as `d1` and the last digit for `num2` as `d2`.
   1. If `d1` is bigger than `d2`, we just store their difference into our result's last digit.
   2. Otherwise, we "borrow" one digit from the first nonzero digit in front of `d1` and in this process, if we encounter zero digits, change them to 9.
   3. (Edge cases)
      1. `num2` has run out of digits, that is we have done our substraction. We just move the remaining digits of `num1` to our `res`.
      2. Otherwise, we have reached our very first digit of `num1`, since `num1` is bigger than `num2`, we just set the corresponding digit of `res` to be the difference of the current digit of `num1` and `num2`
2. We move to the digit that is in front of the digit we have checked.

{% hint style="info" %}
Since `num1` is always greater than `num2`, we are sure that we can borrow successfully.
{% endhint %}

## Tips

1. Ask when to use `malloc()`, when to use `calloc()` and which one is recommended in CS1010?
2. A tricky part in [#id-9.-substract](exercise-4-dynamic-arrays-and-strings.md#id-9.-substract "mention") is the difference between `char` and `long` in C, don't mix them in ur code. Otherwise you will experience weird behavior.
3. To avoid the trouble that `size_t` cannot be negative in the `for` loop, we can convert them into `long` explicitly.
