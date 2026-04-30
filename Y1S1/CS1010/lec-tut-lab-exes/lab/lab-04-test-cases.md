---
description: Thanks for my Lab Tutor Zhang Puyu
---

# Lab 04 - Test Cases

Slides:

{% file src="../../.gitbook/assets/Lab4.pdf" %}
Lab 4 Slides
{% endfile %}

## Testing

### Some conventions

1. It is **impossible** to enumerate **all test cases**.
2. It is **possible** to divide the test cases **into a few groups**.
3. We can choose to test for all possible **classes of inputs/outputs.**
4. Test for the **boundary cases** according to the constraints given.

### Test Case Design

* Should cover **all** conditional (i.e. for all conditional branch, there exists at least one test case which can enter that branch)
* Should contain at least one case for 0 iteration and at least one case for k iterations where $$k\geq1$$.
* Should contain boundary conditions.

### Example

Design the test cases for the program below:

```c
long find_least_significant_9(long n) {
    long count = 1;
    while (n != 0) {
        if ((n % 10) == 9) {
            return count;
        }
        n /= 10;
        count += 1;
    }
    return 0;
}
```

1. We won't enter the loop `while n != 0`, that means $$n=0$$ and now our output is 0, which is correct.
2. We enter the loop, which means $$n\neq0$$.
   1. We don't enter the conditonal first. So, that means there is **no 9** in `n`
   2. We enter the conditional, which means there is at leasat a **9** in `n`
      1. 9 in the last digit.
      2. 9 in the middle.
      3. Have multiple 9.

{% hint style="info" %}
**Testing Tips:**

1. Test each function seperately (print out the value to be returned and compare it with your expectation!
2. Decompose your program into shorter and simpler functions: simpler code is easier to test!
{% endhint %}

## Exercise 2 Review

### factor.c

Optimization methods:

1. $$n-1\rightarrow  \frac{n}{2}$$ since anything bigger than $$\frac{n}{2}$$ for sure cannot divide $$n$$!
2. If $$i$$ divides $$n$$, then surely $$\frac{n}{i}$$ divides $$n$$ as well. See [#proof-of-2](lab-04-test-cases.md#proof-of-2 "mention")

The above implies that we can count factors in **pairs**.

> Note that if $$i$$ divides $$n$$ and $$i<\sqrt{n}$$, then $$\frac{n}{i}>\sqrt{n}$$

So, the supremum (the least upper bound) is $$\sqrt{n}$$.

We should then deal with the special case where $$n$$ is a perfect square (a perfet square has an odd numbero factors while any other integer has an even number of factors)

<details>

<summary>Proof of 2.</summary>

If $$i$$ divides $$n$$, then we can write $$n=qi$$, where $$q\neq0$$. Manipulate this equation, we know $$q=\frac{n}{i}$$. So, now our problem becomes does $$q$$ divide $$n$$ right? This answer is obvious yes right, since we already know that $$n=qi$$. Proof!

</details>

### collatz.c

{% hint style="info" %}
**When to use `for` or `while`?**

**\`for\` is used under the circumstances where we iterate through consecutive numbers. \`while\` loops are used under the circumstances that we don't iterate through consecutive numbers, which means that we can define a more general condition to "loop through".**
{% endhint %}

### pattern.c

## A Little Extension on prime.c

### Question

How to find **all primes** less than or equal to $$n$$.

### Naive Solution

We iterate every integer from $$n$$ to $$2$$ (even if we exclude all even numbers, there are still $$\frac{n}{2}$$ steps. And checking if each integer is prime takes another $$\sqrt{n}$$ steps in the worst case, so in total there are about $$\frac{n\sqrt{n}}{2}$$ steps -- Slow!

### Optimized Solution

Using an **array**, we can speed up the algorithm. The algorithm we are using is called **Sieve of Eratosthenes**.

Consider all integers from $$2$$ to $$n$$

$$
2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,...,n
$$

Take 2 (which happens to be the smallest prime), and cross out all multiples of 2 in the list.

Take the un-crossed integer (which is 3 in this case), and cross out all muptiples of 3.

Iterate through the list while repeating the above process. We claim that: **each time we come across an un-crossed integer** $$i$$**, it will be a prime.**

<details>

<summary>Why?</summary>

Let's use proof by contracdiction. If it is not a prime, the it must have factors smaller than it. However, during our "cross-out" procedure, all the multiple of the number smaller than it must be crossed out. So, that means this number has been crossed out. Contradiction! So, that means this number we encounter is prime.

</details>

Stop when we encounter **the first integer that is bigger than** $$\sqrt{n}$$. We claim that from this integer onwards, **all un-crossed integers are  prime!**

<details>

<summary>Why?</summary>

Use proof by contracdiction also. If this number (**n**) is not a prime. That means **n** has factors that are not itself and 1. And one of its factor must be smaller than or equal to its square root. However, we have crossed out all the numbers that are multiple of that square root since it's smaller the **n**. So, this means this number is already crossed out. Contradiction!

</details>

Number of operations in the worst case: upper-bounded by $$nlog(log(n))$$ (fewer than $$n\sqrt{n}$$).

<details>

<summary>Proof</summary>

The proof is far beyond the scope of this course and contains much mathematical knowledge. I haven't digged deep into it but below are some useful resources:

1. [https://en.wikipedia.org/wiki/Divergence\_of\_the\_sum\_of\_the\_reciprocals\_of\_the\_primes#Proof\_that\_the\_series\_exhibits\_log-log\_growth](https://en.wikipedia.org/wiki/Divergence_of_the_sum_of_the_reciprocals_of_the_primes#Proof_that_the_series_exhibits_log-log_growth)
2. [https://github.com/Z-Puyu/lecture-notes/blob/MA2214-Combinatorics-and-Graphs-I/Combinatorics%20and%20Graphs%20I.pdf](https://github.com/Z-Puyu/lecture-notes/blob/MA2214-Combinatorics-and-Graphs-I/Combinatorics%20and%20Graphs%20I.pdf)(Page 30)

</details>
