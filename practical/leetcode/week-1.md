# Week 1

## Monday

### Problem

{% embed url="https://leetcode.com/problems/plus-one/description/?envType=study-plan-v2&envId=programming-skills" %}

### Solution

#### My Solution

{% code lineNumbers="true" %}
```java
class Solution {
  public int[] plusOne(int[] digits) {
    int length = digits.length;
    int remain = 1;
    for (int i = length - 1; i >= 0; i -= 1) {
      if (digits[i] != 9) {
        digits[i] += 1;
        remain = 0;
        break;
      } else {
        digits[i] = 0;
      }
    }
    if (remain == 0) {
      return digits;
    }
    int[] res = new int[length + 1];
    res[0] = 1;
    for (int i = 1; i < length + 1; i += 1) {
      res[i] = 0;
    }
    return res;
  }
}
```
{% endcode %}

The whole idea is to start from the LSB, iterate all the way till the MSB. And use a variable `remain` to keep track of whether the plus one has been consumed or not.

1. If it is consumed, break the loop and return the result.
2. If not, technically means a new `remain`, which is 1, is generated, we keep iterating, but **remember to change that digit to 0**.

At the end of the iteration, if remain still exist, means we have got a `[1, 0, ... ,0]`.

#### Good Solution

One solution publised on leetcode is more elegant than mine

{% code lineNumbers="true" %}
```java
class Solution {
    public int[] plusOne(int[] digits) {
        for (int i = digits.length - 1; i >= 0; i--) {
            if (digits[i] + 1 != 10) {
                digits[i] += 1;
                return digits;
            }
            digits[i] = 0;
        }
        
        int[] newDigits = new int[digits.length + 1];
        newDigits[0] = 1;
        return newDigits;        
    }
}
```
{% endcode %}

And the tips worth noting down is

1. When create an array using `int[] a = new int[3]`, all elements of that array are initialized to the default value automatically.
2. For Java Array, to get the array length, we call `.length` field. For Java Strings and other objects, we call `.length()` method or `.size()` based on their corresponding API.

## Tuesday

### Problem

{% embed url="https://leetcode.com/problems/to-lower-case/description/?envType=study-plan-v2&envId=programming-skills" %}

### Solution

#### My Solution

This is a trivial problem, as the `String` class in Java provides a method called `.toLowerCase()`, which will return a `String` with all its characters in lower case.

{% code lineNumbers="true" %}
```java
class Solution {
    public String toLowerCase(String s) {
        return s.toLowerCase();
    }
}
```
{% endcode %}

Btw, this problem also invokes us to think the general String Manipulation in Java. For the sake of this, see [#string-manipulation-with-loops](../java-basics-for-dsa.md#string-manipulation-with-loops "mention").

## Wednesday

### Problem

{% embed url="https://leetcode.com/problems/robot-return-to-origin/description/?envType=study-plan-v2&envId=programming-skills" %}

### Solution

#### My Solution

Technically, as this prob is demostrated by Prof Halim during lecture. I would say this is technically Prof Halim's solution. 😂

{% code lineNumbers="true" %}
```java
class Solution {
    public boolean judgeCircle(String moves) {
        int x = 0, y = 0;
        for (char m : moves.toCharArray()) {
            if (m == 'R') {
                ++x;
            } else if (m == 'L') {
                --x;
            } else if (m == 'U') {
                ++y;
            } else {
                --y;
            }
        }
        return (x == 0 && y == 0);
    }
}
```
{% endcode %}

The learning point for this problem is how to do the String Iteration using `.toCharArray()`.

## Thursday

### Problem

{% embed url="https://leetcode.com/problems/matrix-diagonal-sum/description/?envType=study-plan-v2&envId=programming-skills" %}

### Solution

The question is really straight-forward. Just use a for loop to sum the primary and secondary diagnols. And if the width of the square is odd, minus the center point.

{% code lineNumbers="true" %}
```java
class Solution {
    public int diagonalSum(int[][] mat) {
        int len = mat.length;
        int sum = 0;
        for (int i = 0; i < len; i += 1) {
            sum += mat[i][i];       // Primary
            sum += mat[i][len-i-1]; // Secondary
        }
        if (len % 2 == 1) {
            sum -= mat[len / 2][len / 2];
        }
        return sum;
    }
}
```
{% endcode %}

The learning point of this problem is to know how to get the length and width of a 2-D array.

## Friday

### Problem

{% embed url="https://leetcode.com/problems/count-odd-numbers-in-an-interval-range/description/?envType=study-plan-v2&envId=programming-skills" %}

### Solution

#### My Solution

My initial idea is to use a for loop to solve, but the last two test cases I got Time Limit Exceed. And after looking at the hints, I find out this problem is just a **finding pattern** problem in Java. The pattern should be: (denote `high-low+1` as `range`)

1. If range is even, then the number of odd should be `range/2`.
2. Else, meaning that low and high must be both odd or even
   1. If low and high are odd, the number of odd should be `range/2 + 1`
   2. Else, the number of odd should be `range/2`

{% code lineNumbers="true" %}
```java
class Solution {
    public int countOdds(int low, int high) {
        int range = high - low + 1;
        if (range % 2 == 1 && low % 2 == 1) {
            return range / 2 + 1;
        }
        return range / 2;
    }
}
```
{% endcode %}
