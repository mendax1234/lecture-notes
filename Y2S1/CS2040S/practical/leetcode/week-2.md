# Week 2

## Monday

### Problem

{% embed url="https://leetcode.com/problems/monotonic-array/?envType=study-plan-v2&envId=programming-skills" %}

### Solution

#### My Solution

I basically use two for loops to decide whether the `nums[]` is monotonic increase or decrease. This seems a bit tedious.

{% code lineNumbers="true" %}
```java
class Solution {
    public boolean isMonotonic(int[] nums) {
        int length = nums.length;
        if (length == 1) {
            return true;
        }
        // MonoInc
        boolean isMonoInc = true;
        for (int i = 0 ; i < length - 1; i++) {
            if (nums[i] > nums[i+1]) {
                isMonoInc = false;
            }
        }
        if (isMonoInc) {
            return true;
        }

        // MonoDec
        boolean isMonoDec = true;
        for (int i = 0 ; i < length - 1; i++) {
            if (nums[i] < nums[i+1]) {
                isMonoDec = false;
            }
        }
        if (isMonoDec) {
            return true;
        }

        // Not MonoInc or MonoDec
        return false;
    }
}
```
{% endcode %}

#### Good Solution

It uses two indicators `isInc` and `isDec` to mark whether the array is either increasing or decreasing, thus only need one loop can solve

{% code lineNumbers="true" %}
```java
class Solution {
    public boolean isMonotonic(int[] nums) {
        int n = nums.length;
        if (n == 1) return true;

        boolean isInc = true;
        boolean isDec = true;

        for (int i = 1; i < n; i++) {
            if (!isInc && !isDec) {
                return false;
            }

            if (nums[i] < nums[i - 1]) {
                isInc = false;
            }
            if (nums[i] > nums[i - 1]) {
                isDec = false;
            }
        }

        return isInc || isDec;        
    }
}
```
{% endcode %}

## Tuesday

### Problem

{% embed url="https://leetcode.com/problems/spiral-matrix/description/?envType=study-plan-v2&envId=programming-skills" %}

### Solution

The key of this solution is to notice the rule of transition for `dx` and `dy`, which is everything you are about to go out of boundary or visit the visited cell, you transit `dx=-dy, dy=dx`. (`y` is the column positive and `x` is the row position)

{% code lineNumbers="true" %}
```java
class Solution {
  public List<Integer> spiralOrder(int[][] matrix) {
    int rows = matrix.length;
    int cols = matrix[0].length;
    List<Integer> res = new ArrayList<Integer>();
    int cur_x = 0;
    int cur_y = 0;
    int dx = 1;
    int dy = 0;
    for (int i = 0; i < rows * cols; i += 1) {
      // add current
      res.add(matrix[cur_y][cur_x]);
      matrix[cur_y][cur_x] = -101;

      // transition dx, dy
      if (!(0 <= cur_x + dx && cur_x + dx < cols && 0 <= cur_y + dy && cur_y + dy < rows)
          || matrix[cur_y + dy][cur_x + dx] == -101) {
        int temp = dx;
        dx = -dy;
        dy = temp;
      }

      // update cur
      cur_x += dx;
      cur_y += dy;
    }
    return res;
  }
}
```
{% endcode %}

## Wednesday\*

### Problem

{% embed url="https://leetcode.com/problems/robot-bounded-in-circle/description/?envType=study-plan-v2&envId=programming-skills" %}

### Solution

Today's problem is interesting. Bascially, we need to learn that whenever we are dealing with robot-centered problem, use a fixed 2-D map to record the available four steps!

And for this problem, we just need to find the final vector, if it is not facing north, or the robot stays at the origin, then the robot will be in a circle.

{% code lineNumbers="true" %}
```java
class Solution {
    public boolean isRobotBounded(String instructions) {
        int x = 0;
        int y = 0;
        int i = 0;
        int[][] d = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
        for (char s : instructions.toCharArray()) {
            if (s == 'R') {
                i = (i + 1) % 4;
            } else if (s == 'L') {
                i = (i + 3) % 4;
            } else {
                x += d[i][0];
                y += d[i][1];
            }
        }
        return (x == 0 && y == 0) || i > 0;
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
