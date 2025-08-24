# Week 2 Sorting

## Lec

In Java, the sorting is implemented in [`Collections`](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html). An example which uses this feature to solve a problem is as follows,

{% embed url="https://open.kattis.com/problems/nothanks" %}

Basically, we can create an array using `ArrayList` and then pass that array as the argument of the [`Collections.sort()`](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#sort-java.util.List-), after that the array will be sorted ascending order, according to the natural order of its elements.

{% code lineNumbers="true" %}
```java
public class nothanks {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    int n = sc.nextInt();
    sc.nextLine();
    ArrayList<Integer> card = new ArrayList<>();
    for (int i = 0; i < n; i += 1) {
      card.add(sc.nextInt());
    }
    Collections.sort(card);
    int ans = card.get(0);
    for (int i = 1; i < n; i += 1) {
      if (card.get(i - 1) + 1 != card.get(i)) {
        ans += card.get(i);
      }
    }
    System.out.println(ans);
  }
}
```
{% endcode %}

## Thursday

### Problem

{% embed url="https://leetcode.com/problems/shortest-unsorted-continuous-subarray/description/" %}

### Solution

#### Slow Solution

Basically, it sorts the array first. And compare the sorted array with the unsorted one, the first unmatch element's position is denoted by `start`, and the last unmatch element's position is denoted by `end`, then the length will be `end - start + 1`.

{% code lineNumbers="true" %}
```java
class Solution {
    public int findUnsortedSubarray(int[] nums) {
        ArrayList<Integer> list = new ArrayList<>();
        // Convert array to ArrayList
        for (int num : nums) {
            list.add(num);
        }
        Collections.sort(list);
        int length = nums.length;
        int start = 0;
        int end = 0;

        // Find the first unmatch
        for (int i = 0; i < length; i += 1) {
            if (nums[i] != list.get(i)) {
                start = i;
                break;
            }
        }

        // Find the last unmatch
        for (int i = length - 1; i >= 0; i -= 1) {
            if (nums[i] != list.get(i)) {
                end = i;
                break;
            }
        }

        if ((length == 1) || (start == 0 && end == 0)) {
            return 0;
        }
        return end - start + 1;
    }
}
```
{% endcode %}

## Friday

### Problem

{% embed url="https://leetcode.com/problems/average-salary-excluding-the-minimum-and-maximum-salary/description/?envType=study-plan-v2&envId=programming-skills" %}

### Solution

This is an easy problem. No discussion.
