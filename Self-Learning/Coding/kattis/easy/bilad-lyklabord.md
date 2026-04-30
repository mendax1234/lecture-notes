# Bilað Lyklaborð

## Question

{% embed url="https://open.kattis.com/problems/biladlyklabord" %}

## Solution

### Idea

**Time Complexity**: $$O(n)$$

This is a **string manipulation problem**, a basic but efficient idea is to use two variables `prev` and `cur` to store the previous printed character and current character respectively.

1. when current character is **not equal to** the previous printed character, we print the current character and update the previous printed character.
2. otherwise, skip one time

### Code

{% @github-files/github-code-block url="https://github.com/mendax1234/Coding-Problems/blob/main/kattis/biladlyklabord/biladlyklabord.c" %}
