# Coffee Cup Combo

## Question

{% embed url="https://open.kattis.com/problems/coffeecupcombo" %}

## Solution

### Idea

This is an easy question. The idea is to use two variables to store the coffee left after each lecture and the number of lectures Jonna can stay awake.

1. If you encounter a coffee machine, update the `left` to be 2 and increment `sum` by 1.
2. Otherwise, if there is coffee left, decrement `left` by 1 and increment `sum` by 1.

### Code

{% @github-files/github-code-block url="https://github.com/mendax1234/Coding-Problems/blob/main/kattis/coffeecupcombo/coffeecupcombo.c" %}
