# Lec 03 - Basic C Programming

Slides:

{% embed url="https://nus-cs1010.github.io/2425-s1/slides/cs1010-lec03.html#1" %}
Lecture Slides
{% endembed %}

## First C Program

### Statement

A _statement_ is a unit in a programming language that expresses either a command to be executed or declares a new variable or function.

### The `main()` function

In C, `main()` is the _entry point_ to the program. It is where the operating system will begin to execute the program.

`main` returns an integer of type `int` to the operating system, to signal to the operating system whether the program exits successfully or not. In this case, we always return `0` (success) assuming that nothing goes wrong for simplicity.

{% hint style="info" %}
In modern C, the `main` function always returns 0 when it exits. So, we will skip this statement `return 0;` from the `main` function from now on.
{% endhint %}

### Variable Declaration

To declare a vairable, we use a _declaration statement_ looks like as follows:

{% code lineNumbers="true" %}
```c
int hypotenuse_square;
```
{% endcode %}

_All variables must be declared with its corresponding type before used in C_. Indicating the type during declaration lets the computing device running the program knows how much memory should be reserved for the value of this variable, and subsequently, how to interpret the binary sequence stored associated with this variable. (See [#types](lec-02-functions-and-types.md#types "mention") for deeper understanding)

A variable is only visible within the innermost block that encloses the declaration, as specified by the `{` and `}`.

### Compilation Error vs. Run-Time Error

Errors that occur during compiling is called "compilation error". In constrast, errors that occur during execution of a program is called a "run-time error".

However, in real life in C, you may encounter the following **three** errors/warnings:

#### Compilation Warning

A **compilation warning** is a message provided by the compiler that indicates potential issues in the code, but **does not prevent** the code from compiling or running.

* **Example**: Unused variables or deprecated functions. While the program can still run, warnings suggest areas that might cause problems or be improved in the future.
* **Impact**: The code still compiles and runs, but you should address warnings for better code quality.

#### Compilation Error

A **compilation error** is a mistake in the code that **prevents** the program from being compiled into an executable.

* **Example**: Syntax errors (like missing semicolons, undeclared variables, or incorrect function calls). The compiler cannot generate machine code due to these errors.
* **Impact**: The program doesn't compile at all. You need to fix the error before the code can be run.

#### Run-Time Error

A **run-time error** occurs while the program is running and leads to abnormal behavior or crashes.

* **Example**: A **segmentation fault** occurs when the program tries to access an illegal memory location, such as dereferencing a null pointer or accessing out-of-bounds array indices.
* **Impact**: The program crashes or behaves unexpectedly while running, and the error needs to be fixed to ensure stability.

## CS1010 I/O

### Standard Input and Standard Output

In Unix-flavored operating systems, the input is read from an abstract channel called the _standard input_ or `stdin` for short, and an output is sent to an abstract channel called the _standard output_ or `stdout` for short.

The fact that these channels are abstract is a powerful concept -- when we write our code, we do not have to worry about where the inputs come from and where the outputs go to. It will depend on how the user runs our program. Thus, it allows the users of our program the flexibility to control where the data comes from or goes.

For instance, the standard input, by default, reads from the keyboard. But the user can choose to read from a file, using the redirection `<` operator _from the command line_ or the output of another process, using the pipe `|` operator _from the command line_. Similarly, the standard output, by default, writes to the terminal. But the user can choose to write to a file using the redirection `>` operator _on the command line_ or to the input of another process, using the pipe `|` operator, again, on the command line when invoking the program. You will see how cool these are later. But for C programming, it suffices to know for now that we only need to read from `stdin` and write to `stdout` in our code, and we let the users decide where they come from / go to.

## Arithmetic Operations

### Operators

In C, The ~~module~~ remainder operator `%` works only on **integer types**.

### The % Operator <a href="#the-operator" id="the-operator"></a>

It is commonly misunderstood that the `%` operator is equivalent to the modulo operation in number theory, where the operator always gives a **positive** remainder.

This equivalence holds when the two operands are positive. E.g., `9 % 4` gives `1` as the answer. But, when one of the operands is **negative**, the `%` operator may return a **negative** number.

The `%` operator in C is defined as follows: `x % n` is equivalent to `x - ((x / n) * n)` (where `/` is the integer division operator).

For instance, `-9 % 4` is evaluated as `-9 - ((-9 / 4) * 4)` which is `-9 - (-8)`, and gives `-1`.

On the other hand, `9 % -4` is evaluated as `9 - ((9 / -4) * -4)`, which gives `9 - 8`, or `1`.

To avoid confusion between the `%` operator in C and the modulo operation in number theory, we will call the `%` as the _remainder_ operator.
