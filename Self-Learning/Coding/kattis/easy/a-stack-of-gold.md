# A Stack of Gold

## Question

{% embed url="https://open.kattis.com/problems/astackofgold" %}

## Solution

### Idea

This problem is a bit interesting. The solution is implemented in $$O(N)$$. Here we just assume all the stacks are tungsten. And then we loop through every stack (cuz we take different number of coins from every stack). And during each iteration, we add the number of coins times the difference between the weight of gold coins and tungsten coins. If the weight is equal to the input weight. Then this stack is what we want.

### Code

{% @github-files/github-code-block url="https://github.com/mendax1234/Coding-Problems/blob/main/kattis/astackofgold/astackofgold.c" %}
