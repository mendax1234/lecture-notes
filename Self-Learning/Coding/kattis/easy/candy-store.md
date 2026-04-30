# Candy Store

## Question

{% embed url="https://open.kattis.com/problems/candystore" %}

## Solution

### Idea

This is a normal **decomposition** problem. The basic sub-problem you should solve is:

1. find the second capitalised and initial letter in the name
2. count the apperance of the query in your name array
   1. count = 1, output the index
   2. count > 1, output ambiguous
   3. count = 0, output nobody

These problems are easy to solve, so I won't expand the discussion here.

### Code

{% @github-files/github-code-block url="https://github.com/mendax1234/Coding-Problems/blob/main/kattis/candystore/candystore.c" %}
