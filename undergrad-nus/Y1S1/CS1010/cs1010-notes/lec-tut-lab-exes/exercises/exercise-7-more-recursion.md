# Exercise 7 - More Recursion

## Problems

### 1. Fill

The troublesome part of this question is the creation and read of 3-D array. Also, understand this question is a bit hard. (Refer to this [video](https://www.youtube.com/watch?v=VuiXOc81UDM) about "flood filling algorithm" for clearer understanding)

### 2. Maze

The main take-away it to use another array which is the same size as the input 2-D char array to store the explored grids. This idea of `unvisited_array` is used very often!

### 3. Walk

This is a classic dynamic programming problem. This one is just a easy version which only needs to build the table according to the "upper" and "left"

### 4. Sudoku

> A variation problem of [#n-queens](../lecture/lec-10-more-recursion/#n-queens "mention") (find one solution version).

This enhances our wishful thinking in deciding the return **type** of our recursion function. Let's see how.

In this question, we are supposed to find **one solution**. So, our recursive function should return whether we have found a solution or not (This is an obvious **boolean** question). Now, use wishful thinking, after some try on the current solution and if there is a solution to the smaller problem, we will return **TRUE**, which means we have found a solution.

{% hint style="info" %}
In this exercise, the trickest part is to print the puzzles. There are two things to keep in mind:

1. The moment you "update" every cell, print the puzzles ("update" here doesn't confirm the solution is correct, we may still need the backtracking)
2. Print the result at last after you find the solution
{% endhint %}

## Tips

1. When writing recursion in 2-D array, always think about how would you not enter an "infinite loop". This is usually down in your **condition to enter the recursion**. Some common methods are:
   1. mark the attendance of the "cell" you have updated. (a.k.a build an `unvisited_array`)
   2. there is some initial "constant" that you can compare to
