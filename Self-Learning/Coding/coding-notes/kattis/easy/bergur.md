# Bergur\*

## Question

{% embed url="https://open.kattis.com/problems/bergur" %}

## Solution

### Idea

The goal is to minimize the total cost while processing the list. The cost is determined by the value of the minimum element in each segment multiplied by the number of elements in that segment. The **greedy strategy** works as follows:

1. **Identify the Local Minimum (Low Point):**
   * At each step, find the smallest element in the current range of the list (`[start, end]`).
   * This ensures that the cost for the current segment is minimized, as the lowest element dominates the cost for that segment.
2. **Calculate the Cost for the Current Segment:**
   * Use the identified local minimum to calculate the cost for the current segment, which includes all elements from `start` to the local minimum index (`low_pt_index`).
   * The cost for this segment is: $$\text{cost} = \text{min\_value} \times (\text{low\_pt\_index} - \text{start} + 1)$$
3. **Update the Remaining Range:**
   * After processing the current segment, move `start` to the next index after the local minimum (`low_pt_index + 1`).
   * Repeat the process for the remaining range of the list.
4. **Stop When the Entire List is Processed:**
   * The algorithm terminates when the `start` index exceeds `end`, meaning all elements have been processed.

### Code

{% @github-files/github-code-block url="https://github.com/mendax1234/Coding-Problems/blob/main/kattis/bergur/bergur.c" %}
