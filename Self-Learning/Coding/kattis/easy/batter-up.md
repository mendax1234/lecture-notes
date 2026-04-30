# Batter Up

## Question

{% embed url="https://open.kattis.com/problems/batterup" %}

## Solution

### Idea

To translate the rule of this game into readable language, it's nothing but:

1. If it at-bats is _walk_, we don't count them in our actual numbers and sum
2. Else, we increment actual number by one and add its value to sum

The final result: avg should be sum divided by the actual num, which should be a `double`.

### Code

{% @github-files/github-code-block url="https://github.com/mendax1234/Coding-Problems/blob/main/kattis/batterup/batterup.c" %}
