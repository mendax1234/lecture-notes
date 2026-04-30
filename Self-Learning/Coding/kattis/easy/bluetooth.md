# Bluetooth\*

## Question

{% embed url="https://open.kattis.com/problems/bluetooth" %}

## Solution

{% hint style="warning" %}
**Future Improvement**: Improve the logic of the solution written at first since it is pretty complicated. Also pay attention to whether can improve the implementation by using a more advanced language, like C++/Java.
{% endhint %}

### Idea

**Time Complexity**: $$O(1)$$ or $$O(n)$$

My idea is to create an `long` array to store the health status for all 32 teeth, `0` for healthy, `1` for missing and `2` for blue.

Then, iterate through this array and update the health status of `left_upper`, `left_lower`, `right_upper` and `right_lower`.

Lastly, we use these four variables to get the overal health status, `left_okie` and `right_okie`.

### Code

#### C

{% @github-files/github-code-block url="https://github.com/nus-cs1010/libcs1010/blob/master/src/cs1010.c" %}
