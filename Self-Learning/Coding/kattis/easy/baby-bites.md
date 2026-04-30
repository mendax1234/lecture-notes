# Baby Bites

## Question

{% embed url="https://open.kattis.com/problems/babybites" %}

## Solution

### Idea

**Time Complexity:** $$O(n)$$

The solution is easy,

1. Iterate through the word in the input and check whether the word is in the correct index or not
2. If the word is "mumble", just skip it

But the tricky part in C implementation is you need to know how to use `atol()`, which will convert a "number" string to a `long`.

### Code

{% @github-files/github-code-block url="https://github.com/mendax1234/Coding-Problems/blob/main/kattis/babybites/babybites.c" %}
