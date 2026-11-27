# Bus Assignment

## Question

{% embed url="https://open.kattis.com/problems/busassignment" %}

## Solution

### Idea

As the first [ICPC question](https://open.kattis.com/problem-sources/2024%20ICPC%20Pacific%20Northwest%20Regional%20\(November%2016%2C%202024\)) I tried, this question is not that hard. Basically, the idea is:

1. use two variables `cur` (the **current** number of passengers on the bus) and `max` (the **maximum** number of passengers on the bus).
2. Every time you arrived at a bus stop, update the `cur` based on the number of passengers get on and off. After that, compare `cur` with `max`, if `cur` is bigger, then `max=cur`.

### Code

{% @github-files/github-code-block url="https://github.com/mendax1234/Coding-Problems/blob/main/kattis/busassignment/busassignment.c" %}
