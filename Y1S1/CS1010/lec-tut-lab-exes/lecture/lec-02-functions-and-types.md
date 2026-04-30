# Lec 02 - Functions and Types

Slides:

{% embed url="https://nus-cs1010.github.io/2425-s1/slides/cs1010-lec02.html#1" %}
Lecture Slides
{% endembed %}

## Functions

Thinking in terms of functions has the advantage: given a function, we only need to worry about **what** it does, but not **how** it is done. We can treat a function as a black box with well-defined behavior -- given one or more inputs, it will produce a value satisfying certain conditions, i.e., it solves a computational problem.

In fact, a C program is just a collection of functions calling each other.

### Writing Good Functions

To make the functions that you write reusable and composable, you should create your functions in such a way that it **does one thing and one thing only**.

## Types

Recall from Unit 1 that machine code and data manipulated by machine code are all stored as a sequence of binaries, 1s and 0s, in the memory. Each unit of either 1 or 0 is known as a _bit_. And 8 bits form a _byte_.

Remember from [lec-01-computational-problem-solving.md](lec-01-computational-problem-solving.md "mention") that the value of a variable is stored in a memory location as a sequence of bits. These bits have to be interpreted by the machine code to be given a meaning. Does a sequence of 1s and 0s represent an integer? A pixel of an image? A sound sample in an audio clip? A month? As a programmer, we have to tag the variable with its _type_, so that the machine code knows how to interpret the sequence of bits. In addition, the type also tells the machine code, how many bits "belong" to this variable. The number of bits of a type is also known as the _size_ of a type.

### Real Numbers

If we only expect a variable to hold an integer, we should not choose a type that represents real numbers.

### Type Declaration

In C, we have to associate _every_ variable with a type, and once a variable is _declared_ with a type, the type cannot be changed.

This behavior is known as **static typing**. Some programming languages, such as Javascript and Python, are dynamically typed. The type of a variable may change depending on the value the variable is assigned to.
