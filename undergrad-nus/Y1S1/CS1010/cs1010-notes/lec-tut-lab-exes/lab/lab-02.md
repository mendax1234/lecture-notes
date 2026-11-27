---
description: Thanks for my Lab Tutor Zhang Puyu!
---

# Lab 02 - Debugging

Slides:

{% file src="../../.gitbook/assets/Lab2.pdf" %}
Lab 2 Slides
{% endfile %}

## Selected Problems from Exercise 1

### Wishful Thinking

Before we talk about the wishful thinking, let's get a quick review of the **Mathematical Induction**

#### Mathematical Induction

Suppose we want to find the value of $$P(n)$$ for any non-negative interger $$n$$, we only need to do two things:

1. Find $$P(0)$$.
2. Find a way to get $$P(k+1)$$ based on the value of $$P(k)$$.

The so called "**Wishful Thinking"** is just the reverse of above!

#### Wishful Thinking

Suppose we already know:

1. How to compute $$P(k+1)$$ using $$P(k)$$;
2. The value of $$P(0)$$, i.e., "the base case" (or **terminating condition**)

Now we want to find $$P(n)$$, and so we can just find $$P(n-1)$$ first. To find $$P(n-1)$$, we just need to find $$P(n-2)$$ first... the cycle goes on until we reach $$P(0)$$ where the recursion terminates.
