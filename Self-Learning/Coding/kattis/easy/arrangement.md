# Arrangement

## Question

{% embed url="https://open.kattis.com/problems/upprodun" %}

## Solution

### Idea

This problem mainly tests the ability to **find pattern**. The pattern I have found is below:

1. The difference between the teams in each room is exactly 1.
2. The number of rooms that has exactly one more team is m % n.
3. The base number of team in the room is m / n. Then the other choice is m / n + 1 exactly.

### Code

{% @github-files/github-code-block url="https://github.com/mendax1234/Coding-Problems/blob/main/kattis/upprodun/upprodun.c" %}
