# Basketball One-on-One

## Question

{% embed url="https://open.kattis.com/problems/basketballoneonone" %}

## Solution

### Idea

To solve this question trivially, you can

1. Iterate through the input and record the score for A and score for B
2. And after exiting the loop, check whose score is higher

I haven't tried this solution so I am not sure whether it will work or not. But this solution is **not elegant!** And the more elegant or faster way is to add the following steps to our original solution:

3. If `score_a >= 11 && score_a - score_b >= 2`, that means A wins, print the result, return.
4. If `score_b >= 11 && score_b - score_a >= 2`, that means B wins, print the result, return.

Note that steps 3 and 4 are not in `if/else` relation, that means you need to use two `if` to judge.

### Code

{% @github-files/github-code-block url="https://github.com/mendax1234/Coding-Problems/blob/main/kattis/basketballoneonone/basketballoneonone.c" %}
