# Numbers and operators

At this point in the course, we discuss math in JavaScript — how we can use [operators](https://developer.mozilla.org/en-US/docs/Glossary/Operator) and other features to successfully manipulate numbers to do our bidding.

## Numbers

As we have seen before, unlike some other programming languages, JavaScript only has one data type for numbers, both integers and decimals — you guessed it, [`Number`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number). This means that whatever type of numbers you are dealing with in JavaScript, you handle them in exactly the same way.

### Useful Number methods

The [`Number`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number) object, an **instance** of which represents all standard numbers you'll use in your JavaScript, has a number of useful methods available on it for you to manipulate numbers. You can find them all [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number#instance_methods).

{% hint style="info" %}
Note that here we mention the **instance** **methods**, which is different from [**static methods**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number#static_methods)**.** However, I believe this is a concept in OOP (Object Oriented Programming), so I won't talk about it in detail. But you can find the difference by seeing the document on [Numbers](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number).
{% endhint %}

### Converting to `number` data types

Sometimes you might end up with a number that is stored as a string type, which makes it difficult to perform calculations with it. This most commonly happens when data is entered into a [form](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Forms) input, and the [input type is text](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/text). There is a way to solve this problem — passing the string value into the [`Number()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/Number) constructor to return a number version of the same value. For example,

{% code lineNumbers="true" %}
```javascript
let myNumber = "74";
myNumber = Number(myNumber) + 3;
```
{% endcode %}

The result is then 77, as expected.

## Operators

### Arithmetic operators

Arithmetic operators are used for performing mathematical calculations in JavaScript:

| Operator | Name                                | Purpose                                                                                                                           | Example                                                               |
| -------- | ----------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------- |
| `+`      | Addition                            | Adds two numbers together.                                                                                                        | `6 + 9`                                                               |
| `-`      | Subtraction                         | Subtracts the right number from the left.                                                                                         | `20 - 15`                                                             |
| `*`      | Multiplication                      | Multiplies two numbers together.                                                                                                  | `3 * 7`                                                               |
| `/`      | Division                            | Divides the left number by the right.                                                                                             | `10 / 5`                                                              |
| `%`      | Remainder (sometimes called modulo) | Returns the remainder left over after you've divided the left number into a number of integer portions equal to the right number. | `8 % 3` (returns 2, as three goes into 8 twice, leaving 2 left over). |
| `**`     | Exponent                            | Raises a `base` number to the `exponent` power, that is, the `base` number multiplied by itself, `exponent` times.                | `5 ** 2` (returns `25`, which is the same as `5 * 5`).                |

{% hint style="info" %}
You'll sometimes see numbers involved in arithmetic referred to as [operands](https://developer.mozilla.org/en-US/docs/Glossary/Operand).
{% endhint %}

{% hint style="info" %}
You may sometimes see exponents expressed using the older [`Math.pow()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/pow) method, which works in a very similar way. For example, in `Math.pow(7, 3)`, `7` is the base and `3` is the exponent, so the result of the expression is `343`. `Math.pow(7, 3)` is equivalent to `7**3`.
{% endhint %}

{% hint style="info" %}
A full list of all JavaScript operators and their precedence can be found in [Operator precedence](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_precedence).
{% endhint %}

### Increment and decrement operators

Sometimes you'll want to repeatedly add or subtract one to or from a numeric variable value. This can be conveniently done using the increment (`++`) and decrement (`--`) operators. We used `++` in our "Guess the number" game back in our [first splash into JavaScript](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Scripting/A_first_splash) article, when we added 1 to our `guessCount` variable to keep track of how many guesses the user has left after each turn.

{% code lineNumbers="true" %}
```javascript
guessCount++;
```
{% endcode %}

This is similar to the `++` and — operator in C, but as a NUS CS1010 learner, it is not recommended for beginners to use `++` and — [for some reasons](https://nus-cs1010.github.io/2425-s1/notes/07-arithmetic-ops.html?h=increment#avoid-increment-decrement-operator). Instead, `+= 1` and `-= 1` are recommended.

### Assignment operators

Assignment operators are operators that assign a value to a variable. We have already used the most basic one, `=`, loads of times — it assigns the variable on the left the value stated on the right:

{% code lineNumbers="true" %}
```javascript
let x = 3; // x contains the value 3
let y = 4; // y contains the value 4
x = y; // x now contains the same value y contains, 4
```
{% endcode %}

But there are some more complex types, which provide useful shortcuts to keep your code neater and more efficient. The most common are listed below:

| Operator | Name                      | Purpose                                                                                                  | Example   | Shortcut for |
| -------- | ------------------------- | -------------------------------------------------------------------------------------------------------- | --------- | ------------ |
| `+=`     | Addition assignment       | Adds the value on the right to the variable value on the left, then returns the new variable value       | `x += 4;` | `x = x + 4;` |
| `-=`     | Subtraction assignment    | Subtracts the value on the right from the variable value on the left, and returns the new variable value | `x -= 3;` | `x = x - 3;` |
| `*=`     | Multiplication assignment | Multiplies the variable value on the left by the value on the right, and returns the new variable value  | `x *= 3;` | `x = x * 3;` |
| `/=`     | Division assignment       | Divides the variable value on the left by the value on the right, and returns the new variable value     | `x /= 5;` | `x = x / 5;` |

Note that you can quite happily use other **variables on the right-hand side** of each expression, for example:

{% code lineNumbers="true" %}
```javascript
let x = 3; // x contains the value 3
let y = 4; // y contains the value 4
x *= y; // x now contains the value 12
```
{% endcode %}

### Comparison operators

Sometimes we will want to run `true/false` tests, then act accordingly depending on the result of that test — to do this we use **comparison operators**.

| Operator | Name                     | Purpose                                                                      | Example       |
| -------- | ------------------------ | ---------------------------------------------------------------------------- | ------------- |
| `===`    | Strict equality          | Tests whether the left and right values are identical to one another         | `5 === 2 + 4` |
| `!==`    | Strict-non-equality      | Tests whether the left and right values are **not** identical to one another | `5 !== 2 + 3` |
| `<`      | Less than                | Tests whether the left value is smaller than the right one.                  | `10 < 6`      |
| `>`      | Greater than             | Tests whether the left value is greater than the right one.                  | `10 > 20`     |
| `<=`     | Less than or equal to    | Tests whether the left value is smaller than or equal to the right one.      | `3 <= 2`      |
| `>=`     | Greater than or equal to | Tests whether the left value is greater than or equal to the right one.      | `5 >= 4`      |

{% hint style="info" %}
You may see some people using `==` and `!=` in their tests for equality and non-equality. These are valid operators in JavaScript, but they differ from `===`/`!==`. The former versions test whether the **values are the same** but **not** whether the **values' datatypes are the same**. The latter, strict versions test **the equality of both the values and their datatypes**. **The strict versions tend to result in fewer errors, so we recommend you use them**.
{% endhint %}

### Logical operators

Similar to many other programming languages, JS has **three** logical operators: `&&` (AND), `||` (OR) and `!` (NOT).

* `&&` — AND; allows you to chain together two or more expressions so that **all of them** have to individually evaluate to `true` for the whole expression to return `true`.
* `||` — OR; allows you to chain together two or more expressions so that **one or more of them** have to individually evaluate to `true` for the whole expression to return `true`.
* `!` — NOT; allows you to **negate** an expression. `true` after negation will be `false`; `false` after negation will be `true`.
