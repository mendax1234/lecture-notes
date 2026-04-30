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

## Tuesday

### Problem

{% embed url="https://leetcode.com/problems/merge-strings-alternately/description/?envType=study-plan-v2&envId=leetcode-75" %}

### Solution

This problem is very similar to the one Prof. Halim live-coded on Lec 02. The high level algorithm is:

1. Use two pointers (`pt1` and `pt2`) to mark char dealth with in `word1` and `word2`
2. while pt1 < length of word1 and pt2 < length of word 2, build the result string alternatively
3. while pt1 < length of word1, append the remaining chars of word1 into res string
4. while pt2 < length of word2, append the remaining chars of word2 into res string
5. return res string

The overall time complexity is $$O(m+n)$$.

{% code lineNumbers="true" %}
```java
class Solution {
    public String mergeAlternately(String word1, String word2) {
        int pt1 = 0;
        int pt2 = 0;
        boolean dir = true;
        int word1Len = word1.length();
        int word2Len = word2.length();
        StringBuilder res = new StringBuilder();
        while (pt1 < word1Len && pt2 < word2Len) {
            if (dir) {
                res.append(word1.charAt(pt1));
                pt1++;
            } else {
                res.append(word2.charAt(pt2));
                pt2++;
            }
            dir = !dir;
        }
        // word 1 not ending
        while (pt1 < word1Len) {
            res.append(word1.charAt(pt1));
            pt1++;
        }
        // word 2 not ending
        while (pt2 < word2Len) {
            res.append(word2.charAt(pt2));
            pt2++;
        }
        return res.toString();
    }
}
```
{% endcode %}

## Wednesday

### Problem

{% embed url="https://leetcode.com/problems/sort-colors/description/?envType=study-plan-v2&envId=top-100-liked" %}

### Solution

This problem is discussed during the [Lec 03](../../lec-lab/lec-03-more-sorting.md). Can use library sort, idea of quick sort and counting sort to solve it.

## Thursday

### Problem

{% embed url="https://leetcode.com/problems/find-the-highest-altitude/description/?envType=study-plan-v2&envId=leetcode-75" %}

### Solution

This is a classic **prefix sum** problem. We use a `cur` variable to record the prefix sum and update the `max` during each iteration.

{% code lineNumbers="true" %}
```java
class Solution {
    public int largestAltitude(int[] gain) {
        int cur = 0;
        int max = 0;
        for (int g : gain) {
            cur += g;
            max = Math.max(cur, max);
        }
        return max;
    }
}
```
{% endcode %}
