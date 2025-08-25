# Week 3

## Monday

### Problem

{% embed url="https://leetcode.com/problems/kids-with-the-greatest-number-of-candies/description/?envType=study-plan-v2&envId=leetcode-75" %}

### Solution

The high level algorithm is:

1. Find the maximum element in the array (linear, should be $$O(n)$$)
2. Iterate through the array to decide whether each element + extraCandies will be greater than or equal to the maximum element found in above. (linear, should be $$O(n)$$  too)

The overall complexity is thus $$O(n)$$.

{% code lineNumbers="true" %}
```java
class Solution {
    public List<Boolean> kidsWithCandies(int[] candies, int extraCandies) {
        int[] copy = candies.clone();
        // Find maximum
        Arrays.sort(candies);
        int max = candies[candies.length - 1];

        // Iterate through the list
        List<Boolean> list = new ArrayList<>();
        for (int i = 0; i < candies.length; i++) {
            if (copy[i] + extraCandies >= max) {
                list.add(true);
            } else {
                list.add(false);
            }
        }

        return list;
    }
}
```
{% endcode %}

{% hint style="info" %}
Here I am lame so just sort the list first LOL.
{% endhint %}
