# Exercise 8 - Struct

## Problems

### Digits

> The six-star (hardest) question fro CS1010. (Supposed to be)

This question is nothing but tedius. Break the big problem into small problems and solve it. The trickest two parts are as follows:

#### Get the k-nearest neighbor

This involves the technique of **searching and sorting.** Since the list is **unsorted**, so we just need to use linear search to find the smallest 5 neighbors. The time complexity is $$O(kN)$$. When doing linear search, follow strictly to what [the question](https://nus-cs1010.github.io/2425-s1/exercises/ex08.html#breaking-ties) states to write your conditions.

#### Predict the number

To predict the number using the k-nearest neighbors you have found, there is nothing special, just follow strictly what the [question](https://nus-cs1010.github.io/2425-s1/exercises/ex08.html#breaking-ties) states to write the conditions.
